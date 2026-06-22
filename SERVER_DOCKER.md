# Run Twenty On A Server With Docker

This repo includes a root-level Docker Compose file for a simple self-hosted server install.

## Prerequisites

- Docker Engine
- Docker Compose v2
- At least 2 GB RAM

## First Run

Copy the environment template and fill in the required values:

```bash
cp .env.server.example .env.server
openssl rand -base64 32
```

Edit `.env.server`:

- Set `SERVER_URL` to the URL people will use to open Twenty.
- Set `PG_DATABASE_PASSWORD` to a strong value without special characters.
- Set `ENCRYPTION_KEY` to the generated secret.

Start the stack:

```bash
docker compose --env-file .env.server -f docker-compose.server.yml up -d
```

Check status:

```bash
docker compose --env-file .env.server -f docker-compose.server.yml ps
curl http://localhost:${TWENTY_HTTP_PORT:-3000}/healthz
```

## Updating

Set `TAG` in `.env.server` to the Twenty image tag you want, then run:

```bash
docker compose --env-file .env.server -f docker-compose.server.yml pull
docker compose --env-file .env.server -f docker-compose.server.yml up -d
```

## Public Access

For a quick direct server install, keep `TWENTY_HTTP_PORT=3000` and set:

```ini
SERVER_URL=http://your-server-ip:3000
```

For production, put Twenty behind a reverse proxy that terminates HTTPS and forwards traffic to port `3000` on this Docker host. In that case set:

```ini
SERVER_URL=https://crm.example.com
```

Keep the same `ENCRYPTION_KEY` for the lifetime of the instance. Losing it means losing access to encrypted secrets stored in the database.
