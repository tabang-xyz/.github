# Lintune

**Open-source MSP platform.** One identity, one password, one URL — for every service you run for your customers.

Lintune wires together Keycloak, Mailcow, Nextcloud, and Vaultwarden behind a single SSO layer and gives MSPs a control plane to provision and manage all of it from one place.

```sh
curl -fsSL https://get.lintune.xyz | bash
```

---

## How it works

```
curl get.lintune.xyz | bash
        │
        ▼
  lintune-admin  ──SSH──▶  target server
  (control plane)             │
        │                     ├── Keycloak   (SSO & identity)
        │                     ├── Mailcow    (email)
        │                     ├── Nextcloud  (file storage)
        │                     └── Vaultwarden (passwords)
        │
  lintune-dash
  (tenant portal)
```

The installer provisions everything and wires each service to Keycloak SSO automatically. MSPs manage realms (customers) from `lintune-admin`; end users log in once at `lintune-dash` and reach all their services.

---

## Repositories

| Repo | Description |
|------|-------------|
| [lintune-admin](../lintune-admin) | Laravel control plane for MSP operators. Handles realm provisioning, remote installs over SSH, Keycloak/Mailcow/Nextcloud integration, and service status monitoring. Docker support files live in `docker/`. |
| [lintune-dash](../lintune-dash) | Laravel tenant portal for end customers. Scoped to a single realm per session — users, groups, mailboxes, and Nextcloud access. Docker support files live in `docker/`. |
| [uptime-kuma](../uptime-kuma) | Fork of [Uptime Kuma](https://github.com/louislam/uptime-kuma) extended with a Lintune REST API (`/api/lintune/*`) for programmatic monitor management. Published as `ghcr.io/lintune/uptime-kuma` on Github Container Registry. |
| [get.lintune.xyz](../get.lintune.xyz) | Bootstrap install script. Installs Docker, generates secrets, writes config files, and starts the Lintune stack via `docker compose`. |

---

## Stack

- **Identity** — Keycloak 26 with Organizations + Home IdP Discovery (per-tenant email-domain routing, no manual IdP picker)
- **Email** — Mailcow Dockerized
- **Files** — Nextcloud AIO with `user_oidc` for Keycloak group-gated login
- **Passwords** — Vaultwarden *(planned)*
- **Status** — Uptime Kuma (Lintune fork)
- **Apps** — Laravel 11 + PHP 8.4 + MariaDB 11
- **Proxy** — Caddy (automatic SSL) or bring your own

---

## Status

Early alpha — full installer working end-to-end with Keycloak, Mailcow, and Nextcloud. Vaultwarden integration planned.
