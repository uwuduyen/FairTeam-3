# FairTeam

Real account-based FairTeam app. New users register themselves and start with an empty workspace.

## Local
1. Node 24.x
2. `npm install`
3. `npm start`
4. Open http://localhost:3000
5. Use **Đăng ký** to create a new account.

## Google Sign-In
Set these environment variables before `npm start`:
- `GOOGLE_CLIENT_ID`
- `GOOGLE_CLIENT_SECRET`

Google OAuth callback:
`http://localhost:3000/api/auth/google/callback`

For Render, add the same two variables in the service environment. The callback becomes:
`https://YOUR-RENDER-DOMAIN/api/auth/google/callback`

## Background sync
FairTeam refreshes team/task/review data every 5 seconds while the user is active. When a Leader assigns a task, the assignee's My Tasks is updated automatically without a manual reload.

## Shareable invites
Leaders can generate an invite link without entering a Gmail address. The recipient opens the link, creates/logs into a FairTeam account, and is added as a Member automatically.

## Sample workspace
The real build includes **Load sample team (5 people)** on an empty My Teams workspace. It creates one Leader (the current user) plus four sample members and example tasks.

A separate sample startup is also included:
`npm run sample`
Sample login: `sample@fairteam.local` / `123456`

## Core flow
Plan → Work → Evidence → Leader Review → Fair Contribution → Report

## Stack
Node.js + Express + SQLite locally; PostgreSQL + persistent sessions on Render when `DATABASE_URL` is provided.

## Important
Do not use `npm audit fix` as part of setup. If an old local database exists and you want a completely fresh workspace, stop the server and remove `database/fairteam.db`, then start again.


## Production persistence / Render

FairTeam uses PostgreSQL when `DATABASE_URL` is present, and SQLite only for local development. The Render Blueprint intentionally uses a paid Render Postgres plan (`0.1c-256mb`) so the database is not the 30-day Free Postgres instance. The web service may remain on the Free plan for low-cost testing; the database is the durable source of truth. For a more responsive always-on public app, upgrade the web service as well.

Before going public, set `SESSION_SECRET`, `GOOGLE_CLIENT_ID`, and `GOOGLE_CLIENT_SECRET` in Render. Never commit these secrets to GitHub.
