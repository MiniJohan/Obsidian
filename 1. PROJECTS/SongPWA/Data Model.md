# Data Model — Song Player

---

## Supabase Tables

### `tracks`

| Column | Type | Notes |
|---|---|---|
| `id` | uuid | PK — `gen_random_uuid()` |
| `user_id` | uuid | FK → `auth.users(id)`, cascade delete |
| `title` | text NOT NULL | Track title (required) |
| `artist` | text | Optional |
| `duration_seconds` | integer | Extracted client-side on upload |
| `file_path` | text NOT NULL | Path in Supabase Storage `audio` bucket |
| `cover_path` | text | Path in Supabase Storage `artwork` bucket |
| `file_size` | integer | Bytes |
| `mime_type` | text | `audio/mpeg`, `audio/mp4`, etc. |
| `created_at` | timestamptz | Default `now()` |

---

## Supabase Storage

Two private buckets. Signed URLs generated on demand for playback and thumbnail display.

| Bucket | Contents | Access |
|---|---|---|
| `audio` | MP3 / M4A / WAV / FLAC | Private — signed URLs |
| `artwork` | JPEG / PNG cover images | Private — signed URLs |

**Path convention:**
- Audio: `{user_id}/{track_id}.{ext}`
- Artwork: `{user_id}/{track_id}.{ext}`

UUID filename ensures uniqueness and prevents collisions without extra logic.

**Signed URL expiry:** Default 1 hour is fine. URLs are generated fresh when a track is tapped to play or when the library loads artwork. For the active player, regenerate the URL if the user is still listening after 50 minutes (edge case, handle in V2 if needed).

---

## RLS Policies

Users can only touch their own tracks.

---

## SQL

```sql
-- Tracks table
create table tracks (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id) on delete cascade not null,
  title text not null,
  artist text,
  duration_seconds integer,
  file_path text not null,
  cover_path text,
  file_size integer,
  mime_type text,
  created_at timestamptz default now()
);

-- RLS
alter table tracks enable row level security;

create policy "own tracks only"
  on tracks for all
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);

-- Index — fast library fetch sorted by recency
create index tracks_user_created_idx on tracks(user_id, created_at desc);
```
