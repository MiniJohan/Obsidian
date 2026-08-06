# VaultPWA — Password Manager

> Status: Planning
> Stack: React + Vite, Web Crypto API, Argon2id (WASM), Supabase, Vercel
> Type: Personal PWA
> Aesthetic: Dark, minimal (consistent with Mise)

---

## Core Philosophy

**Zero-knowledge**: The server stores only ciphertext. The master password and plaintext passwords never leave the device. Supabase can be breached and it doesn't matter.

---

## Encryption Architecture

```
Master Password + Random Salt (from Supabase)
         ↓ Argon2id (memory-hard KDF)
  Symmetric Key (AES-256-GCM, 256-bit)
         ↓ encrypt / decrypt
  Vault JSON (array of entries, in-memory only)
         ↓ encrypted blob + IV
  Supabase (stores only ciphertext, IV, KDF params)
```

Key never leaves memory. Cleared on lock or timeout.

---

## KDF Decision: Argon2id > PBKDF2

| KDF | Support | Memory-hard | Dep |
|-----|---------|-------------|-----|
| PBKDF2-SHA256 | Native Web Crypto | ❌ GPU-parallelizable | None |
| Argon2id | argon2-browser (WASM) | ✅ Brute-force resistant | ~100KB WASM |

**Decision: Argon2id** via `argon2-browser`. PBKDF2 at 600K iterations is defensible but not future-proof. Argon2id won the Password Hashing Competition for a reason. The WASM dep is worth it.

Params (recommended starting point): `m=65536, t=3, p=4` (~1s on modern hardware)

---

## Supabase Schema

```sql
create table vaults (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null unique,
  encrypted_vault text not null,     -- base64 AES-256-GCM ciphertext
  vault_iv text not null,            -- base64 IV (public, not secret)
  kdf_algorithm text not null default 'argon2id',
  kdf_salt text not null,            -- base64 random 16-byte salt (generated at registration)
  kdf_params jsonb not null,         -- { m, t, p } for Argon2id
  vault_version int not null default 1,
  updated_at timestamptz default now()
);

alter table vaults enable row level security;
create policy "own vault only"
  on vaults for all using (user_id = auth.uid());
```

**Only one row per user.** Whole vault is one encrypted blob.

---

## Vault JSON Structure (plaintext, pre-encryption)

```json
{
  "version": 1,
  "entries": [
    {
      "id": "uuid",
      "label": "GitHub",
      "username": "zon@email.com",
      "password": "hunter2",
      "url": "github.com",
      "notes": "",
      "tags": ["dev"],
      "created": "2026-08-06T00:00:00Z",
      "modified": "2026-08-06T00:00:00Z"
    }
  ]
}
```

Serialized to JSON → encrypted to base64 blob → stored in Supabase.

---

## Unlock Flow

1. User enters master password
2. Client fetches `kdf_salt` and `kdf_params` from Supabase (unauthenticated or via auth token)
3. Argon2id derives 256-bit symmetric key
4. Client fetches `encrypted_vault` and `vault_iv`
5. AES-256-GCM decrypt → vault JSON in memory
6. Key stays in memory. Cleared on lock.

**Wrong password:** GCM auth tag verification fails → "incorrect password" — no need to store a hash.

---

## Save Flow

1. User modifies an entry
2. Generate fresh random IV (16 bytes)
3. AES-256-GCM encrypt vault JSON with current key + new IV
4. PUT encrypted blob + new IV to Supabase
5. Updated `updated_at`

---

## Auth

**Supabase email + password** — magic links are out (iOS PWA opens in Safari, loses session).
Same decision already made for Empty PWA.

---

## V1 Feature Set

### Must-have
- [ ] Unlock screen (master password input)
- [ ] Entry list (search, copy username/password)
- [ ] Add / Edit / Delete entry
- [ ] Password generator (length, charset toggles)
- [ ] Auto-lock after inactivity (5 min, configurable)
- [ ] Clipboard auto-clear (30s after copy)
- [ ] Manual lock button
- [ ] Export encrypted backup (download vault blob as .json)

### V2 / Deferred
- [ ] Import from Bitwarden/CSV
- [ ] TOTP codes
- [ ] WebAuthn biometric unlock
- [ ] HIBP breach check (k-anonymity model)
- [ ] Tags / folders
- [ ] Browser extension (different project entirely)

---

## Security Checklist

- [ ] Master password: never stored, logged, or sent to server
- [ ] Derived key: in-memory only, never persisted
- [ ] Salt: random per user, generated at signup (not email — email is mutable)
- [ ] IV: fresh random bytes per encryption operation
- [ ] Integrity: GCM auth tag handles this — no separate HMAC needed
- [ ] KDF params stored unencrypted (client needs them to unlock)
- [ ] Auto-lock on inactivity
- [ ] Clipboard auto-clear (30s)
- [ ] HTTPS (Vercel default)
- [ ] CSP headers configured in vercel.json
- [ ] No inline scripts
- [ ] RLS on vaults table

---

## What NOT to Do

| Bad idea | Why |
|---|---|
| Store a hash of master password for verification | Unnecessary (GCM auth tag does this) + extra attack surface |
| Per-entry encryption with same key | No security gain, adds complexity + IV management risk |
| localStorage for vault data | Persists plaintext or key on device — use in-memory state only |
| Email as KDF salt | Email can change; use a random salt generated at signup |
| Roll your own crypto | Never. Use Web Crypto API |
| Magic link auth | iOS PWA issue — already decided |
| bcrypt for anything here | bcrypt is a one-way hash — cannot decrypt |

---

## Name Ideas

- **Crypt** — obvious, clean
- **Keep** — as in safekeeping
- **Cofre** — Spanish/Portuguese for "safe/chest"
- **Seal** — sealed vault
- **Strongbox** — classic
- Unnamed until Zon picks one

---

## Decision Log

| Date | Decision | Reasoning |
|---|---|---|
| 2026-08-06 | Argon2id over PBKDF2 | Memory-hard, GPU-resistant, worth WASM dep |
| 2026-08-06 | Vault-level encryption (one blob) | Simpler, fewer IV risks, no real per-entry benefit |
| 2026-08-06 | Supabase email+password auth | iOS PWA magic link issue |
| 2026-08-06 | Random salt (not email) | Email is mutable, breaks key derivation if changed |
| 2026-08-06 | Web Crypto API for AES-GCM | Native, no deps, correct |
