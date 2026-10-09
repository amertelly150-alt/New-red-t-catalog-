# red-t Catalog — مستقل عن بيئة التطوير السابقة

## Architecture
- Frontend: Vercel (Vite/React)
- API: Render (Express/Node)
- Database: PostgreSQL (Neon, Supabase, Render Postgres, or another managed PostgreSQL provider)
- Admin: `/admin` on the frontend, using a secure HttpOnly session cookie on the API

## 1. Create PostgreSQL
Create a managed PostgreSQL database and copy its connection string into `DATABASE_URL`.

## 2. Deploy API
Create a Render Web Service from this repository. `render.yaml` contains the build/start commands. Set:
- `DATABASE_URL`
- `ADMIN_USERNAME`
- `ADMIN_PASSWORD`
- `CORS_ORIGIN` = the exact Vercel site origin, e.g. `https://red-t-catalog.vercel.app`

The build runs Drizzle schema push before starting the API. The catalog seeds itself from the bundled default data on first successful API access.

## 3. Deploy frontend
Import the same repository into Vercel. The included `vercel.json` builds the Vite app and serves `artifacts/red-t-catalog/dist/public`.
Set:
- `VITE_API_BASE_URL` = the Render API origin, without a trailing slash

## 4. Verify
- `https://YOUR-API/api/healthz` returns `{"status":"ok"}`
- `https://YOUR-FRONTEND/` loads the catalog
- `https://YOUR-FRONTEND/admin` opens the admin login
- Login, add/edit/delete a product, then refresh the public catalog

## Important
Do not commit `.env` or real admin credentials.
