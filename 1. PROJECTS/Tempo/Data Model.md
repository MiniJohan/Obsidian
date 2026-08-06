# Tempo — Data Model

> Supabase (Postgres) schema. All tables scoped to `user_id` via RLS.

---

## Tables

### `templates`
Reusable named schedule patterns.

```sql
templates (
  id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     uuid REFERENCES auth.users NOT NULL,
  name        text NOT NULL,
  color       text DEFAULT '#6C63FF',  -- accent color for visual identity
  created_at  timestamptz DEFAULT now()
)
```

---

### `blocks`
Time slots that belong to a template.

```sql
blocks (
  id           uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  template_id  uuid REFERENCES templates(id) ON DELETE CASCADE,
  title        text NOT NULL,
  start_time   time NOT NULL,   -- e.g. '09:00'
  end_time     time NOT NULL,   -- e.g. '10:30'
  notes        text,
  order_index  int DEFAULT 0    -- explicit ordering
)
```

**Notes:**
- Times are wall-clock (no timezone) — user's local time
- `order_index` is a fallback; primary sort is `start_time`
- Blocks cannot span midnight (v1 constraint)

---

### `schedule_rules`
Defines when a template is applied — once or on a repeat pattern.

```sql
schedule_rules (
  id             uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id        uuid REFERENCES auth.users NOT NULL,
  template_id    uuid REFERENCES templates(id) ON DELETE SET NULL,
  label          text,          -- optional name, e.g. "Monday Routine"
  start_date     date NOT NULL,
  end_date       date,          -- NULL = no end (forever)
  repeat_type    text NOT NULL, -- 'once' | 'daily' | 'weekly' | 'monthly' | 'specific'
  weekdays       int[],         -- [0–6] Mon=0, used when repeat_type = 'weekly'
  specific_dates date[],        -- used when repeat_type = 'specific'
  created_at     timestamptz DEFAULT now()
)
```

**repeat_type values:**

| Value | Meaning |
|---|---|
| `once` | Applies to start_date only |
| `daily` | Every day from start_date to end_date |
| `weekly` | On weekdays[] within date range |
| `monthly` | On the same day-of-month as start_date |
| `specific` | Only on dates listed in specific_dates[] |

---

### `schedule_overrides`
Skip or replace a single occurrence of a rule — handles exceptions without destroying the rule.

```sql
schedule_overrides (
  id                       uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  rule_id                  uuid REFERENCES schedule_rules(id) ON DELETE CASCADE,
  date                     date NOT NULL,
  action                   text NOT NULL,     -- 'skip' | 'replace'
  replacement_template_id  uuid REFERENCES templates(id) ON DELETE SET NULL
)
```

---

## Date Resolution Logic (client-side)

```js
// Returns the template + blocks to render for a given date
async function getScheduleForDate(date) {
  const rules = await fetchAllRules();   // from Supabase
  const overrides = await fetchOverrides();

  const active = rules.filter(rule => {
    if (date < rule.start_date) return false;
    if (rule.end_date && date > rule.end_date) return false;

    switch (rule.repeat_type) {
      case 'once':     return date === rule.start_date;
      case 'daily':    return true;
      case 'weekly':   return rule.weekdays.includes(dayOfWeek(date));
      case 'monthly':  return dayOfMonth(date) === dayOfMonth(rule.start_date);
      case 'specific': return rule.specific_dates.includes(date);
    }
  });

  // Apply overrides
  return active
    .map(rule => {
      const override = overrides.find(o => o.rule_id === rule.id && o.date === date);
      if (!override) return rule.template_id;
      if (override.action === 'skip') return null;
      if (override.action === 'replace') return override.replacement_template_id;
    })
    .filter(Boolean);
  // Returns array of template_ids (multiple templates can apply to one day)
}
```

**Multiple templates on one day:** Allowed — blocks are merged and sorted by start_time in the timeline view.

---

## RLS Policies (pattern)

```sql
-- Users can only see their own data
CREATE POLICY "user owns templates"
  ON templates FOR ALL
  USING (auth.uid() = user_id);

-- Blocks are accessible if the parent template is owned
CREATE POLICY "user owns blocks"
  ON blocks FOR ALL
  USING (
    template_id IN (SELECT id FROM templates WHERE user_id = auth.uid())
  );
```

Same pattern for `schedule_rules` and `schedule_overrides`.
