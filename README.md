# Acquisitions API Docker Setup (Neon Local + Neon Cloud)

This project is dockerized for two modes:

- **Development**: app container + **Neon Local** proxy container (ephemeral branch workflow)
- **Production**: app container connecting directly to **Neon Cloud** using `DATABASE_URL`

## Files added

- `Dockerfile`
- `docker-compose.dev.yml`
- `docker-compose.prod.yml`
- `.env.development`
- `.env.production`

## 1) Development mode (Neon Local)

Neon Local runs as a Docker service and exposes Postgres on `neon-local:5432` inside the compose network.

Your app uses:

- `DATABASE_URL=postgres://neon:npg@neon-local:5432/neondb`
- `NEON_LOCAL_SQL_ENDPOINT=http://neon-local:5432/sql`
- `USE_NEON_LOCAL=true`

Neon Local uses:

- `NEON_API_KEY`
- `NEON_PROJECT_ID`
- optional `PARENT_BRANCH_ID` (if empty, default parent branch is used)
- `DELETE_BRANCH=true` (ephemeral branch cleanup on shutdown)

### Start development stack

1. Fill placeholders in `.env.development`:
   - `{{NEON_API_KEY}}`
   - `{{NEON_PROJECT_ID}}`
   - optional `PARENT_BRANCH_ID`
2. Start:

```bash
docker compose -f docker-compose.dev.yml up --build
```

3. API will be available at:
   - `http://localhost:3000`

### Stop development stack

```bash
docker compose -f docker-compose.dev.yml down
```

With `DELETE_BRANCH=true`, Neon Local removes the ephemeral branch when the proxy container is stopped.

## 2) Production mode (Neon Cloud)

Production does **not** run Neon Local.  
The app container connects directly to your Neon Cloud database via:

- `DATABASE_URL={{DATABASE_URL}}`
- `USE_NEON_LOCAL=false`

### Start production stack

1. Fill placeholders in `.env.production`:
   - `{{DATABASE_URL}}` (Neon Cloud URL)
   - `{{JWT_SECRET}}`
2. Start:

```bash
docker compose -f docker-compose.prod.yml up --build -d
```

3. API will be available at:
   - `http://localhost:3000`

### Stop production stack

```bash
docker compose -f docker-compose.prod.yml down
```

## 3) How DATABASE_URL switches between environments

- **Development** (`.env.development`):  
  `DATABASE_URL=postgres://neon:npg@neon-local:5432/neondb`
- **Production** (`.env.production`):  
  `DATABASE_URL={{DATABASE_URL}}` (Neon Cloud endpoint)

The application-level switch is controlled by:

- `USE_NEON_LOCAL=true` in development
- `USE_NEON_LOCAL=false` in production

Your existing DB config in `src/config/database.js` already reads this flag and only applies the local Neon endpoint when enabled.
