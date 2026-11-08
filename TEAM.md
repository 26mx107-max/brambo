# Team plan (2 days)

## Who owns what

| | Person 1: Landing and Auth | Person 2: Dashboard and Profile | Person 3: Discovery and Global UI |
|---|---|---|---|
| **Pages** | `index.html`, `auth.html` | `home.html`, `profile.html` | `reviews.html`, `editors.html` |
| **Styles** | `auth.css` | `style.css` | `app.css` |
| **Scripts** | `auth.js`, `script.js` (landing) | `home.js`, `profile.js` | `reviews.js`, `editors.js`, `common.js` |
| **Other** | `server/*` (API, MongoDB, seed), `users.json`, `.env.example` | | `README.md`, `TEAM.md`, search and filter logic |

Heads up on two shared files:
- `style.css` holds the colour/spacing tokens (`:root`) and the landing page styles, and every page loads it. Person 2 owns it, but Person 1 may edit the landing sections and Person 3 should not redefine tokens in `app.css`. Tell Person 2 before changing `:root`.
- `common.js` is used by every page. Person 3 owns it. Person 1 owns only its "accounts" block (`B.api`, `B.startSession`, `B.guard`, `B.logout`). Everyone else asks Person 3 before editing it.

## Suggested checklist

### Person 1: Landing and Auth
- [ ] Day 1: install MongoDB, run `server/` (`npm install`, `.env`, `npm start`, `npm run seed`)
- [ ] Day 1: sign up and log in end-to-end from `auth.html`; check the user appears in MongoDB (`mongosh`, `use brambo`, `db.users.find()`)
- [ ] Day 1: share the demo login and API URL with the team
- [ ] Day 2: landing polish (copy, responsive check at 360px, 768px, 1280px), error states on the form, log out
- [ ] Day 2: optional: rate limiting (`express-rate-limit`), confirm-password strength hint

### Person 2: Dashboard and Profile
- [ ] Day 1: `home.html` rows (Coming soon, Continue watching, Watch later, Friends' recents) look right with real data, including empty states
- [ ] Day 1: check the title filter on home (type a title; rows with no match hide)
- [ ] Day 2: `profile.html`: stats, avatar/banner/bio editing, review delete, responsive layout
- [ ] Day 2: own `style.css` clean-up (unused rules, token consistency)

### Person 3: Discovery and Global UI
- [ ] Day 1: `editors.html` filters (collections/genres/countries) and the title filter; `reviews.html` layout
- [ ] Day 1: top-bar TMDB search (`B.header` in `common.js`): keyboard use, Escape to close, mobile width
- [ ] Day 2: accessibility pass on the shared header (focus order, aria labels), loading and error states
- [ ] Day 2: final README review, screenshots for the report, tag the release

## Git workflow (keeps three people from overwriting each other)

```bash
# once: one person creates the repo and pushes main, the others clone it
git clone <repo-url> && cd brambo

# each person works on their own branch
git checkout -b feat/auth            # person 1
git checkout -b feat/dashboard       # person 2
git checkout -b feat/discovery       # person 3

# small, frequent commits
git add <files you own>
git commit -m "auth: wire signup form to /api/signup"
git pull --rebase origin main        # before every push
git push -u origin feat/auth         # then open a Pull Request into main
```

Rules of thumb: only edit files you own (see table); ask the owner for changes to shared files; merge to `main` at the end of Day 1 and again at the end of Day 2 so problems show up early; never commit `server/.env`, `node_modules/` or a real TMDB key (see README).

## Quick integration test before pushing the final version
1. `server`: `npm start` shows "MongoDB connected".
2. Sign up a new user from the landing page, land on `home.html`.
3. Search a series in the top bar, open it, press "Start watching", then check `profile.html` shows it.
4. Log out, log back in with the same account.
5. On `home.html` and `editors.html`, type in the title filter; clear it.
6. Resize to phone width on every page.
