# Tabang — Monorepo Context

Tabang is a B2B utility/ISP support platform. Each subfolder is its own git repo and its own Docker service. They share a Postgres instance and communicate via Daloy.

---

## Products

| Folder | Product | Status | Port |
|---|---|---|---|
| `tugon/` | AI customer support platform | **Live (Docker)** | 3000 |
| `daloy/` | Integration hub + channel gateway | **Live (Docker)** | 3001 |
| `tanaw/` | Network topology + GIS service | Spec only | 3002 |
| `hatid/` | License server + admin GUI | **Built** | 3003 |
| `susi/` | Shared auth service (passkeys + JWT) | **Built** | 3004 |
| `bayad/` | Payment portal | Not started | — |
| `ayos/` | Back office / field ops (meter reading, procurement) | Stub only | — |

Each product repo has its own `CLAUDE.md` (technical reference) and `handover.md` (current state).

---

## Deployment

**Deployment repo: `tabang-stack`** (`github.com/tabang-xyz/tabang-stack`)

The root `docker-compose.yml`, `.env.example`, and `db/` init scripts live there — not in this repo. Start the full stack from `tabang-stack`:

```bash
docker compose up
```

GHCR images:
- `ghcr.io/tabang-xyz/tugon:latest`
- `ghcr.io/tabang-xyz/daloy:latest`

---

## Inter-service communication

All services communicate over Docker Compose internal DNS. No service talks to the outside world except Daloy.

```
External (FB Messenger, future channels)
        ↕
      Daloy  :3001  — only service with internet access + external credentials
        ↕
      Tugon  :3000  — all business logic, AI, operator UI
        ↕
      Tanaw  :3002  — topology graph (future)
        ↕
      Hatid  :3003  — license validation (future)
        ↑
   Shared Postgres
```

Every inter-service URL is read from an env var with a Docker DNS fallback (e.g. `process.env.TUGON_URL ?? 'http://tugon:3000'`). Leave unset when running from `tabang-stack`.

---

## GitHub issues

All product issues are tracked at `github.com/tabang-xyz/tugon/issues` for now. Each service will get its own repo issue tracker as it matures.

---

## Key decisions

- Daloy is the **only** service with external credentials (FB tokens, IMAP passwords, etc.). Tugon never holds them.
- Postgres is shared across services — each gets its own database (`tugon`, `daloy`, etc.).
- The connector JSON engine in Daloy is the extension point — new channels = new JSON file, no code change.
- Tanaw, Hatid, and Bayad are designed but not yet started. Don't over-engineer Tugon/Daloy around them yet.
