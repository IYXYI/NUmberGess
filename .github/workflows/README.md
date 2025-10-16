Workflows for the Guess the Number project live in this folder.

- `daily-number.yml`: generates a new daily secret and commits `secret-number.json`.
- `issue-guess.yml`: reacts to new issues containing guesses and updates `leaderboard.json` when players guess correctly.

Adjust workflow permissions in repository settings before enabling in a public repository.
