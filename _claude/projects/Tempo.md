# Tempo — Schedule PWA

> Status: Backend build in progress
> Stack: Vanilla HTML/CSS/JS, Supabase (Postgres + Auth), Service Worker + IndexedDB
> Hosting: TBD (GitHub Pages / Cloudflare Pages / Vercel)
> Source of truth: `D:\Obsidian\Vaults\void\1. PROJECTS\Tempo\`

---

## What It Is

Personal daily schedule planner. Template-driven, repeat-rule aware, iPhone-first.
Not a calendar app — manages day *structure*, not events.

---

## Scope of Claude's Work

**Backend only.** Zon is building the frontend (Phases 1–7 of Build Plan) independently.
Claude delivers: Supabase schema, RLS, auth.js, db.js, .env, hosting config.

---

## Auth Decision

⚠️ Project file says magic link — but magic link fails on iOS PWA (same issue as Empty + Mise).
**Pending confirmation from Zon: magic link or email+password?**

---

## Backend Checklist

- [ ] Supabase schema SQL (templates, blocks, schedule_rules, schedule_overrides)
- [ ] RLS policies (all 4 tables)
- [ ] js/auth.js — auth flow (pending auth decision)
- [ ] js/db.js — all CRUD functions
- [ ] Config / .env setup
- [ ] Hosting config (pending platform decision)
- [ ] sw.js / IndexedDB offline layer

---

## Data Model (summary)

| Table | Key columns |
|---|---|
| `templates` | id, user_id, name, color |
| `blocks` | id, template_id, title, start_time, end_time, notes, order_index |
| `schedule_rules` | id, user_id, template_id, start_date, end_date, repeat_type, weekdays[], specific_dates[] |
| `schedule_overrides` | id, rule_id, date, action (skip/replace), replacement_template_id |

---

## Decision Log

| Date | Decision | Reasoning |
|---|---|---|
| 2026-08-07 | Backend-only scope | Zon handles frontend, Claude handles DB/auth/data layer |
| 2026-08-07 | Auth method TBD | Magic link in spec but conflicts with iOS PWA session issue |
