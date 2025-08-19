## Trending Movies App

Discover trending movies, search titles, and see a Top 5 list derived from real user searches stored in Appwrite.

### Features
- Shows top 20 trending/popular movies from TMDB
- Search for movies with debounced input
- Tracks search popularity in Appwrite and displays a Top 5 trending list
- Responsive UI with Tailwind CSS

### Tech Stack
- React + Vite
- Tailwind CSS (via `@tailwindcss/vite`)
- TMDB API (v3 endpoints, v4 Bearer token)
- Appwrite (database for tracking search trends)

---

## Getting Started

### Prerequisites
- Node.js 18+
- TMDB account (to obtain a v4 API Read Access Token)
- Appwrite project (Database + Collection)

### Installation
```bash
npm install
```

### Environment Variables
Create a `.env` file in the project root:

```bash
# TMDB (use your v4 Read Access Token)
VITE_TMDB_API_KEY=YOUR_TMDB_V4_BEARER_TOKEN

# Appwrite
VITE_APPWRITE_PROJECT_ID=your-project-id
VITE_APPWRITE_DATABASE_ID=your-database-id
VITE_APPWRITE_COLLECTION_ID=your-collection-id
```

Notes:
- Vite only exposes variables prefixed with `VITE_` to the client (`import.meta.env`).
- `VITE_TMDB_API_KEY` should be the TMDB v4 Read Access Token (a long string), used as a Bearer token.

### Run in Development
```bash
npm run dev
```
Open the URL printed in your terminal (typically `http://localhost:5173`).

### Production Build
```bash
npm run build
npm run preview
```

---

## Appwrite Setup

1) Create a Project in Appwrite and note the `Project ID`.
2) Create a Database and a Collection for search trends.
3) Add attributes to the Collection (examples):
   - `searchTerm` (string)
   - `count` (integer)
   - `movie_id` (string or integer)
   - `poster_url` (string)
4) Indexes (recommended):
   - Index on `searchTerm` for equality queries
   - Index on `count` for ordering (descending)
5) Configure permissions as needed (e.g., read-all for trending list, restricted writes).

The app uses two operations via the Appwrite SDK:
- Update or create a document when a search succeeds: increments `count` if `searchTerm` exists, otherwise creates a new document.
- Read Top 5 documents ordered by `count` descending to display the trending list.

Appwrite endpoint in this project is set to `https://nyc.cloud.appwrite.io/v1` (see `src/appwrite.js`).

---

## TMDB API Notes

- Base URL: `https://api.themoviedb.org/3`
- Auth: Bearer token header using your v4 Read Access Token:

```http
Authorization: Bearer YOUR_TMDB_V4_BEARER_TOKEN
Accept: application/json
```

- Endpoints used:
  - Search: `/search/movie?query=...`
  - Discover (popular): `/discover/movie?sort_by=popularity.desc`

---

## Project Structure

```
src/
  App.jsx              # Main app: state, effects, fetch, rendering
  appwrite.js          # Appwrite client + DB helpers
  main.jsx             # Bootstraps React to #root
  components/
    Search.jsx         # Controlled input and change handler
    MovieCard.jsx      # Movie tile card
    Spinner.jsx        # Loading indicator
public/                # Static assets (icons/images)
```

---

## Available Scripts

- `npm run dev` — Start Vite dev server
- `npm run build` — Build for production
- `npm run preview` — Preview the production build
- `npm run lint` — Lint the project

---

## Troubleshooting

- TMDB 401/403 errors: ensure `VITE_TMDB_API_KEY` is the v4 Read Access Token and passed as a Bearer token.
- No trending results: verify Appwrite env IDs and collection attributes, and that permissions allow reads.
- CORS/Appwrite: ensure your web origin is allowed in Appwrite settings.
- Environment variables not available: confirm the `VITE_` prefix and that you restarted the dev server after editing `.env`.

---

## Acknowledgements
- Data by TMDB (The Movie Database)
- Backend-as-a-service by Appwrite
- Built with React + Vite and Tailwind CSS
