# Acquisitions API - Warp Project Guide

## Overview

This is a Node.js + Express API project using:

- ES Modules (`"type": "module"`)
- Drizzle ORM with PostgreSQL
- Zod for request validation
- ESLint + Prettier for code quality

## Project Structure

- `src/index.js` - entry point
- `src/server.js` - starts HTTP server
- `src/routes/` - route definitions
- `src/controllers/` - request handlers
- `src/services/` - business logic
- `src/models/` - database models/schema
- `src/config/` - logger and DB config
- `src/utils/` - shared utilities
- `src/validations/` - Zod schemas

## Setup

1. Install dependencies:
   - `npm install`
2. Create/update `.env` with your values:
   - `PORT=3000`
   - `NODE_ENV=development`
   - `LOG_LEVEL=info`
   - `DATABASE_URL={{DATABASE_URL}}`

## Development

- Run dev server:
  - `npm run dev`

## Database (Drizzle)

- Generate migrations:
  - `npm run db:generate`
- Run migrations:
  - `npm run db:migrate`
- Open Drizzle Studio:
  - `npm run db:studio`

## Code Quality

- Lint:
  - `npm run lint`
- Auto-fix lint issues:
  - `npm run lint-fix`
- Format:
  - `npm run format`
- Check formatting:
  - `npm run format:check`

## Current Auth Routes

- `POST /sign-up`
- `POST /sign-in`
- `POST /sign-out`

## Notes

- Server default URL: `http://localhost:3000`
- Start from project root before running scripts.
