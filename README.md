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
- **Persistence** — saves to your browser by default, or to a shared cloud database
  (Supabase) so the whole team sees one live pipeline. See *Data storage & team sync*.

---

## Roles & access control

The dashboard has a sign-in gate with two account types:

| Account | Signs in with | Sees pricing? | Can manage users? |
|---------|---------------|---------------|-------------------|
| **Admin** | The shared admin passcode | ✅ Full access, incl. pricing & revenue | ✅ Yes |
| **User** | Their own username + passcode (created by the Admin) | ❌ Everything **except** pricing | ❌ No |

**"Pricing" hidden from Users** covers: card values, the "Pipeline value" &
"Revenue closed" stat tiles, the "Proposal value by stage" & "Revenue closed"
analytics panels, the Budget & Proposal-amount form fields, budget/amount columns
in CSV export, and the value line in downloaded proposals. Users keep full access
to everything else (clients, contacts, stages, notes, calendar, follow-ups).

### Creating user logins (Admin only)

1. Sign in as **Admin** (using the admin passcode).
2. Open the account menu (avatar, top-right) → **👥 Manage user logins**.
3. Enter a **username** and a **passcode**, click **Add user**. Share those two
   values with your team member.

**Forgot a passcode?** The Admin can see every user's passcode (🔑) in *Manage
user logins*, so just read it there — or click **Reset** next to a user to set a
new one.

User accounts are stored in the shared cloud database, so a login created by the
Admin works on any device. Only the Admin can create, view, reset or remove user
logins, and load/clear sample data — Users don't see those options.

### Signing in

- **User:** pick **User**, enter your username + passcode, click **Continue**.
- **Admin:** pick **Admin**, enter the admin passcode (name optional), click **Continue**.

Sign out anytime from the account menu. Admin is never auto-restored on reload —
pricing stays locked until you sign in again.

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

## Data storage & team sync

By default the dashboard saves to **your browser's local storage** — data stays on
that one device and isn't shared. To let your **whole team share one live pipeline**,
connect a free [Supabase](https://supabase.com) database (5-minute, one-time setup):

1. **Create a project** at [supabase.com](https://supabase.com) → *New project*.
2. **Create the table** — open *SQL Editor*, paste this, and click *Run*:

   ```sql
   create table if not exists pipeline_state (
     id text primary key,
     data jsonb,
     updated_at timestamptz default now()
   );
   alter table pipeline_state enable row level security;
   create policy "public access" on pipeline_state
     for all using (true) with check (true);
   ```

3. **Get your keys** — *Project Settings → API*. Copy the **Project URL** and the
   **anon / public** key.
4. **Paste them into the dashboard** — near the top of the `<script>` in
   [`phoenixx-it-dashboard_1.html`](phoenixx-it-dashboard_1.html):

   ```js
   const SUPABASE_URL  = "https://YOUR-PROJECT.supabase.co";
   const SUPABASE_ANON = "YOUR-ANON-PUBLIC-KEY";
   ```

5. **Save, commit and push** — Vercel redeploys and the whole team now shares the
   same board. Changes from teammates appear automatically (auto-refresh every 15s).

Leave `SUPABASE_URL` / `SUPABASE_ANON` blank and it keeps working in local-storage
mode on each device.

> **Security note:** the `anon` key and `ADMIN_PIN` live in the page source, and the
> SQL policy above allows public read/write. That's fine for a small internal team
> behind a private URL, but anyone with the link can read/write the data. For stricter
> control, add Supabase Auth and tighten the row-level-security policy.

---

## Tech

Plain HTML, CSS and vanilla JavaScript — no build step, no framework. Data persists in
the browser's local storage by default, or in a shared [Supabase](https://supabase.com)
Postgres database when configured (loaded via CDN).
