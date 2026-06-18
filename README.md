# Supabase Stack Docker

Self-hosted [Supabase](https://supabase.com) — the open-source Firebase alternative.

## Services

| Service | Image | Description |
|---------|-------|-------------|
| **Studio** | `supabase/studio` | Web dashboard for managing your Supabase project |
| **Kong** | `kong/kong:3.9.1` | API gateway — routes all API traffic |
| **Auth** | `supabase/gotrue:v2.189.0` | Authentication server (GoTrue) |
| **REST** | `postgrest/postgrest:v14.12` | Instant REST API (PostgREST) |
| **Realtime** | `supabase/realtime:v2.102.3` | WebSocket realtime engine |
| **Storage** | `supabase/storage-api:v1.60.4` | File storage API |
| **Meta** | `supabase/postgres-meta:v0.96.6` | Database introspection API |
| **Functions** | `supabase/edge-runtime:v1.74.0` | Edge Functions (Deno) |
| **DB** | `supabase/postgres:17.6.1.136` | PostgreSQL with Supabase extensions |
| **Supavisor** | `supabase/supavisor:2.9.5` | Database connection pooler |
| **imgproxy** | `darthsim/imgproxy:v3.30.1` | Image transformation proxy |

## Usage

```bash
# Start all services
docker compose up -d

# Access the Studio dashboard
open http://localhost:8000
```

Default dashboard credentials: `supabase` / `this_password_is_insecure_and_should_be_updated`

## Configuration

All environment variables are in `.env`. **Change all secrets before going to production:**

- `POSTGRES_PASSWORD` — database password
- `JWT_SECRET` — JWT signing key (32+ chars)
- `DASHBOARD_PASSWORD` — Studio login password
- `SECRET_KEY_BASE` — Realtime/Supavisor secret
- `VAULT_ENC_KEY` — Supavisor encryption key (32 chars)
- `PG_META_CRYPTO_KEY` — Studio encryption key (32 chars)

## API Access

All API routes are proxied through Kong on port 8000:

| Endpoint | Description |
|----------|-------------|
| `http://localhost:8000/rest/v1/` | REST API (PostgREST) |
| `http://localhost:8000/auth/v1/` | Auth API (GoTrue) |
| `http://localhost:8000/storage/v1/` | Storage API |
| `http://localhost:8000/graphql/v1` | GraphQL API |
| `http://localhost:8000/realtime/v1/` | Realtime WebSocket |
| `http://localhost:8000/functions/v1/` | Edge Functions |

Use the anon key from `.env` as the `apikey` header for public endpoints.
