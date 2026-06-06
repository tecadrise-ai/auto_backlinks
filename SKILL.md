# Backlink Placement Agent

## Goal

Place one honest directory or profile backlink for one target URL per run.

The agent reads `backlinks.csv`, picks one directory URL with an empty `status`, signs up with the configured email account, finds the add listing or profile section, submits the single configured `TARGET_URL`, uses one random about-us text from `about_us.csv`, and logs the result.

## Config

Set these values before running. Use `config.example` as the template.

```text
TARGET_URL=https://your-business.example/
BUSINESS_NAME=Your Business Name
ABOUT_US_TEXT=about_us.csv
SIGNUP_NAME=Your Name
EMAIL_LOGIN=backlinks@example.com
EMAIL_PASSWORD=your-email-password
EMAIL_ACCESS_METHOD=imap
EMAIL_IMAP_HOST=imap.example.com
EMAIL_IMAP_PORT=993
EMAIL_IMAP_SSL=true
EMAIL_POP3_HOST=pop.example.com
EMAIL_POP3_PORT=995
EMAIL_POP3_SSL=true
EMAIL_SMTP_HOST=smtp.example.com
EMAIL_SMTP_PORT=587
```

Use IMAP for verification emails when available. Use POP3 only if IMAP is not available. SMTP is only needed if the directory requires sending or replying to an email.

## Required Files

Keep these files in the skill working folder.

### `backlinks.csv`

```csv
url,status,notes,live_url
https://example-directory.com,,,
https://example-startup-list.com,done 2026-06-06,Submitted and approved,https://example-startup-list.com/products/acme
https://paid-only-directory.com,skip,Requires paid plan,
```

Use the first row where `status` is empty. Process only one row per run.

### `about_us.csv`

This file contains only text variations for the one configured `TARGET_URL`.

```csv
about_us
"Acme Analytics helps growing teams understand product usage, find drop off points, and make better decisions with simple dashboards."
"Acme Analytics gives founders and marketers a clear view of customer behavior without complex setup or enterprise reporting tools."
"Acme Analytics turns website and product events into practical reports teams can use to improve activation, retention, and revenue."
```

Pick one random row from `about_us.csv` for each listing.

## Browser Rules

- Use the browser tools available in the current agent setup, such as Playwright, browser MCP, Chrome CDP, or a user controlled browser harness.
- Navigate, inspect the page, then click/type using visible page context. Do not guess hidden selectors.
- Do not bypass captchas, paywalls, phone verification, or destructive prompts. Mark the row blocked and explain why.
- Do not create spammy, false, or unrelated listings. Skip sites where the business does not fit.

## Workflow

1. Read `backlinks.csv` and choose the first URL with empty `status`.
2. Read the configured values:
   - `TARGET_URL`
   - `BUSINESS_NAME`
   - `SIGNUP_NAME`
   - `EMAIL_LOGIN`
   - `EMAIL_PASSWORD`
   - `EMAIL_ACCESS_METHOD`
   - IMAP or POP3 host, port, and SSL settings
3. Read `about_us.csv` and randomly select one about-us text.
4. Open the directory URL from `backlinks.csv` in the browser.
5. Look for signup/login links such as `Sign up`, `Register`, `Add listing`, `Submit`, `Claim profile`, `Add website`, or `Create profile`.
6. If an account is needed, sign up with `SIGNUP_NAME`, `EMAIL_LOGIN`, and `EMAIL_PASSWORD`.
7. If email verification is required, access the inbox using the configured email method, preferably IMAP, otherwise POP3. Find the newest verification email from that site and complete verification.
8. Find the add listing or profile section and submit:
   - Business name from `BUSINESS_NAME`
   - Website from `TARGET_URL`
   - Description from the randomly selected `about_us.csv` row
   - Closest matching category if requested
   - Logo, images, address, or social fields only if provided by the user's project files
9. Save or publish the listing.
10. Update `backlinks.csv`:
   - `done YYYY-MM-DD` when submitted or live
   - `blocked YYYY-MM-DD: reason` for captcha, paid only, OAuth only, wrong category, broken site, or manual action needed
   - Add `live_url` when available
11. Report the directory URL processed, status, live URL if any, and next blocker if manual help is required.

## Status Examples

```csv
url,status,notes,live_url
https://good-directory.com,done 2026-06-06,Profile published,https://good-directory.com/acme-analytics
https://review-site.com,blocked 2026-06-06: captcha,Manual captcha required,
https://paid-directory.com,blocked 2026-06-06: paid only,No free submission path,
https://wrong-niche.com,skip 2026-06-06,Only accepts restaurants,
```

## Keep It Safe

- One backlink target per run.
- Never overwrite the whole CSV if you can update only the chosen row.
- Never expose real passwords in reports or logs.
- Use truthful business descriptions only.
- If unsure whether a submission is allowed, stop and ask the user.
