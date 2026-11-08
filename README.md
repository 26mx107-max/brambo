<<<<<<< HEAD
# Brambo

A dark, Letterboxd-inspired web series tracker. Browse live TMDB data, log episodes, save shows for later, write reviews and build a profile.

- **Frontend:** plain HTML/CSS/JS (no build step). Live series data from [TMDB](https://www.themoviedb.org/).
- **Backend:** Node + Express + MongoDB (Mongoose) for accounts. Passwords hashed with bcryptjs, sessions via JWT.
- **Team split and Git workflow:** see [TEAM.md](TEAM.md).

## Run it locally

You need Node 18+ and MongoDB running locally (or a MongoDB Atlas connection string).

### 1. Backend
```bash
cd server
npm install
cp .env.example .env        # then edit .env and set JWT_SECRET
npm start                   # http://localhost:5000
npm run seed                # optional: creates the demo account from ../users.json
```

### 2. Frontend
Open `config.js` and paste your TMDB key into `apiKey` (free at themoviedb.org > Settings > API).
Then serve the project folder (not `server/`) with any static server:
```bash
npx serve .                 # or use the VS Code "Live Server" extension
```
Open `index.html` through that URL (not by double-clicking the file). Demo login after seeding: `demo` / `demo12345`.

### Keep your TMDB key out of Git
`config.js` is committed with an empty key. After you add yours, tell Git to ignore your local change:
```bash
git update-index --skip-worktree config.js
```
(Undo with `--no-skip-worktree` if you ever need to commit a change to that file.)

## API

| Method | Path | Body | Success |
|---|---|---|---|
| POST | `/api/signup` | `{ username, email, password }` | `201 { token, user }` |
| POST | `/api/login` | `{ identifier, password }` (username or email) | `200 { token, user }` |
| GET | `/api/me` | header `Authorization: Bearer <token>` | `200 { user }` |
| GET | `/api/health` | | `200 { ok: true }` |

Errors return `{ message }` with `400` (validation), `401` (bad credentials), `409` (username/email taken).

## How the frontend uses it
`auth.js` validates the form, then calls `B.api()` (in `common.js`), which `fetch`es the API. On success `B.startSession()` stores `brambo.token` and `brambo.session` in `localStorage` and redirects to `home.html`. Every app page calls `B.guard()`, which redirects to the login page if the token is missing or expired. `Log out` clears both keys.

Your watch progress, favourites, reviews and profile edits are still stored in the browser (`localStorage`), per user. Moving them into MongoDB is the natural next step if there is time.

## Pages
| Page | What it does |
|---|---|
| `index.html` | Landing page (every call to action leads to `auth.html`; logged-in visitors go straight to `home.html`) |
| `auth.html` | Log in / sign up |
| `home.html` | Coming soon, continue watching, watch later, friends' recents. Has a live title filter |
| `reviews.html` | Your reviews and community reviews |
| `editors.html` | Editor's choice with genre/country/collection filters and a live title filter |
| `profile.html` | Stats, shows watched, favourites, reviews, edit avatar/banner/bio |

## Project structure
```
index.html  auth.html  home.html  profile.html  reviews.html  editors.html
style.css   app.css    auth.css                    # global + landing / app shell / auth styles
common.js   config.js                              # shared helpers (TMDB, session, UI) / settings
script.js   auth.js    home.js  profile.js  reviews.js  editors.js
users.json                                         # dev seed accounts for `npm run seed`
server/     server.js  seed.js  package.json  .env.example
```

## Known limits (good "future work" for the report)
- Activity data (progress, favourites, reviews) lives in the browser, and the friends' feed is simulated.
- No rate limiting or email verification on the auth endpoints.
- A TMDB key is required; without it the pages show a "key required" message.
=======
# brambo
>>>>>>> 81eab3a1ff884725347b5a5af8cc64f54b205c27
