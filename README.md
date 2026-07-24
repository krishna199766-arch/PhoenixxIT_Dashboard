# Phoenixx IT — Client Pipeline Dashboard

A single-file, zero-dependency sales CRM for tracking the Phoenixx IT client
pipeline — from discovery to onboarding. Everything (UI, logic, styling, data
persistence) lives in one HTML file that runs straight in the browser.

**File:** [`phoenixx-it-dashboard_1.html`](phoenixx-it-dashboard_1.html)

---

## Features

- **Kanban pipeline board** — drag clients across six stages: Discovery, Approached,
  Meeting Scheduled, Proposal Shared, Onboarded/Converted, Not Converted. Every move
  is recorded in the client's activity timeline.
- **Client records** — company, contact, services, budget, lead source, priority,
  rating, tags, follow-up dates, attachments and notes.
- **Analytics** — monthly leads, conversion funnel, industry & lead-source breakdown,
  services in demand, team load, and (Admin only) proposal value by stage & revenue closed.
- **Calendar** — meetings, follow-ups, proposals and expected close dates by month.
- **Search & filters** — by stage, industry, priority, source, owner, follow-up window,
  value and date range.
- **Notifications** — follow-ups due, overdue items, meetings today, stale proposals.
- **Export** — CSV (Excel), JSON backup/restore, and printable proposal summaries.
- **Local persistence** — data is saved in the browser; no server required.

---

## Roles & access control

The dashboard has a sign-in gate with two roles:

| Role | Passcode | Sees pricing? |
|------|----------|---------------|
| **Admin** | Required | ✅ Full access, including all pricing & revenue |
| **Manager** | None | ❌ Everything **except** pricing |

**"Pricing" hidden from Managers** covers: card values, the "Pipeline value" &
"Revenue closed" stat tiles, the "Proposal value by stage" & "Revenue closed"
analytics panels, the Budget & Proposal-amount form fields, budget/amount columns
in CSV export, and the value line in downloaded proposals. Managers keep full access
to everything else (clients, contacts, stages, notes, calendar, follow-ups).

### Signing in

- **Manager:** enter your name, leave **Manager** selected, click **Continue**.
- **Admin:** enter your name, choose **Admin**, enter the passcode, click **Continue**.

You can switch roles or sign out anytime from the account menu (avatar, top-right).
Switching to Admin re-prompts for the passcode. Admin is never auto-restored on
reload — pricing stays locked until you sign in again.

### Changing the Admin passcode

Edit the `ADMIN_PIN` constant near the top of the `<script>` in
[`phoenixx-it-dashboard_1.html`](phoenixx-it-dashboard_1.html):

```js
const ADMIN_PIN = "phoenixx2026";
```

> **Note:** This is client-side gating suitable for an internal browser tool. The
> passcode lives in the page source, so treat it as light access control, not
> hardened security. For true enforcement, put the pricing behind a server API.

---

## Usage

No build step, no install. Just open the file:

1. Download or clone the repo.
2. Double-click `phoenixx-it-dashboard_1.html` (or open it in any modern browser).
3. Sign in and start managing your pipeline.

Sample data loads automatically on first run. Use the account menu to load sample
data, clear all data, or back up / restore from a JSON file.

---

## Tech

Plain HTML, CSS and vanilla JavaScript — no frameworks, no dependencies, no network
calls. Data persists via the browser's local storage.
