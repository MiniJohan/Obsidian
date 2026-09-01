# LORE — Data Model

---

## memory.json

Pre-seeded on project creation. Lives at `D:\LORE\backend\lore_data\memory.json`.
Updated manually until Phase 4 (vault bridge). LORE can write `facts` and `preferences` dynamically via LLM response fields.

```json
{
  "identity": {
    "name": "Zon",
    "os": "Windows",
    "editor": "VS Code",
    "gpu": "RTX 5060 Ti 16GB",
    "stack": ["HTML/CSS/JS", "React + Vite", "Python", "FastAPI", "Supabase"],
    "learning": ["React", "Python backend"]
  },
  "projects": [
    { "name": "LORE", "status": "active build", "location": "D:\\LORE\\" },
    { "name": "SongPWA", "status": "planning", "stack": "React + Vite + Supabase" },
    { "name": "VaultPWA", "status": "architecture done, awaiting green light" },
    { "name": "Mise", "status": "V1 complete, live on Vercel" }
  ],
  "facts": [],
  "preferences": {},
  "conversation_history": []
}
```

---

## LLM Response Schema

Two shapes. `remember` and `forget` are optional on either.

```json
{ "type": "chat", "message": "Here's my answer." }

{ "type": "chat", "message": "...", "remember": "Zon prefers responses under 3 sentences" }

{ "type": "chat", "message": "...", "forget": "fact_id_to_remove" }
```

No `action` type. LORE does not open apps or URLs.

---

## Capability System

Each capability is a Python file in `backend/capabilities/`. Adding a file and registering it is all that's needed to extend LORE.

### Base class (`base.py`)

```python
class BaseCapability:
    name: str = ""
    description: str = ""

    def system_prompt_injection(self, memory: dict) -> str:
        """Context injected into every system prompt when this capability is active."""
        return ""

    def should_activate(self, query: str) -> bool:
        """Whether this capability activates for this query. Default: always."""
        return True
```

### Registry (`__init__.py`)

Loads all capabilities at startup. Each active capability calls `system_prompt_injection()` and its output is appended to the system prompt before every LLM call. Order matters — general capabilities first.

### Planned capabilities

| File | Phase | What it does |
|---|---|---|
| `general.py` | 1 | Always active. Injects identity and basic context. |
| `code.py` | 3 | Detects code questions. Injects stack, editor, current learning focus. |
| `projects.py` | 3 | Injects active projects and their statuses. |
| `vault.py` | 4 | Reads `_claude/active.md` at startup. Injects live project state. |

---

## log.json

Rolling log. Max 100 entries, oldest trimmed on write.

```json
[
  {
    "timestamp": "2026-09-01T14:32:00",
    "input": "What's the best way to manage state in React?",
    "response": "For your scale, useState and useContext are sufficient..."
  }
]
```
