# Utpost

Plattform för friluftsdestinationer. Redaktionella guider, användarnas egna turer och bilder.

## Kom igång

Kräver **Docker Desktop** och **Node 22.18+** (`node -v`).

### Sätt 1: allt i Docker

```bash
docker compose up --build -d          # postgres, mongo, api (:4000) och client (:3001)
docker compose exec api npm run seed  # första gången: testdata i Postgres
```

Klienten: http://localhost:3001 · API: http://localhost:4000/api/health (ska svara `"postgres":"ok","mongodb":"ok"`).
Logga in med valfri seedad användare, lösenord `hemligt123` (e-postadresserna syns i `docker compose exec postgres psql -U utpost -c 'select email from users'`).

Stoppa med `docker compose down`. Lägg till `-v` för att även kasta databasernas data.

### Sätt 2: databaserna i Docker, appen med npm (utveckling)

```bash
npm install
docker compose up -d postgres mongo   # bara databaserna, på 5433 och 27017
npm run seed                          # första gången
npm run dev:api                       # API:et på :4000, startar om vid filändring
npm run dev:client                    # Vue-klienten på :3001 med hot reload
```

`api/src/config.js` läser `DATABASE_URL` och `MONGO_URL` ur miljön om de finns, annars localhost-adresserna ovan.

## Struktur

- `api/` – Express + Postgres (Drizzle) + MongoDB (driver). REST under `/api`, GraphQL på `/graphql` (`api/src/graphql/`, GraphiQL i browsern på http://localhost:4000/graphql). Kräver Node 22.18+ (routes skrivs i TypeScript och körs direkt av Node). `api/Dockerfile` byggs från repots rot.
- `client/` – ny klient i Vue 3 + TypeScript (port 3001). `client/Dockerfile` bygger och serverar `dist/` (multi-stage kommer i M4).
- `shared/` – API-kontraktet som TypeScript-typer (`@utpost/shared`), används av `api/` och `client/`.
- `web/` – den gamla React-klienten. Portas vy för vy till `client/`, tas bort i M6.
- `compose.yaml` – hela miljön. `docs/decisions/` – teamets beslutsdokument.

## Kommandon (kör från roten)

    npm run dev:client        # Vue-klienten på :3001 (API:et måste köra: npm run dev:api)
    npm run lint              # ESLint på client/
    npm run format:check      # Prettier – bara kontroll, ändrar inget
    npm run typecheck         # vue-tsc i client/ + tsc i api/
    npm test                  # Vitest, en gång, avslutar
    npm run build             # vite build av client/
    npm run mongo:smoke [id]  # skriver en tur ur Postgres som ett dokument i MongoDB och läser tillbaka

## Deploy

Kommer i M6.
