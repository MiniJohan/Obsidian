---
status: active
last-updated: 2026-08-02
---

# Empty PWA

> Minimal iPhone-first thought capture and checklist app.
> Live on GitHub Pages. Supabase backend with magic-link auth.

---

## Stack
| Layer | Tools |
|---|---|
| Frontend | Vanilla HTML / CSS / JS, PWA manifest |
| Backend | Supabase (Postgres + Auth) |
| Hosting | GitHub Pages |
| Auth | Magic-link (passwordless email via Supabase) |
| AI (planned) | Ollama via Cloudflare Tunnel |

---

## What's Been Built

- PWA scaffolded and live on GitHub Pages
- Supabase project connected and configured
- Magic-link authentication working end-to-end (no passwords)
- Core thought-capture flow: write a thought, it persists to Supabase
- Basic checklist UI functional

---

## What's Needed Next

### 1. Timestamps
Add created/updated timestamps to thoughts.

**Tasks:**
- [ ] Confirm `created_at` column exists on `thoughts` table (Supabase adds this by default)
- [ ] Add `updated_at` column with auto-update trigger if not present
- [ ] Display timestamps in UI — relative format ("2 hours ago") preferred
- [ ] Consider a small utility like `dayjs` or write a minimal relative-time helper

---

### 2. Multiple Lists
Biggest structural change. Turns the app from a single stream into a proper multi-list tool.

**Schema changes needed:**
```sql
-- New table
create table lists (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  name text not null,
  created_at timestamptz default now()
);

-- Add FK to thoughts
alter table thoughts add column list_id uuid references lists(id);
```

**Tasks:**
- [ ] Create `lists` table in Supabase
- [ ] Add `list_id` FK column to `thoughts` table
- [ ] Migrate existing thoughts to a default list
- [ ] UI: list switcher / sidebar
- [ ] UI: create new list, rename list, delete list (with confirmation)
- [ ] Filter thoughts query by active `list_id`
- [ ] Handle default list for new users on first login

---

### 3. AI Rewrite via Ollama
Let the local AI rewrite or clean up a captured thought in place.

**How it works:**
- Cloudflare Tunnel exposes local Ollama endpoint to the public internet (with auth)
- PWA calls tunnel URL with the thought text
- Ollama (qwen2.5:14b or a lighter model like llama3.2:3b for speed) rewrites it
- Result replaces or sits alongside the original thought

**Tasks:**
- [ ] Set up Cloudflare Tunnel pointing to local Ollama (port 11434)
- [ ] Secure the tunnel endpoint (Cloudflare Access or a simple secret header)
- [ ] Add "rewrite" button to thought UI
- [ ] POST thought text to tunnel → stream or await response
- [ ] Update thought in Supabase with rewritten text (or show diff first)
- [ ] Decide: auto-replace or show before/after and let user confirm

**Note:** VALE already has Ollama running. The tunnel is the only missing piece.

---

## Known Issues
-

---

## Decision Log

### 2026-08-02 — Second Brain setup
Set up Obsidian vault (Void) as a second brain with Claude Desktop MCP integration.
This file is Claude's persistent context for the Empty PWA project.
Vault path: `D:\Obsidian\Vaults\void`
MCP server: `@modelcontextprotocol/server-filesystem`
