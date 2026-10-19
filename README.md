# Login page for Claude MCP Connector

Sign-in page for **Tietokanta Sync**, an MCP connector that lets Claude check and control the Shopify &rarr; Supabase &rarr; Netvisor sync. The page is a single static `index.html` served by GitHub Pages.

## What it does

The page has two modes:
- **Opened by Supabase Auth** (URL has `?authorization_id=...`): when someone clicks **Connect** in Claude, they sign in here and then either **Allow** or **Deny** Claude's access. On **Allow**, they are sent back to Claude.
- **Opened directly**: the person signs in to their account and can change their password (at least 12 characters).

The page signs people with Supabase Auth (email and password). It doesn't store any data of its own. Sign-ups are off, so accounts are created by an admin in Supabase.

## Sessions

- The sign-in is kept in `sessionStorage`, so it ends when the tab or browser is closed. Every new tab asks people to sign in again.
- **Sign out** ends only this page's session (`scope: "local"`). Claude's connection uses its own tokens and keeps working.

## Configuration

At the top of the script in `index.html`:

| Constant | Value |
|---|---|
| `SUPABASE_URL` | database url |
| `SUPABASE_PUBLISHABLE_KEY` | The project's publishable key (Supabase &rarr; Project Settings &rarr; API keys |

Both values are public by design. The data is protected by Row Level Security and the MCP server's own checks.

The page depends on these Supabase settings:

| Supabase setting | Value |
|---|---|
| Authentication &rarr; URL Configuration &rarr; Site URL | the URL of this login page |
| Authentication &rarr; OAuth Server &rarr; Authorization Path | `/supabase-login/` |

Site URL plus Authorization Path must exactly match the page's address.

## Deploying 

GitHub Pages publishes the `main` branch. Commit a change to `index.html` and the live page updates within a few minutes. Reload with Ctrl+F5 to bypass browser cache.

## Troubleshooting

| Error | Check |
|---|---|
| "Failed to fetch | `SUPABASE_URL` or `SUPABASE_PUBLISHABLE_KEY` is wrong, or a browser extension blocks the request. DevTools (F12) &rarr; Console shows the failing address |
| Blank Page | A script error. Devtools &rarr; Console shows it. |
| Error after clicking Allow, in Claude | Supabase &rarr; Logs &rarr; Auth, at `/oauth/token` |
| "Invalid login credentials | Wrong email or password. An admin can set a temporary password in Supabase &rarr; Authentication &rarr; Users |
