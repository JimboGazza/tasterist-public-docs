# Tasterist Development Documentation

## Purpose

This document captures how Tasterist has evolved, how the codebase is structured, and how to maintain release-quality changes safely.

It is intended to be GitHub-facing project documentation and should be updated alongside major product changes.

## Product Scope

Tasterist is an operations-first web app for class/taster administration:

- Dashboard, Today, Month, and Stats operational views.
- Add Taster and Record Leaver flows, including simplified class-first/date-second taster booking.
- Find Taster search/edit/reschedule workflow.
- Admin Tasks workflow for attendance/admin checklist completion, hotlead/payment follow-up rails, and Recently Completed / Archive review.
- Mover workflow for class transfers.
- Hotleads pipeline for waiting-list contact management, Facebook leads, starter interest, and popup-first actioning.
- Overdue Payments workflow for payment follow-ups, contact history, Search/Admin Tasks integration, and left-club leaver handoff.
- Staff Overview and profile pages for manager/admin workload oversight.
- Team and personal to-do workflows, including a dedicated Elle board and extended personal priority boards.
- Account management with role controls and password security gates.
- Help / suggestion reporting with owner email delivery and in-app reply threading.
- Email scheduling/reporting via direct Purelymail SMTP + Render cron.
- Import workflows for external class data.
- Class CSV upload workflow for structured class-data ingestion.
- Tablet tools: whiteboard, stopwatch/timer, deck of cards/Ring of Fire, true/false, colour score game, resources, and joke of the day.
- Willow's Corner: role-specific view with insurance notice feature.
- Backup Centre for admin data snapshots.
- Public marketing landing page.

## Architecture Overview

- `app.py`: Primary Flask application (routing, domain logic, data access, auth, reporting, and import orchestration).
- `templates/`: Server-rendered Jinja templates.
- `static/style.css`: Single stylesheet for app visuals and layout behavior.
- `docs/`: Runbooks, deployment notes, release history, and QA checklists.
- `scripts/`: Maintenance and utility scripts (`update_release_history.sh`, import tools, etc).
- `VERSION`: Canonical app version shown in login UI.

## Data Model (High-Level)

- `tasters`: Child taster records and checklist flags (attendance, club fees, BG, badge).
- `leavers`: Child leaver records and checklist completeness.
- `users`: Auth accounts with roles and password-change enforcement flags.
- `user_admin_days`: User ownership mapping for admin day/programme cells.
- `class_sessions`: Timetable/session templates.
- `audit_logs`: App-level operational and security actions.
- `app_settings`: Feature switches and environment-backed behavior.
- `support_reports`: Stored help / suggestion / bug reports, including reporter details, source path, body, and attachment metadata.
- `support_report_replies`: Linked reply records for operator responses, including recipient, body, attachment metadata, draft/sent/failed state, and timestamps.
- `class_data_slots`: One row per class slot for external class CSV freshness, expiry day, and latest upload state.
- `class_data_uploads`: Append-only upload snapshots for each class slot.
- `class_data_rows`: Parsed child-level class data rows kept separate from operational taster records.
- `chasing_payments`: Parent overdue-payment follow-up records with status, owner label, and note state.
- `chasing_payment_contacts`: Child contact-history rows for calls and voicemails linked to chasing payment records.
- `admin_task_archives`: Restorable archive rows for Admin Tasks items.
- `dashboard_todos` / `dashboard_section_todos`: Normal personal To-Dos and extended priority-board To-Dos.
- `hotleads`: Waiting-list leads, including optional contact details, Facebook source flag, completion state, and starter-interest links.
- `move_requests`: Class move and starter-interest requests, including linked source hotleads where applicable.
- QA database routing: fixed QA accounts can be routed to the live DB or an isolated celebrity-seeded SQLite test DB through `qa_database_mode`.

## Security and Access Controls

- CSRF enforced on write operations.
- Login rate limiting and lockout windows.
- Role checks centered on `admin`, `manager`, `staff`, `willow`, `elle`, and `tablet`.
- Legacy `owner` compatibility remains in selected code paths, but the live role model now centers on `admin`.
- Taster edit/reschedule is available to signed-in non-tablet users; destructive taster deletion remains admin-controlled.
- Admin Console includes an admin-only QA Tools panel for fixed QA accounts:
  `qa-staff@tasterist.test`, `qa-willow@tasterist.test`, `qa-manager@tasterist.test`,
  `qa-elle@tasterist.test`, and `qa-tablet`.
- QA Tools are intentionally exact-match only: they can create/reset the fixed QA accounts, force non-tablet QA password resets, restore QA admin-day defaults, and audit every change, but cannot target real users.
- QA database mode is also exact-match protected: only the fixed QA account usernames can be routed to the isolated test database.
- Mandatory password-change flow for first login / default-password accounts.
- Emergency owner credential path controlled by env/secret values.

## Development Timeline Summary

The complete commit-by-commit history is in `docs/RELEASE_HISTORY.md`. Major phases:

1. Foundations and UI baseline.
2. Import reliability and schedule handling.
3. Security hardening and startup stability.
4. Postgres runtime + admin flow stabilization.
5. Cloudflare email pipeline and diagnostics.
6. Dashboard/day/month/admin UX iteration and layout scaling.
7. Find Taster performance and quality-of-life tooling.
8. Stats/dashboard metric realignment and operational reporting updates.
9. Post-launch: mover workflow, hotleads pipeline, branding, dashboard redesign.
10. Email infrastructure migration (Cloudflare → Purelymail SMTP + Render cron).
11. Public marketing landing page.
12. Tablet tools suite (whiteboard, stopwatch/timer, deck of cards, true/false, resources).
13. Support reply workflow and admin console / class upload polish.
14. Chasing Payments workflow with Search/Admin Tasks integration.
15. Full otter branding rollout, role reset, popup-first Hotleads, team workboards, and performance passes.
16. May 2026 QA follow-up: Staff Overview, archive-aware Admin Tasks, Search cleanup, Hotlead starter interest, extended personal To-Do boards, tablet persistence, and Padel Americano scheduling.
17. Late May 2026 polish: Request to Move callbacks, staff To-Do board links, QA test DB mode, tablet resources/colour game/jokes, Overdue Payment left-club flow, and Padel playoff/fairness improvements.

## May 2026 QA Follow-Up

The May 2026 pass was driven by a full live-app QA checklist and operator review. It tightened safety, reduced visual clutter, and made the busiest pages fit real laptop/desktop screens with internal scrolling instead of page sprawl.

### Staff Overview

- `/staff` is the manager/admin/Elle overview for operational staff workload.
- Staff/willow/tablet users are blocked from the all-staff list; optional own-profile behavior remains scoped by route checks.
- QA and admin/service accounts are filtered out of the staff grid unless they are a real operational account that needs to appear.
- Staff cards show admin task counts, overdue admin work, payments, and claimed hotleads over the same recent window as Admin Tasks.
- Staff profile pages combine scoped Admin Tasks, To-Do, Hotleads, Overdue Payments, and stats in a brand-aligned layout.
- Staff profile To-Do is intentionally simple: `Add task` plus `Open Board` / `Open List`.
- Opening a staff member's list routes to `/staff/<id>/todo`, giving managers/admins a full board/list view rather than a cramped embedded cell.
- Added staff tasks land in the correct extended priority section or normal To-Do list based on the target account's setting and selected priority.

### Admin Tasks

- `/admin/tasks` supports `archive=only`, preserving `view` and `staff_user_id`.
- The bottom section is now `Recently Completed / Archive`, built from archived rows plus recently completed operational items in the three-month lookback.
- All restore buttons are archive-only; completed rows remain non-restorable.
- Hotleads Due and Payment Chases were moved into the right rail so the main To Action queue can focus on tasters, leavers, movers, insurance, and move requests.
- Contacted tasters use a reduced primary action set: Contacted, Reschedule, Open, plus secondary actions in More.

### Search and taster lifecycle

- Search has one always-visible main query input, URL-synced search state, clearer helper copy, and labelled row actions.
- Admin Cleanup Mode remains admin-only with typed confirmation, a 25-row cap, and audit logging.
- Role-safe remove/archive paths exist from Day view, Search, and Edit Taster, while tablet users remain blocked from desktop destructive actions.
- Tasters created from hotleads are included in Search so staff can reschedule them by name.
- Add/reschedule taster now selects class day/time first, then exact date on the next step; Confirm Taster makes location and cross-location swap clearer.

### Hotleads and starter interest

- Hotleads uses an Admin Tasks-style layout: active leads in a large scrollable left list, claimable leads in the top-right panel, stats below.
- Add Hotlead supports optional Facebook/contact fields: parent name, email, phone, child name, and child age.
- Facebook rows keep the Facebook indicator but avoid crowding cards with contact pills; full contact details are available where follow-up happens.
- Register Interest creates a linked Starter move request and marks the hotlead with a requested-callback style outcome while keeping notes and history.
- Starter requests can reactivate the linked hotlead from zero contacts without losing history.

### Extended To-Do

- The Extended To-Do preference only controls personal To-Do routing/display.
- Elle is always routed to the extended board; other enabled users go to `/my-todo`, while disabled users keep the modal.
- Extended sections have editable names, priority levels, dashboard visibility, and internal scroll.
- Existing normal To-Dos migrate into extended sections on first use so switching an existing account does not lose data.
- Managers/admins can view staff extended boards through Staff Overview without granting the staff member broader permissions.

### Request to Move callbacks

- `/move-requests` separates normal move requests from `Callbacks / Starters`.
- `Add Callback` creates a starter callback item with exact DOB, programme/facility, requested day, callback due date, and optional notes.
- Callback due dates feed Admin Tasks for the user who added the callback when the due date arrives.
- Callback rows support contacted toggles, schedule-taster handoff, postpone callback, archive, notes, and linked-hotlead reactivation.
- Callback/starter rows use the same natural row height and packed-top scroll behavior as normal move requests.

### Tablet, Padel, and Stats

- Tablet Ring of Fire / deck state saves locally and flushes server-side so accidental navigation does not reset the activity.
- Public `/resources` is available without login, while `/tablet/resources` remains the tablet-focused entry point.
- Daily Spark uses Joke of the Day instead of historical “On This Day” entries.
- Colour Score Game supports tablet-scale scoring, negative totals, local persistence, and distinct team icons.
- Padel supports Americano-style personal scoring for small groups, multi-court fill-first scheduling, court labels, anti-repeat sit-out balancing, playoff/final structure, and compact score controls.
- Stats keeps the main graph prominent, with Monthly Breakdown and Admin Days moved into lower collapsible panels.

### QA test database

- Admin settings include a QA database toggle for fixed QA accounts only.
- `Beyonce` labels the live DB mode.
- `Dolly Parton` labels the isolated celebrity-populated test DB mode.
- The test DB is seeded with celebrity-style dummy names and operational rows for realistic QA without polluting live data.

### Overdue Payments

- Completion options are `Paid`, `Payment Plan`, and `Left the club`.
- `Left the club` completes the active reminder and provides an `Add as leaver` handoff into Record Leaver.
- Completed payment reminders leave live Overdue Payments and Admin Tasks queues.

## Operator Request Timeline (Recent Sprint)

Recent delivery cycles were driven by production-like operator feedback. Main themes:

1. Dashboard and stats graph readability and placement of tasters/leavers context text.
2. Month/day/add/admin layout scaling so cards align to viewport without outer-page scroll.
3. Uniform card sizes and per-column/day internal scrolling instead of global page overflow.
4. Sidebar active-page indicator simplification (dot-based state cue).
5. Admin action-bar grouping/dividers and consistent button sizing.
6. Day-view row redesign with larger clickable name lane routing to Find Taster.
7. Duplicate session cleanup (AM/PM mirror issues), with safer dedupe and non-destructive correction rules.
8. Add/Leaver flows simplification (class naming cleanup, removed low-value “upcoming” signals).
9. Reschedule UX upgrades (date options in weekly cadence and cross-location behavior fixes).
10. Email preference/layout refinements in settings.
11. Data safety guards around duplicate leavers and import-side normalization.
12. Ongoing cross-device scaling adjustments for desktop monitor behavior.
13. Find Taster responsiveness improvements and filter model refinements.
14. Current hardening pass for deployment readiness.
15. Reply-to-Suggestion workflow so owner/operator can respond directly from stored reports without losing original context.
16. Full performance pass across Search, Month, Day, Dashboard, Stats, Hotleads, and Admin Tasks.
17. Popup-first Hotlead workflow, with modal history/notes/actions and reduced full-page redraws.
18. Role reset across admin/manager/staff/willow/elle/tablet, including the dedicated Elle board and wider role-based access cleanup.

## Reply to Suggestion Workflow

This workflow starts from the existing help / suggestion email flow and adds a stored thread model so the owner can reply quickly from inside Tasterist.

### Flow Summary

1. Signed-in staff submits a help / suggestion / bug report.
2. The app stores the report in `support_reports` before attempting email delivery.
3. The owner receives the usual report email, now with a `Reply to Suggestion` action link.
4. Opening that link loads an in-app reply screen with:
   - recipient prefilled from the original report sender
   - default editable thank-you reply
   - original report subject, sender, source path, body, and attachments visible while composing
   - file attachment support
5. Replies are stored in `support_report_replies` as drafts or sent/failed replies.
6. Outgoing reply emails include the operator reply and the original report content underneath for context.

### Persistence Model

- `support_reports` stores the original inbound report and delivery state for the owner-facing notification email.
- `support_report_replies` stores linked reply records by `report_id`.
- Attachments are stored as JSON metadata plus base64 content so they can be reused for resend/reply email delivery.
- Audit logs are written for draft save, send, and resend actions.

### Operator UX Notes

- The fastest path is `Quick Send Default`, which sends the default thank-you message unchanged.
- `Save Draft` keeps the thread attached to the original report so the owner can return later.
- Failed sends stay visible in reply history and can be resent from the same thread page.
- Admin Console now surfaces a `Recent Suggestions` list so the owner has an in-app inbox entry point, even without opening the email first.

## Class CSV Upload Workflow

This workflow provides owner/admin access to structured class-data uploads without merging them into the live taster workflow yet.

### Flow Summary

1. Open `Admin Console`.
2. Use `Upload Classes` to open the class upload grid.
3. Choose a programme and click a class slot tile.
4. A modal opens with:
   - selected class/day/time
   - CSV file picker
   - `Done`
   - `Cancel`
5. Upload validation confirms the selected slot matches the CSV metadata.
6. Successful uploads append a snapshot and refresh the slot state.

### UI Notes

- The upload grid intentionally uses the same class-tile sizing language as Add Taster / Add Leaver so the page feels familiar.
- Slot status is shown by stronger border states:
  - green = fresh upload
  - orange = overdue refresh
  - neutral = awaiting first upload
- Each day column uses its own internal scroll region so dense programmes remain usable without growing the whole page.

## Overdue Payments Workflow

This workflow adds a manual payment follow-up lane at `/admin/payments`, reusing Hotleads/Admin Tasks/Search interaction patterns rather than introducing a new UI system.

### Access Model

- Allowed roles: `admin`, `manager`, `staff`, `willow`, `elle`
- Blocked roles: `tablet`, anonymous
- `admin` and `manager` can see all payment follow-ups.
- `staff`, `willow`, and `elle` work from their own view, while Search can still surface broader record visibility.
- Allowed roles can create records and log contacts; the page now centers on operational follow-up rather than legacy admin-only ownership rules.

### Persistence

- `chasing_payments` stores the main record:
  - child
  - class day
  - programme
  - cached owner label
  - creation note
  - active/completed/completed-plan status
  - created/updated/completed/reactivated timestamps
- `chasing_payment_contacts` stores each logged contact event:
  - type (`call` / `voicemail` / `email`)
  - required note
  - creator
  - created/updated timestamps

### Page Structure

- `/admin/payments` shows:
  - summary cards for Active / Completed / Completed - Plan / Mine / All
  - active list first
  - collapsible completed list underneath
  - Add Overdue Payment modal
  - Add Contact modal
  - Payment Plan modal
  - Edit modal
  - History modal

### Integration Points

- `Admin Tasks`
  - active payment chases appear as payment follow-up rows with an `Open` button
- `Search`
  - payment chases appear as payment record types with `Open`, `View History`, and `Delete`
- shared child autocomplete
  - create/edit child fields use the same `/child-name-suggestions` endpoint as other child-entry flows

### Contact Indicator Rules

- newest 4 contacts render inline
- calls use a green pill
- voicemails use an amber mic-style pill
- emails use an envelope-style pill
- extra contacts collapse into `+N`
- tooltip text includes:
  - contact type
  - UK-formatted date/time
  - user who logged it

## Launch Day Update (2026-02-27)

This launch-day wave is the largest UX + reliability pass so far. It is intentionally broad and focused on operational confidence under real admin usage.

### Primary Outcomes

- End-to-end UI scaling stabilized across Dashboard, Month, Today/Day, Add, Leaver, Find Taster, and Admin Tasks.
- Data safety significantly improved around mirrored duplicates, reschedule edge cases, and duplicate leaver handling.
- Find Taster moved from “heavy and fragile at zoom” to “stable and fast enough for daily admin use”.
- Reschedule behavior now supports same-day shifts (for example +30 minutes) without false duplicate blocking.

### Key Launch-Day Deliverables

- Month-view initial circles now communicate outcome state for past tasters (missed/attended/fully complete).
- Find Taster list redesigned to one-line names, controlled note rendering (`View Note` modal), and fixed action lanes.
- Add/Leaver week grid sizing normalized with consistent tile/day widths and reduced layout drift.
- Sidebar active marker simplified to border-only state (dot removed) for cleaner navigation feedback.
- Settings and support workflows refined for cleaner control placement and better operator confidence.
- Admin Tasks spacing, grouping, overflow behavior, and to-action semantics refined.
- Day rows redesigned for larger click targets and direct Find Taster deep links.

### Bug-Fix Concentration Areas

- Repeated chart growth/render regression in stats.
- Month bottom-row clipping and jump-panel overlap.
- Residual shell scrolling on add/admin pages.
- Button-size inconsistency across action groups.
- Reschedule same-day duplicate false positives.
- Search clear/interaction rough edges in Find Taster link-through flows.
- Import-driven AM/PM duplicate noise and non-destructive correction logic.

## Tablet Tools Suite

Five tablet-optimised tools for class/session use, all accessible from the tablet nav bar.

### Tools

- **Tablet setup** (`templates/tablet_setup.html`) — shared tablet account location picker. First use asks staff to choose Honley or Lockwood, stores the choice in a cookie, and exposes a Change Location route for deliberate resets.
- **Whiteboard** (`templates/tablet_whiteboard.html`) — freehand canvas backed by location-scoped `app_settings` keys, with legacy fallback from `users.tablet_whiteboard_data`. Honley and Lockwood/Preschool tablets keep separate boards. Saves on draw-end, loads on open. Clear resets to blank and saves the empty state.
- **Stopwatch / Timer** (`templates/tablet_stopwatch_timer.html`) — dual-mode: countdown timer (user sets duration) and running stopwatch. No server state; fully client-side.
- **Deck of Cards** (`templates/tablet_deck_of_cards.html`) — shuffles a full deck and draws one card at a time. Useful for games and warm-ups.
- **True / False** (`templates/tablet_true_false.html`) — large-display binary answer tool for class activities.
- **Resources** (`templates/tablet_resources.html`) — curated content list driven by `tablet_resources_content.py` (keeps content out of the template).

### Design Notes

- All tools share the same tablet-first CSS language and are shown only when `is_tablet_user` is true.
- Tablet location preference drives tablet defaults: Honley uses Honley only; Lockwood uses Lockwood plus Preschool.
- Whiteboard data is stored server-side so it persists across sessions and devices for the same tablet location.
- The resources content module (`tablet_resources_content.py`) is a plain Python list of dicts, making it easy to add/reorder entries without touching HTML.

## Hotleads Pipeline

A waiting-list contact management pipeline. Access is controlled by `hotleads_access_mode()` which reads from the `app_settings` table (admin-configurable, not an env var).

### Key Behaviours

- Entries move through pipeline stages; status toggles are fetch-based (no page reload).
- Completing a hotlead is triggered automatically when a taster is scheduled from it.
- Hotlead stats surface in the Open Admin Tasks section.
- Completed hotleads are filterable and searchable.
- Creation is limited to manual adds (not triggered by imports).

### Data

- `hotleads` table: one row per contact with stage, notes, and timestamps.
- Dashboard card scrolls internally so it doesn't push other grid rows.

## Mover Workflow

A record-keeping workflow for members transferring between classes. Available to all signed-in users.

### Flow

1. Pick source taster record (from session).
2. Pick target class/session.
3. Mover row created; appears in day view and admin tasks.

### Notes

- Source selection persists across reloads (localStorage).
- Mover rows shown wider in day view than taster rows to distinguish them visually.
- Postgres-safe: date union queries patched for psycopg2 compatibility.
- Move Requests page (`templates/move_requests.html`) gives an overview of pending/completed moves.

## Email Infrastructure

### Current Stack (as of Apr 2026)

- **Sending**: Direct SMTP via Purelymail (`noreply@tasterist.com`).
- **Scheduler**: Render cron job triggers the weekly report endpoint.
- **Cloudflare worker**: Removed entirely.

### What Changed

The Cloudflare worker was removed in favour of direct Purelymail SMTP because the worker approach added an indirection layer that complicated debugging and deployments. The Render cron approach keeps email delivery entirely within the existing infrastructure.

### Weekly Reports

- Sent to opted-in users on their configured day/frequency.
- Template widened to 700px; By Programme section collapsed to one line for readability.
- Owner-only mode respected.

## Backup Centre

`templates/backups.html` — admin-accessible page for manual data backup actions. Accessible from the Admin Console. Provides download triggers for DB snapshots without requiring Render dashboard access.

## Public Marketing Landing Page

`templates/landing.html` — standalone marketing page served at `/`. Not part of the authenticated app shell. SEO metadata, social links, and feature copy maintained separately from the app.

## Release and Documentation Process

For each feature set:

1. Implement minimal-risk, isolated changes.
2. Run smoke checks and route-level sanity tests.
3. Update release history using:
   - `bash scripts/update_release_history.sh`
4. Update this document when a sprint materially changes product behavior.
5. Commit and push so GitHub remains the source of truth.

## QA Expectations Before Deploy

Minimum pass criteria:

- Auth/login and first-password gate behavior verified.
- Help / suggestion submit and reply flow verified, including draft save, send, and failed resend path.
- Add Taster flow (date suggestion, save, duplicate guardrails) verified.
- Day view checklist actions and Find Taster deep-linking verified.
- Find Taster search/filter interactions performant under representative data volume.
- Admin Tasks and layout/scroll behavior verified at default browser zoom.
- Stats/dashboard metric cards and graph labels verified for correctness.
- No unhandled server errors in add/edit/leaver workflows.

## Maintenance Notes

- `docs/RELEASE_HISTORY.md` is generated from git history.
- `docs/DEVELOPMENT_DOCUMENTATION.md` is intentionally curated and should include rationale, not only commit messages.
- If deployment behavior changes (Render/Cloudflare/env secrets), update corresponding runbooks in `docs/` in the same pull request.
