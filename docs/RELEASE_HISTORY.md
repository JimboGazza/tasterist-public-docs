# Tasterist Release History

Auto-generated from git history.

## Snapshot

- Current app version file: `1.1.0`
- Latest documentation refresh: `2026-05-31`
- Current app HEAD at refresh: `7a6a66a` (`2026-05-29`)
- Total app commits at refresh: `392`
- Current release line: `Late May 2026 workflow fit, tablet resources, Staff To-Do cleanup, and Request to Move callback polish`
- Product state: otter-only branding, role-aware operations, Staff Overview, archive-aware Admin Tasks, popup-first hotleads, extended personal to-do boards, cleanup mode, persistent tablet tools, public resources, Padel tournament tooling, and QA database routing

## Late May Workflow Fit and Tablet Resources (2026-05-17 → 2026-05-29)

This wave followed the 16 May QA follow-up and focused on the remaining rough edges: To-Do consistency, Request to Move callback handling, tablet resources, payment completion outcomes, QA database routing, and Padel tournament usability.

### Staff Overview and To-Do standardisation

- Staff profile To-Do panels were simplified into a clear `Add task` command plus an `Open Board` / `Open List` link.
- Managers/admins can open a staff member's personal To-Do board/list at `/staff/<id>/todo`, seeing the same board structure the staff member would see for their own list.
- Extended users keep priority-board routing; normal users keep the standard list.
- Adding a staff task from the profile sends it to the correct extended section or normal list based on the chosen priority.
- Normal To-Do and extended-board tests were refreshed so switching accounts to extended mode preserves existing entries and due dates.

### Request to Move callbacks/starters

- `/move-requests` gained an `Add Callback` flow using the starter-interest pattern: child name, exact DOB, programme/facility, requested day, callback due date, and optional notes.
- Callback/starter requests are split from normal move requests into a separate side-by-side list and can be filtered by request/callback type.
- Callback due dates place the item into the adding user's Admin Tasks when due.
- Callback rows support contacted toggles, schedule-taster handoff, postpone callback, archive, notes, and linked-hotlead reactivation where applicable.
- Callback/starter row scaling now matches normal move-request cards instead of stretching sparse rows to fill the whole cell.

### Tablet resources and Daily Spark

- Tablet Daily Spark now uses Joke of the Day instead of “On This Day”.
- Daily Spark clipping around the tablet home button was fixed, and global non-selectable UI text was tightened for tablet use.
- `/resources` is publicly accessible without login; tablet users still use `/tablet/resources`.
- The old Interval Timer and Random Exercises resource cards were removed.
- Colour Score Game was added to tablet resources, scaled for tablets, supports negative scores, saves locally, and includes a green-team leaf icon.
- Ring of Fire/deck state remains persistent across accidental navigation.

### QA database mode

- Admin settings include a QA database mode toggle for fixed QA accounts.
- QA accounts can use the live DB (`Beyonce`) or an isolated celebrity-populated test DB (`Dolly Parton`).
- The setting applies across the fixed QA account set and is exact-match protected so real accounts are not routed into QA tooling.

### Overdue Payments

- Payment completion choices now include `Paid`, `Payment Plan`, and `Left the club`.
- `Left the club` completes the payment reminder and provides a direct `Add as leaver` handoff into Record Leaver.
- Completed left-club/payment-plan outcomes leave the active payment page and Admin Tasks follow-up queue.

### Padel tournament scoreboard

- Americano scheduling was improved for 11-player and small-player tournaments, with fairer sit-out rotation and court fill-first behavior.
- Playoff/final structure was added so league scoring remains fair while finals crown a separate winner.
- Leaderboards show fairness context such as played/remaining matches.
- Score controls were compacted into plus/minus controls with bold numeric scores.
- Results export and tournament history were expanded so played games can be copied or revisited.

### Hotleads and contact management

- Facebook hotlead contact information is copyable where staff need to follow up.
- Hotleads can be edited from the popup/search flows for name, age, and saved contact details.
- Starter-interest and requested-callback states preserve notes/history while allowing reactivation when needed.

### Recent app commits covered by this refresh

| Commit | Date | Summary |
|---|---:|---|
| `7a6a66a` | 2026-05-29 | Refine staff todo boards and move request callback layout |
| `8a578c2` | 2026-05-27 | Add left club payments and tablet polish |
| `3d65dff` | 2026-05-22 | Add colour game and tidy extended todos |
| `ec2ea04` | 2026-05-22 | Polish QA flows and task management |
| `fe13d38` | 2026-05-20 | Fix tablet spark clipping and selection |
| `29a52bc` | 2026-05-20 | Replace tablet daily spark history with jokes |
| `2bfaf5a` | 2026-05-19 | Implement remaining Tasterist UI and data flow improvements |
| `0caa494` | 2026-05-17 | Rework padel leaderboard layout |
| `bfb3341` | 2026-05-17 | Improve Americano playoff scoring |

## QA Follow-up, Staff Overview, and Workflow Fit (2026-05-10 → 2026-05-16)

This wave followed a full live-app QA pass and focused on safer cleanup, clearer manager oversight, tighter page-fit layouts, and fewer accidental dead ends in daily workflows.

### Staff Overview and manager oversight

- Added `/staff` Staff Overview and staff profile pages for admin, manager, and Elle-style oversight.
- Staff Overview excludes QA/admin service accounts while keeping real operational admin accounts visible where needed.
- Staff cards now focus on admin tasks, overdue admin work, payments, and claimed hotleads; email, workload, to-do, and due counters were removed for a cleaner grid.
- Staff profile pages now show brand-aligned KPI cards, scoped Admin Tasks, personal to-do, overdue payments, hotleads, and operational stats in a page-fit layout with internal scrolling where needed.
- Staff Overview admin counts now match the same live scoped Admin Tasks rules and three-month lookback used by `/admin/tasks?view=all&staff_user_id=<id>`.

### Admin Tasks and archive/completed workflow

- Added `archive=only` mode to `/admin/tasks`, preserving `view` and `staff_user_id`, so the completed/archive list can become the whole page when needed.
- Renamed the bottom section to `Recently Completed / Archive` and built it from archived rows plus completed tasters, leavers, movers, willow notices, hotleads, and payment chases from the existing three-month lookback.
- Restore actions now appear only for genuinely restorable archived rows; completed-only rows show a completed status without restore controls.
- Reworked the right rail into stacked `Hotleads Due` and `Payment Chases` lists, moving those items out of the main To Action queue and scaling the rail to the visible page height.
- Payment chase cards in Admin Tasks now show contact indicators and keep Add Contact / Open actions close to the row.
- Contacted taster rows now show the focused primary action set: Contacted, Reschedule, Open, plus the More menu for secondary actions.
- Added a confirmed permanent-delete path for admin-controlled taster cleanup from Admin Tasks where the destructive action already exists.

### Search, taster cleanup, and booking flow

- Search now has a persistent top search input, clearer helper text, URL-synced query state, labelled row actions, and role-safe cleanup controls.
- Admin cleanup mode remains admin-only with typed confirmation, a 25-record limit, and audit logging.
- Day view, Search, and Edit Taster gained clearer role-safe remove/archive actions with confirmation modals.
- Tasters created from hotleads now appear correctly in Search so they can be found and rescheduled by name.
- Add/reschedule taster flow was simplified: choose the class pattern first, then choose the exact date on the next step.
- Confirm Taster gained clearer location messaging, a back button, notes placement above wrong-location actions, and a working Honley/Lockwood quick-swap for matching class times.
- The Add Taster, Record Leaver, and Add Mover pages no longer show the shared To-Do button in their page headers.

### Hotleads and move-request starter workflow

- Hotleads leaderboard now excludes QA/admin accounts, keeps James' main account visible, centers rank markers, and presents the top three as a visual podium.
- Hotleads page now follows the Admin Tasks layout pattern: active hotleads in a large scrollable left list, claimable hotleads in the top-right panel, and scalable stats below.
- Claim Hotleads rows now keep Facebook indicators without crowding the card with parent/email/phone pills.
- Add Hotlead supports an optional Facebook mode with parent name, email, phone, child name, and child age, preserving child name as the only required field.
- Hotlead contact information is stored and shown where staff need it for follow-up.
- Added Register Interest from the Hotlead More menu, creating a Starter move request linked back to the hotlead, preserving notes/history, and allowing reactivation from zero contacts.
- Fixed Admin Tasks hotlead popup actions so selections save without the modal disappearing unexpectedly.

### Extended To-Do and personal boards

- Repurposed Extended To-Do so it only controls personal To-Do routing/display.
- Elle remains always on; other users with the setting enabled route To-Do buttons to `/my-todo`, while normal users keep the modal.
- `/my-todo` now provides a personal priority board with editable list titles, priority levels, dashboard visibility, notes, and page-fit internal scrolling.
- Dashboard and Staff Profile To-Do areas render the priority-led board for extended users while preserving the normal To-Do list for everyone else.
- Existing normal To-Do rows are copied into the extended board on first use so switching a real account over does not lose existing tasks.
- The extended board back button now returns to the page that opened it where a safe `next` target is available.

### Tablet, Padel, Stats, and page-fit polish

- Tablet Ring of Fire / deck state now persists across accidental navigation using local browser state plus server flushes.
- Padel gained Americano-style personal scoring for smaller groups, a visible/default-on Fill Courts First setting, court labels per round, and scheduling improvements to avoid repeated sit-outs where possible.
- Stats moved Monthly Breakdown and Admin Days into lower collapsible sections so the main graph and supporting dropdowns read more cleanly.
- Light/dark readability and viewport-fit polish continued across Admin Tasks, Hotleads, Staff Overview, Add Taster, and extended To-Do pages.

## Hotlead Reporting Alignment (2026-05-09)

- Added three calendar-month Hotlead metrics across operational views: `Claimed Hotleads`, `Booked Hotleads`, and `No Space Hotleads`.
- Admin Tasks now shows those three Hotlead month stats in summary cards and in each staff row within Open Admin Tasks.
- Hotleads and Stats now use the same calendar-month definitions for claimed, booked, and no-space outcomes.
- Dashboard Hotleads overview now includes this month’s claimed, booked, and no-space totals for the logged-in user.
- Request to Move cards now show who added each request directly in the row summary, and new requests store the same actor display format as the rest of the app.

## Sprint Wave: Operations Scaling, Cleanup Mode, and Tablet Consolidation (2026-04-25 → 2026-05-04)

Admin Tasks scaling, safer cleanup tooling, tablet account consolidation, and continued usability hardening.

### Admin Tasks scaling

- My Admin Days view now combines At a Glance and Workload Mix into one compact Workload Overview card so the task list stays dominant.
- All Days view keeps Open Admin Tasks in the right rail, stretches it to the usable page height, and exposes staff To-Do actions directly beside View.
- Open Admin Tasks staff rows now show Total first and remove redundant HL Due / Movers counters for a cleaner all-days overview.
- To Action rows were tightened for responsive scaling at browser zoom and smaller desktop widths.

### Cleanup and QA safety

- Admin-only Cleanup Mode added to Search with visible-row selection, preview, typed confirmation, a 25-record limit, and audit logging.
- The full testing checklist now covers cleanup paths, tablet location setup, Admin Tasks scaling, Search paging, popup workflows, and final data cleanup sweeps.

### Shared tablet account flow

- Tablet users now choose Honley or Lockwood on first use instead of requiring separate saved passwords.
- Tablet preference drives defaults across tablet pages: Honley stays Honley; Lockwood includes Lockwood and Preschool.
- Tablet whiteboards are now location-scoped so Honley and Lockwood/Preschool can keep separate saved boards.

### Search, stats, and navigation polish

- Search defaults to 50 records per page and keeps server-side paging/filtering.
- Stats sections became collapsible and remember browser state.
- Normal page navigation uses slimmer progress feedback while save/delete/import actions keep blocking feedback.

### QA hotfix follow-up (2026-05-05)

- Taster Edit is available to all signed-in non-tablet users, while destructive deletion remains admin-only.
- Day view taster menus now show a distinct Reschedule action so staff are not routed through Edit to move a booking.
- Deployment checklist now explicitly covers non-tablet taster edit/reschedule access and the admin-centered role model.
- Admin Console now includes fixed-account QA Tools for staff, willow, manager, elle, and tablet live-testing paths, with exact-match account protections and audit logging for every QA action.

## Sprint Wave: Operations Speed, Role Reset, and Workboards (2026-04-12 → 2026-04-24)

Performance, role simplification, popup-first hotleads, Elle workflow support, and tablet question-bank overhaul.

### Performance and page-weight reduction

- Major speed pass across Search, Month, Day, Dashboard, Stats, Hotleads, Admin Tasks, and Overdue Payments.
- Shared shell work (notes / to-do modal and repeated per-page payload) was trimmed or deferred so navigation feels faster.
- Search, dashboard, and admin-heavy surfaces were reworked to cut large initial payloads and reduce full-page redraws.

### Role reset and access cleanup

- Live role model centered on `admin`, `manager`, `staff`, `willow`, `elle`, and `tablet`.
- `Elle` role added as a manager-equivalent operational account with a dedicated workboard.
- Hotleads, Overdue Payments, and Move Requests moved further toward role-based access instead of older app-setting gating.
- Willow access expanded where needed for Insurance Notices and Overdue Payments.

### Team to-dos and Elle board

- Team to-do page added for manager/admin-style oversight.
- Elle workboard added with `Urgent`, `Important`, `When I Can`, `Monday Tasks`, and notes.
- To-do rows gained richer metadata (`Added by`, `Added`, `Completed`) plus stronger completed-state fading.
- Dashboard to-do behavior updated so Elle sees her urgent lane instead of the old generic personal list.

### Hotleads and Overdue Payments polish

- Hotleads moved toward a popup-first interaction model, with row click opening the modal directly.
- Hotlead history/note flows were tightened so users can move between popup, note, and history screens without losing context.
- Active hotlead follow-up steps were redesigned into clearer one-row step blocks.
- Unclaimed hotleads can now carry notes, with an indicator visible before claim.
- Overdue Payments gained added-date visibility, clearer paid/deleted/plan states, and a third `Email` contact method.

### Branding and tablet tools

- Otter branding became the single live brand direction and the old logo selector was removed.
- Tablet True or False was rebuilt with a larger question bank plus category/difficulty filters.

## Sprint Wave: Otter Branding & UI Polish (2026-04-11)

New logo system, landing page overhaul, role access fixes, and dashboard card improvements.

### New Otter Logo & Feature Flag

- New otter logo added (`tasterist-logo-otter.png` + 16/32/192/512 variants + `.ico`).
- `new_logo` app setting in Admin Console → Feature Access toggles the logo across sidebar, login page, landing page, favicons, and PWA manifest.
- `serve_manifest()` Flask route replaces the static `manifest.webmanifest` so Android PWA icons follow the toggle.
- SVG favicon suppressed when new logo is active (SVG takes browser priority over PNG/ICO — was blocking the otter favicon).
- Light-mode sidebar logo: CSS `filter` converts the image to dark green (`#123523`) matching sidebar text. Logo size 53px.

### Landing Page

- Scroll-down otter illustration (`otter-scroll.png`) replaces the chevron arrow. Fixed to viewport bottom; docks to hero section bottom on scroll. Only shown when new logo is enabled.
- Peeping otter (`otter-peek.png`) added above footer, sized 240px wide.
- Dividing line (`border-bottom` on peek wrap) between otter section and footer — `#2a3d30`, 4px.
- Footer background lightened to `#162a1c` with `#8ab8a0` text.
- Nav **Log in** button redesigned: pill-shaped with green gradient, subtle glow, hover effect.
- "Top 3 largest gymnastics clubs in the UK" trophy stat added to Pennine Gymnastics card.

### Role & Sidebar Fixes

- Admin Tasks sidebar link now visible to all non-tablet roles (`{% if not is_tablet_user %}`).
- `is_willow_user` added to global context processor.
- Fixed `AttributeError: 'method_descriptor' object has no attribute 'today'` in Admin Tasks — `datetime.date.today()` → `date.today()`.

### Admin Tasks Stats

- `hl_due` (overdue hotleads per staff, via `hotlead_timeline()`) and `payment_chases` (active chasing-payments rows matched to staff admin day assignments) added to Open Admin Tasks per-staff breakdown.

### Dashboard

- Hotlead lane cards (`align-content: start; align-items: start; align-self: start`) no longer stretch to fill the panel — fixed-height at all viewport sizes.

### Chasing Payments Flag

- `chasing_payments_access` app setting gates Chasing Payments for staff/manager/willow. Owners/admins always on.

## Sprint Wave: Post-Launch Growth (2026-02-27 → 2026-04-09)

This is the post-launch delivery sprint covering everything built after the Feb 27 launch wave. It spans six major feature areas across ~8 weeks of continuous iteration.

### Find Taster & Search Overhaul (Feb 27 – Mar 3)

- Find Taster switched to button-triggered search (no more live-typing lag).
- Search results layout redesigned: compact status icons, async toggle buttons (no page reload), flattened name cell, admin-day filter.
- `View Note` modal now contains row diagnostics (moved out of inline).
- Search table set to 80% zoom for density; row height controlled.
- Source badges (T/L) replaced with compact indicators.
- Day-to-Find-Taster deep-link error fixed.
- Clear/reset interaction fixed for Find Taster link-through flows.
- Leaver edit/delete actions added in Search.

### Mover Workflow (Mar 2–5)

- Full owner-only mover workflow added: pick from/to session, admin task integration, search visibility.
- Mover selection flow redesigned (cleaner two-step: source pick → target pick).
- Mover rows shown in day view with wider layout.
- Mover date unions hardened for Postgres.
- Cross-facility mover selection fixed.
- Preschool mover add schedule fallback fixed.
- Mover rows visible in admin tasks; opened to all signed-in users.

### Branding, Theming, and Settings (Mar 1–3)

- Tasterist logo added; favicons refreshed.
- Light theme fully rebuilt with new palette (matte month tiles, neutralized borders, warm accent removal).
- Dark/light theme toggle persisted per-user and cached locally.
- Settings page reflowed; email preferences card stabilized.
- Admin day headers darkened; light mode admin task contrast fixed.

### Stats Improvements (Mar 3)

- Monthly breakdown redesigned.
- Stats links from search improved.
- Signup logic fixed; month gaps filled.

### Hotleads Pipeline (Mar 10–31)

- Full hotleads workflow: pipeline view, status toggles (fast fetch-based, no reload), bin, completed view, search visibility.
- Hotleads access control moved into admin settings (`app_settings` table, not env var).
- Hotlead stats added to Open Admin Tasks section.
- Hotlead creation limited to manual adds (not import).
- Hotleads card and lists on dashboard scroll internally.
- Dashboard hotleads card with placeholder state.
- Hotlead completion triggered when scheduling a taster from a hotlead.

### Dashboard Redesign (Mar 26)

- Dashboard fully redesigned with new CSS grid layout.
- Confirm Taster flow added (separate confirmation step before saving).
- My Notes page added (personal per-user notes).
- Dashboard notes and todos made personal (per-user, not shared).

### Willow's Corner (Apr 3)

- Insurance notice feature added for Willow's Corner role.
- Notice message shown via dedicated section in Willow's Corner view.

### Postgres & Startup Hardening (Mar 21)

- Database init deferred until first request (avoids startup stall on Render cold boot).
- 503 response added while DB is unavailable.
- Session and Postgres startup handling hardened.
- Startup stall time during DB outages reduced.

### Email Infrastructure Migration (Apr 4)

- Cloudflare email worker removed entirely.
- Direct Purelymail SMTP backend added (`noreply@tasterist.com` as sending account).
- Render cron job added for weekly report delivery (replaces Cloudflare cron trigger).
- Weekly email widened to 700px; By Programme collapsed to one compact line.
- Email routing aligned: Render default configured.

### Public Marketing Landing Page (Apr 4)

- Full marketing landing page added (`templates/landing.html`).
- SEO metadata, indexing signals, and social links polished.
- Stats strip updated (15+ features and workflows messaging).
- About copy refined (trampoline, dashboard mention).
- Nav links for scale, trusted-by, about sections added.
- GitHub link added to footer.

### Dashboard + Hotleads UX Polish (Apr 5)

- Dashboard layout, spacing, and card proportions significantly refined.
- Admin Tasks redesigned: card layout, staff assignment cards, hotlead stat integration.
- Hotleads page redesigned with richer pipeline UI.
- Find Taster (`all_tasters.html`) layout and filter polish.
- Day view row layout and edit interactions refined.
- Edit Taster quick class suggestions refined.
- Sidebar and base template streamlined.

### Tablet Tools Suite (Apr 5)

- Four new tablet-optimised tools added:
  - **Whiteboard** — account-backed freehand drawing canvas (data stored in `tablet_whiteboard_data` column on `users`).
  - **Stopwatch / Timer** — dual-mode countdown and stopwatch for class use.
  - **Deck of Cards** — shuffled card draw tool for games/warm-ups.
  - **True / False** — binary response display tool.
- **Resources** page added (`tablet_resources.html`) with content driven by `tablet_resources_content.py`.
- All tablet tools accessible from the tablet nav bar.
- Willow's Corner layout significantly expanded and refined.

### Support Reply Workflow (Apr 9)

- Full reply-to-suggestion workflow implemented (described in detail in `DEVELOPMENT_DOCUMENTATION.md`).
- `support_reply.html` screen: recipient prefilled, original report shown inline, attachments supported.
- `emails/support_reply.html` + `.txt` email templates.
- `emails/support_report.html` + `.txt` updated with Reply action link.
- `support_report_replies` table added.
- My Notes page refactored and linked from sidebar.
- Tablet Today view significantly expanded.
- Test suite added: `tests/test_support_reply_flow.py`.

### Admin Console + Class Uploads Polish (Apr 9)

- Admin Console refined: Recent Suggestions inbox, Cloud Preflight repositioned, action log cleanup.
- Class upload grid introduced (`admin_class_uploads.html`): programme-based tile grid matching Add Taster visual language.
- Slot status indicators: green (fresh), orange (overdue), neutral (awaiting).
- Day view header and action strip polished.
- Move Requests page layout refined.
- Backup Centre added (`backups.html`) — admin-accessible data backup actions.

### QA + Testing Infrastructure (Apr 4–9)

- `docs/FULL_APP_TESTING_CHECKLIST.md` significantly expanded (covering hotleads, tablet tools, support reply, class uploads).
- `docs/BEAUTIFY_AND_FIX_AGENT_CHECKLIST.md` added.
- `docs/FULL_LAP_QA_2026-04-04.md` added (full QA lap record).
- `docs/PAIR_QA_AGENT_PROMPT.md` added (AI-assisted QA prompt template).
- `tests/test_support_reply_flow.py` added (first automated test file).

### Chasing Payments Workflow (Apr 11)

- New `/admin/payments` workflow added for manual overdue payment follow-ups.
- Access opened to `owner`, `admin`, `manager`, and `staff`; blocked for `tablet` and `willow`.
- Sidebar navigation updated with `Chasing Payments` under Actions.
- Records auto-derive owner labels from existing admin day ownership (`user_admin_days`).
- Contact history added via `Add Contact` modal with `Call` and `Voicemail` logging.
- Active/completed/completed-plan states added with reactivation support.
- Search integration added with `Payment Chase` record type and deep-link `Open` action.
- Admin Tasks integration added for active payment-chase rows.
- New persistence tables:
  - `chasing_payments`
  - `chasing_payment_contacts`
- Automated coverage added in `tests/test_chasing_payments.py`.

## Launch Day Update (2026-02-27)

This is the major pre-launch stabilization + polish release wave. It bundles UI redesign work, data integrity hardening, workflow simplification, and performance fixes across the full app.

### What Shipped (Feature + UX)

- Dashboard cards rebalanced to fit screen height better, with improved programme/admin split sections.
- Dashboard programme graphs aligned with taster/leaver narrative text and clearer metric ordering.
- Dashboard month and mini-calendar interactions improved (large clickable area plus per-day clickability).
- Today/day view redesigned rows with stronger hierarchy and larger click targets.
- Today/day names made directly clickable into Find Taster with search prefilled.
- Day row layout cleaned up to remove duplicate inner outlines and improve hit area.
- Day snapshot/session progress presentation reworked for clearer completion states.
- Month view restored Sunday handling where needed and non-operating cells visually distinguished.
- Month view day-number readability increased and cell sizing/fit improved.
- Month view initials now color-coded for past sessions:
  - Missed (amber),
  - Attended (green),
  - Fully complete (super green when attended + club fees + BG + badge).
- Add Taster and Record Leaver week grids standardized to uniform day/tile widths.
- Add/Leaver pages now rely on per-day/per-column internal scrolling instead of whole-page overflow.
- Add/Leaver “Today” badge moved to top-right without pushing content down.
- Add Taster class tile copy simplified (removed redundant “Pennine Gymnastics” prefixes and duplicated time text).
- Add/Leaver class tiles simplified to fixed compact formats (removed low-value upcoming bubbles).
- Add Taster date selection upgraded to weekly date dropdown behavior for class cadence.
- Reschedule panel expanded and better integrated with add flow and programme switcher.
- Admin Tasks action bar spacing/grouping refined with clearer visual dividers.
- Admin Tasks now supports cleaner action-group sizing for large/small buttons.
- Admin Tasks filter/summary clutter reduced (removed redundant active-filters container).
- Admin Tasks now supports expanded to-action handling for missing-data leavers.
- Sidebar active-state style updated from dot indicator to subtle lime border treatment.
- Sign-out switched to anchored account popover confirmation instead of full-screen modal.
- Light/Dark theme support expanded with settings-driven theme selection.
- Settings email schedule card layout refined (save control placement, conditional schedule controls).
- Support/help modal enhanced and attachment support expanded with safe file checks.
- Added richer “added by” visibility in records and row metadata.
- Added archive workflows in admin follow-up handling.
- Notes workflows expanded across day/admin interactions with view/edit modal patterns.

### Data Integrity + Safety Fixes

- Mirrored AM/PM duplicate taster handling hardened (import-time and runtime protection).
- Early-morning false duplicates corrected with safer PM normalization logic.
- Non-duplicate PM records preserved while only invalid mirrored cases are merged/fixed.
- Duplicate leaver insertion hardened with safer upsert/fallback behavior.
- Reschedule workflows hardened to avoid accidental duplicate row explosions.
- Same-day reschedule now correctly excludes the source row from duplicate checks.
- Same-day +30 minute moves now process cleanly in reschedule path.
- No-op reschedule submissions now return a clear warning instead of creating churn.
- Unknown/alien class/day diagnostics improved in Find Taster and admin workflows.
- Import guardrails refined to avoid destructive clears and preserve valid data paths.

### Performance + Scaling Fixes

- Find Taster query/render path optimized for large datasets with capped fast-load behavior.
- Find Taster sorting/filtering optimized on the client (cached filter groups and sort keys).
- Find Taster table now uses stable fixed-layout columns to survive different browser zoom levels.
- Find Taster names are one-line/ellipsis to prevent row-height blowouts from long text.
- Long notes moved behind `View Note` modal to stabilize row height and improve scan speed.
- Right-side Find Taster action buttons forced to single-line always-visible layout.
- Residual outer-page scroll removed on add/leaver/admin layouts where internal scroll is intended.
- Cross-device desktop scaling tuned for better behavior on high-resolution monitors.

### Bug Fixes (High-Impact)

- Fixed stats page chart growth/regeneration issue on repeated page opens.
- Fixed month view clipping where bottom date row could be obscured by jump-to panel.
- Fixed add-page residual scroll after day-column scroll behavior changes.
- Fixed admin tasks residual shell scrolling regressions.
- Fixed mismatched action button sizing (attendance/contacted and related admin controls).
- Fixed reschedule location-switch behavior not honoring programme changes.
- Fixed inability to clear Find Taster quick search in linked workflows (clear affordance restored).
- Fixed class/day tile alignment regressions on Saturdays and sparse-day layouts.
- Fixed sidebar active marker inconsistency (dot removed, border-only active state).

### Operator-Facing Outcome

- Faster day-to-day workflow through clearer UI structure and larger click targets.
- Safer taster/leaver data handling under real import noise and duplicate edge cases.
- Better reliability for launch demos and live operations across desktop and mobile form factors.

## Release Discipline

1. Run `./scripts/update_release_history.sh`.
2. Update `VERSION` if releasing.
3. Commit both `docs/RELEASE_HISTORY.md` and `VERSION`.
4. Push to GitHub so the repository history docs stay current.

## Full Commit Ledger

| Commit | Date | Author | Message |
|---|---|---|---|
| `d2535e6` | 2026-02-17 | James Gardner | Initial app baseline with UI/admin improvements |
| `379901f` | 2026-02-17 | James Gardner | Fix Excel 4:45 slot matching and harden login import |
| `1cfc4f3` | 2026-02-17 | James Gardner | Fix leaver slot sync by day/time and add leaver checklist fields |
| `ac0aac1` | 2026-02-17 | James Gardner | Unify admin followups, add club fees flow, and cloud preflight |
| `65dd5bf` | 2026-02-17 | James Gardner | Redesign dashboard full-width with programme today lists and add Render runbook |
| `46c2c61` | 2026-02-17 | James Gardner | Refine dashboard clock/list sizing and add cloud backup action |
| `a52c8e3` | 2026-02-17 | James Gardner | Add PWA install support and run/domain hosting guides |
| `242f030` | 2026-02-17 | James Gardner | Add tasterist.com canonical host support and GoDaddy deploy steps |
| `5e9bc7b` | 2026-02-17 | James Gardner | Harden auth: owner-only account creation, CSRF, rate limiting, and secure defaults |
| `adbe1ce` | 2026-02-17 | James Gardner | Fix startup crash by running init_db after helper definitions |
| `fc3f07b` | 2026-02-17 | James Gardner | Reduce Render gunicorn worker count to prevent boot restart loops |
| `0a726c2` | 2026-02-17 | James Gardner | Fix Render sqlite lock boot loop with single worker and DB init retries |
| `5f66d3c` | 2026-02-17 | James Gardner | Add break-glass owner password reset via env var |
| `6418786` | 2026-02-17 | James Gardner | Relax password policy to 7 chars with upper/lowercase only |
| `fddd72c` | 2026-02-17 | James Gardner | Adjust password policy to uppercase + number + min 7 |
| `bf7e75e` | 2026-02-17 | James Gardner | Switch cloud to manual imports and allow Excel sync in configured sheets folder |
| `24d5cdf` | 2026-02-17 | James Gardner | Make cloud import path safer and prevent table clear on missing sheets |
| `902dc84` | 2026-02-18 | James Gardner | Add upload-based manual import workflow and storage status visibility |
| `c862e86` | 2026-02-18 | James Gardner | Fix importer to use app DB path and bootstrap missing tables |
| `ae61f0b` | 2026-02-18 | James Gardner | Handle GET on import endpoints with friendly redirects |
| `9db9b56` | 2026-02-18 | James Gardner | Polish admin console, import flow, and name normalization |
| `8ad68bc` | 2026-02-18 | James Gardner | Make imports safe against empty/unreadable workbook runs |
| `600e93f` | 2026-02-18 | James Gardner | Pin 2025 imports to local archive and improve import guidance |
| `5fbac3b` | 2026-02-18 | James Gardner | Restore class grid fallback and relax 2025 local import strictness |
| `326d40a` | 2026-02-18 | James Gardner | Improve class scheduling UX and make imports merge-safe by default |
| `9fc9971` | 2026-02-18 | James Gardner | Add SQLite-to-Postgres migration tooling and runbook |
| `ecb64e8` | 2026-02-18 | James Gardner | Deploy latest app fixes (security, email, UI, admin) |
| `b69e197` | 2026-02-18 | James Gardner | Add unknown-class filter + diagnostics and PM time fixer |
| `0364f2e` | 2026-02-19 | James Gardner | Switch runtime DB to Postgres primary and harden startup |
| `4965f3e` | 2026-02-19 | James Gardner | Split manual add into dedicated screens and remove OneDrive sync toggle |
| `090fe54` | 2026-02-19 | James Gardner | Apply manual class timetable and tighten weekly schedule fallback |
| `ec06b3a` | 2026-02-19 | James Gardner | Fix admin password loop and disable forced strong-password flow |
| `1ae633e` | 2026-02-19 | James Gardner | Fix Postgres admin-day upserts and sidebar flash placement |
| `53d3562` | 2026-02-19 | James Gardner | Dock today summary cards and switch UI dates to UK format |
| `cec95a2` | 2026-02-19 | James Gardner | Trim Pennine Gymnastics prefix in admin to-action class labels |
| `0f65b0b` | 2026-02-19 | James Gardner | Simplify admin tasks subtitle to past 3 months |
| `3cee970` | 2026-02-19 | James Gardner | Skip orphan preschool Saturday rows during workbook import |
| `999de34` | 2026-02-19 | James Gardner | Harden taster date guardrails and polish button motion |
| `e4d6bab` | 2026-02-19 | James Gardner | Refine dashboard navigation, settings links, and app-taster export |
| `9628434` | 2026-02-19 | James Gardner | Add Cloudflare email worker config and leaver form/back-button fixes |
| `46ece77` | 2026-02-19 | James Gardner | Fix Cloudflare email MIME headers (Date/Message-ID) |
| `15bd3a2` | 2026-02-22 | James Gardner | Add taster delete + changelog, login version label, and email worker header fix |
| `a889468` | 2026-02-22 | James Gardner | Add retry + better error formatting for email send |
| `b72f3fa` | 2026-02-22 | James Gardner | Fix retry bug by rebuilding EmailMessage per attempt |
| `c1f7468` | 2026-02-22 | James Gardner | Add worker send_email diagnostic log |
| `3e47f7b` | 2026-02-22 | James Gardner | Add worker binding diagnostic flags on GET |
| `373b1c2` | 2026-02-22 | James Gardner | Use destination-address binding recipient resolution for send_email |
| `e212318` | 2026-02-22 | James Gardner | Stabilize Cloudflare email sender config and worker naming |
| `02a6806` | 2026-02-22 | James Gardner | Use native Cloudflare send_email API and tighten binding |
| `6980e21` | 2026-02-23 | James Gardner | Support Render Secret Files for auth and critical config |
| `8f7cce4` | 2026-02-23 | James Gardner | Support BOOSTRAP typo key and apply owner bootstrap once for recovery |
| `657b499` | 2026-02-23 | James Gardner | Remove Excel sync warnings and add minimal bottom-left settings cog |
| `d270136` | 2026-02-23 | James Gardner | Add compact mobile/tablet dashboard mode |
| `aed66ab` | 2026-02-23 | James Gardner | Allow admins to access admin menu and show compact nav link |
| `3867cbf` | 2026-02-23 | James Gardner | Hide unknown rows from admin totals while keeping To Action |
| `66e0ca0` | 2026-02-23 | James Gardner | Filter stats to current reporting window |
| `ade5d03` | 2026-02-23 | James Gardner | Replace month jump dropdown with year-month button grid |
| `3f6c237` | 2026-02-23 | James Gardner | Add upcoming hint and viewport-sized scrollable week columns |
| `92f8c2a` | 2026-02-23 | James Gardner | Upgrade find-taster filters and add actioned changelog workflow |
| `c77ac14` | 2026-02-23 | James Gardner | Fit admin To Action panel to viewport with internal scroll |
| `4c1eca3` | 2026-02-23 | James Gardner | Force non-destructive imports and add explicit clear-data action |
| `bf765d3` | 2026-02-23 | James Gardner | Restrict import monitor to admins and neutralize top metric colors |
| `48c3009` | 2026-02-24 | James Gardner | Add owner email diagnostics endpoint |
| `451cb03` | 2026-02-24 | James Gardner | Align email worker owner/allowlist to james@penninegymnastics.com |
| `721bdc5` | 2026-02-24 | James Gardner | Add templated weekly email + Resend backend support in worker |
| `b87d1d4` | 2026-02-24 | James Gardner | Checkpoint: UI updates, layout fixes, and workflow improvements |
| `63941ba` | 2026-02-24 | James Gardner | Weekly email: add New This Week tasters table; refine Today header layout |
| `2a51035` | 2026-02-24 | James Gardner | Weekly email: add header logo and high-visibility quick links |
| `9d78d39` | 2026-02-24 | James Gardner | Fix weekly email scope in owner-only mode |
| `4e4d960` | 2026-02-24 | James Gardner | Email updates: admin-day scope visibility and account signup emails |
| `2cacde6` | 2026-02-24 | James Gardner | Email scheduling: per-user send day and admin test-send recipient picker |
| `ad47041` | 2026-02-24 | James Gardner | Admin email test: default recipient checkboxes to off |
| `4485ba0` | 2026-02-24 | James Gardner | Email routing toggle and support report popup |
| `74dfabf` | 2026-02-24 | James Gardner | Improve support report email readability and templating |
| `c224314` | 2026-02-24 | James Gardner | Release 1.0.2 and refresh release history |
| `f47e047` | 2026-02-24 | James Gardner | Center dashboard live datetime with long UK-style format |
| `93bc5dd` | 2026-02-24 | James Gardner | Move day location selector to bottom-right above snapshot |
| `dfe51f0` | 2026-02-24 | James Gardner | Add login contact line for james@tasterist.com |
| `73a773e` | 2026-02-24 | James Gardner | Refine login hero layout and contact link styling |
| `d6c2c2f` | 2026-02-24 | James Gardner | Enable Postgres upload-based imports and restore import monitor metrics |
| `1cdeace` | 2026-02-24 | James Gardner | Push pending UI updates for settings, add/leaver, find, day and admin tasks |
| `74b9dfd` | 2026-02-24 | James Gardner | Fix white-screen fallback and improve Find Taster table editing/layout |
| `df43354` | 2026-02-24 | James Gardner | Fix psycopg boot crash on literal percent in no-param SQL |
| `7ac08dc` | 2026-02-24 | James Gardner | Harden postgres SQL translation for literal percent signs |
| `0c85677` | 2026-02-24 | James Gardner | Fix import staging schema: create class_sessions table |
| `3579227` | 2026-02-24 | James Gardner | Add staging schema guard for Postgres import temp DB |
| `a8824a2` | 2026-02-24 | James Gardner | Fix Find Taster min range handle layering |
| `dd53a25` | 2026-02-24 | James Gardner | Add Reschedule Taster shortcut to sidebar and mobile menu |
| `289e26f` | 2026-02-24 | James Gardner | Move Cloud Preflight checks into Admin Console |
| `3459051` | 2026-02-24 | James Gardner | Grey out non-operating month days by programme |
| `02c0ee0` | 2026-02-24 | James Gardner | Place Cloud Preflight above Action Logs in Admin Console |
| `f06bf0f` | 2026-02-24 | James Gardner | Reorder sidebar actions: place Reschedule between Add and Leaver |
| `c5f984e` | 2026-02-24 | James Gardner | Split reschedule flow into summary step and change-class screen |
| `2b986ff` | 2026-02-24 | James Gardner | Remove sidebar reschedule shortcut and highlight Find Taster reschedule button |
| `9ca5a37` | 2026-02-24 | James Gardner | Narrow Club Fees/BG/Badge columns in Admin Tasks actions |
| `867649e` | 2026-02-24 | James Gardner | Add admin taster edit and duplicate merge backend |
| `8effcff` | 2026-02-24 | James Gardner | Wire admin taster edit UI, dedupe action, and compact admin-task check buttons |
| `a2edde7` | 2026-02-24 | James Gardner | Fix Find Taster timeframe slider handle layering |
| `a5765d8` | 2026-02-24 | James Gardner | Refine Admin Tasks mini check buttons sizing and spacing |
| `7218624` | 2026-02-24 | James Gardner | Add explicit spacing between Cloud Preflight and Action Logs cards |
| `f71fc6c` | 2026-02-24 | James Gardner | Speed up Find Taster render and remove empty duplicate indicator |
| `74198f3` | 2026-02-24 | James Gardner | Always show class calendar during reschedule and polish reschedule header card |
| `f4bdbf4` | 2026-02-24 | James Gardner | Default Find Taster unknown/alien filter to off |
| `c810dff` | 2026-02-24 | James Gardner | Always show day bottom summary cards and move location switch to top-right |
| `d3400a6` | 2026-02-24 | James Gardner | Rework sidebar account actions and move help into Actions section |
| `cc606bd` | 2026-02-24 | James Gardner | Dock day summary cards to bottom when no tasters |
| `58a8c38` | 2026-02-24 | James Gardner | Use day-style location selector in month view header |
| `852d19d` | 2026-02-24 | James Gardner | Dock month jump panel and add dashboard mini calendar layout |
| `312c4b2` | 2026-02-24 | James Gardner | Reflow dashboard cards and dock month jump menu with richer programme chart styling |
| `a08f00a` | 2026-02-24 | James Gardner | Show leavers as orange segment in dashboard programme graph |
| `5e75235` | 2026-02-24 | James Gardner | Refine dashboard fill and expand stats summary panels |
| `8ca5014` | 2026-02-24 | James Gardner | Fix stats admin-day queries to use weekday from taster_date |
| `919488f` | 2026-02-24 | James Gardner | Tighten month-view scaling and page-fit responsiveness |
| `abc07b6` | 2026-02-24 | James Gardner | Fix dashboard/month layout and stabilize stats chart rendering |
| `e01dfd6` | 2026-02-24 | James Gardner | Tighten dashboard today section and remove programme totals footer |
| `ccb426c` | 2026-02-24 | James Gardner | Make add/leaver class tiles stretch to fill calendar column height |
| `7dd1912` | 2026-02-24 | James Gardner | Fix add/leaver tile sizing and refine admin task action bar spacing |
| `78c36b2` | 2026-02-24 | James Gardner | Fix add page height fill and switch sidebar signout to anchored popover |
| `2b94c08` | 2026-02-24 | James Gardner | Remove add-page shell scroll and add active sidebar indicators |
| `7935b59` | 2026-02-24 | James Gardner | Adjust admin task divider to separate large and small action groups |
| `7217c03` | 2026-02-24 | James Gardner | Refine sidebar active dot, admin action dividers, and add find-taster sorting |
| `1fd1dd1` | 2026-02-24 | James Gardner | Hard-lock add/leaver pages to remove residual shell scrolling |
| `f62b7dc` | 2026-02-25 | James Gardner | Standardize today taster row height and eliminate admin tasks shell scroll |
| `4f68a4d` | 2026-02-25 | James Gardner | Fix stats programme bars and prevent mirrored duplicate tasters |
| `4e0ec56` | 2026-02-25 | James Gardner | Refine Add Taster class tiles and upcoming badge visibility |
| `c415415` | 2026-02-25 | James Gardner | Refine email preferences layout and schedule field visibility |
| `7e3a77a` | 2026-02-25 | James Gardner | Pin add/leaver Today badge to header top-right |
| `479f7c3` | 2026-02-25 | James Gardner | Remove upcoming counters and tighten add/leaver class tiles |
| `d0b421d` | 2026-02-25 | James Gardner | Enforce uniform day and tile widths in add/leaver grids |
| `aae1fd3` | 2026-02-25 | James Gardner | Tune Add Taster tile typography and compact spacing |
| `1840847` | 2026-02-25 | James Gardner | Shorten class display names by removing Pennine Gymnastics prefix |
| `6594dcb` | 2026-02-25 | James Gardner | Remove Sunday columns from calendar and scheduling UIs |
| `3bc35c4` | 2026-02-25 | James Gardner | Restyle add selector chips and remove redundant back buttons |
| `25944b9` | 2026-02-25 | James Gardner | Restore Sunday in month view and focus Add Taster on form panel |
| `4cc9f44` | 2026-02-25 | James Gardner | Restore Sunday column in dashboard mini month view |
| `6eb0532` | 2026-02-25 | James Gardner | Remove back-to-month button from day view header |
| `d41cd0b` | 2026-02-25 | James Gardner | Remove Active Filters card from admin tasks layout |
| `814f893` | 2026-02-25 | James Gardner | Make day-view names clickable and prefill Find Taster search |
| `e1eb1ce` | 2026-02-25 | James Gardner | Refine day-view name links with larger click targets and spacing |
| `a369348` | 2026-02-25 | James Gardner | Auto-clean mirrored duplicate tasters during import and dedupe |
| `658df8d` | 2026-02-25 | James Gardner | Make day-view name lane a clean full-width clickable area |
| `279b6ac` | 2026-02-25 | James Gardner | Harden mirrored duplicate cleanup to ignore class label drift |
| `e4bc1e5` | 2026-02-25 | James Gardner | Auto-correct non-duplicate early Lockwood/Honley sessions to PM |
| `044ecf1` | 2026-02-25 | James Gardner | Add week-class dropdown for taster date selection |
| `d019d21` | 2026-02-25 | James Gardner | Allow reschedule location switch to change programme |
| `eb09e45` | 2026-02-25 | James Gardner | Make add-taster date dropdown weekly for selected class |
| `11585e9` | 2026-02-25 | James Gardner | Make reschedule taster card span full header width |
| `ef66a25` | 2026-02-25 | James Gardner | Replace leaver email field with reason and notes |
| `02cf14f` | 2026-02-25 | James Gardner | Improve cross-device desktop scaling and fit |
| `7ba5faa` | 2026-02-25 | James Gardner | Relax desktop scaling for true 100% zoom |
| `aa71d0d` | 2026-02-25 | James Gardner | Default desktop UI scale to 90 percent |
| `5e29d86` | 2026-02-25 | James Gardner | Include incomplete leavers in admin to-action and fix overflow scaling |
| `0566e63` | 2026-02-25 | James Gardner | Upsert duplicate leavers safely and expand stats monthly columns |
| `bb69a94` | 2026-02-25 | James Gardner | Speed up Find Taster and lock day-row height with notes |
| `71a2ecc` | 2026-02-25 | James Gardner | Adjust dashboard/stats metrics and fix month view clipping |
