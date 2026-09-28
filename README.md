# Video Interview Assessor

A generative-AI video interview assessment tool, built during the **Plugin Live Hackathon**. Candidates record video responses in the browser; the app uploads and processes the recordings and produces assessment reports.

## Tech Stack

**Frontend** (`frontend/`) — React 18 + Vite
- Tailwind CSS for styling
- [Supabase](https://supabase.com/) (`@supabase/supabase-js`, `@supabase/auth-ui-react`) for authentication and session handling
- `react-media-recorder` for in-browser webcam/video recording
- `react-router-dom` for routing
- ESLint for linting

**Backend** (`backend/`) — Node.js + Express
- `pg` — PostgreSQL client
- `multer` — handling uploaded video files
- `fluent-ffmpeg` — server-side video processing
- `googleapis` — Google Drive API integration (recordings/reports are stored to Drive via an OAuth2 flow)
- `axios`, `cors`, `dotenv`

## Structure

```
backend/
  server.js            # Express app entry point
  report.js            # report generation
  generate_token.js    # one-time OAuth2 flow to authorize the app against Google Drive
                        # (writes a token to credentials.json)
  credentials.json      # Google OAuth token (generated, not a static secret file)

frontend/
  src/
    App.jsx
    components/
      LoginRegister.jsx
      Dashboard.jsx
      VideoRecorder.jsx
      VideoPreview.jsx
      Reports.jsx
      SessionContext.jsx
      SupabaseClient.jsx
```

## Setup

### Backend

```bash
cd backend
npm install
```

Requires:
- A `.env` file with your PostgreSQL connection details and any Supabase service keys the backend needs.
- A Google Cloud OAuth client secret (`client_secret.json`) with the Drive API enabled.
- Run the one-time authorization flow to obtain a Drive API token before starting the server:

```bash
node generate_token.js
# follow the printed URL, paste back the auth code
```

Then start the server:

```bash
node server.js
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Requires a Supabase project URL/anon key configured (see `src/components/SupabaseClient.jsx`) for auth to work.

## Notes

This was built for a hackathon (Gen AI / video speech analysis track) — expect it to be a prototype rather than a production-hardened app.
