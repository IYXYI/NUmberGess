# NUmberGess

## Guess the Number Challenge

This repository implements a simple "Guess the Number" daily challenge using GitHub Pages for the frontend and GitHub Actions for daily generation and issue-based guess handling.

What you'll find here:

- `index.html` - a static frontend (GitHub Pages) where players can enter guesses and view a small leaderboard.
- `assets/style.css` - styles for the page.
- `secret-number.json` - the current secret number (1-100). In the simple default setup this file is public and the frontend fetches it to provide instant hints. See notes below on hiding the number.
- `leaderboard.json` - a simple JSON file storing successful guesses (player and timestamp).
- `.github/workflows/daily-number.yml` - scheduled workflow that generates a new daily secret number and commits it to the repo.
- `.github/workflows/issue-guess.yml` - workflow triggered on new issues; if the issue body contains a numeric guess it responds with a hint and, if correct, updates the leaderboard.

Notes:

- The current implementation keeps `secret-number.json` readable (so the frontend can give immediate feedback). If you want to hide the number until the next day, the Actions workflow can be extended to only provide high/low hints via an API-like interaction from Actions (advanced; not included here).
- To play via the issue-based interface, open a new issue with body containing a single number (or text containing a number). The issue-triggered workflow will reply with "Too high", "Too low", or "Correct!" and will add winners to the `leaderboard.json`.

How to use (locally / testing):

- Serve `index.html` via GitHub Pages (enable Pages in repository settings, serve from the `main` branch root). The static UI will fetch `secret-number.json` and `leaderboard.json` automatically.
- To force a new daily number immediately for testing, run the `daily-number.yml` logic locally or create a manual dispatch in the workflow (you can add a workflow_dispatch trigger).

Next steps / ideas:

- Hide the secret number from the frontend (provide only hint responses) by moving guess handling to Actions and returning hint responses via comments or a small external service.
- Rate-limit per player and add a per-day scoreboard.
- Add nicer animations and mobile layout tweaks.

Per-day scoreboard and rate limiting

- Players can attempt up to 3 guesses per day. The frontend opens a prefilled GitHub issue to record a guess.
- The `issue-guess.yml` workflow enforces the daily limit, comments on the issue with hints, and records winners in `leaderboard.json` (with a `day` tag).
- Attempt counts are stored in `daily_guesses.json` (committed by Actions) so rate-limits persist across restarts.


---

Files created by the automation in this repo are a baseline. Inspect and adjust the workflow permissions before enabling in a real repository.

Getting the GitHub Pages site live

- Make sure Pages are enabled in the repository Settings → Pages. Choose branch: `main`, folder: `/ (root)`.
- After enabling, you can either push to `main` (which triggers the Pages deploy workflow) or run the `Build and deploy GitHub Pages` workflow manually via Actions → `Build and deploy GitHub Pages` → Run workflow.
- The workflow will print the expected Pages URL as a run log. The default URL is: `https://<owner>.github.io/<repo>/`. If your repo is an org or user page, the path may differ.