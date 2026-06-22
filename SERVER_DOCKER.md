# Run Twenty On A Server With Docker

This repo includes a root-level Docker Compose file configured for Dokploy.

## Prerequisites

- Dokploy with a Docker Compose service
- At least 2 GB RAM

## First Run

Copy the environment template and fill in the required values:

```bash
cp .env.example .env
openssl rand -base64 32
```

Edit `.env`:

- Set `SERVER_URL` to the public URL people will use to open Twenty, usually your Dokploy domain such as `https://crm.example.com`.
- Set `PG_DATABASE_PASSWORD` to a strong value without special characters.
- Set `ENCRYPTION_KEY` to the generated secret.

In Dokploy:

- Use Compose Path: `./docker-compose.yml`
- Add a domain for service `server`
- Set the container port to `3000`

The Compose file uses `expose: 3000` instead of host port binding, so it will not conflict with Dokploy running on host port `3000`.

For a manual local check, start the stack:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
```

## Updating

Set `TAG` in `.env` to the Twenty image tag you want, then run:

```bash
docker compose pull
docker compose up -d
```

## Public Access

This root Compose file is intended to run behind Dokploy's reverse proxy. Set:

```ini
SERVER_URL=https://crm.example.com
```

If you are not using Dokploy and need direct host-port access, use `docker-compose.server.yml` instead and set:

```ini
SERVER_URL=http://your-server-ip:3000
```

Keep the same `ENCRYPTION_KEY` for the lifetime of the instance. Losing it means losing access to encrypted secrets stored in the database.
