# auto_backlinks

`auto_backlinks` is a simple agent skill for placing directory backlinks for one target URL.

The workflow is intentionally small:

1. Pick one directory URL from `backlinks.csv` where `status` is empty.
2. Open that directory in a browser.
3. Sign up or log in with the configured email account.
4. Find the add listing, submit website, create profile, or similar section.
5. Submit the configured `TARGET_URL`.
6. Use one random about-us text from `about_us.csv`.
7. Update `backlinks.csv` with `done`, `blocked`, or `skip`.

## Files

- `SKILL.md` contains the agent instructions.
- `config.example` shows the required target URL and email inbox settings.
- `backlinks.csv` contains example directory targets.
- `about_us.csv` contains example about-us text variations for the one target URL.

## Setup

Copy `config.example` to your own private config file or agent environment. Do not commit real passwords.

Edit:

- `TARGET_URL`
- `BUSINESS_NAME`
- `SIGNUP_NAME`
- `EMAIL_LOGIN`
- email inbox settings
- `about_us.csv`
- `backlinks.csv`

## Safety

This project is for legitimate, manual-style directory submissions only. Do not bypass captchas, paywalls, phone verification, or site rules. If a site is not relevant to the business, mark it skipped.
