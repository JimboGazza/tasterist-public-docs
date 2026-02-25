# Tasterist Release History

Auto-generated from git history.

## Snapshot

- Current app version file: `1.0.2`
- Current HEAD: `71a2ecc`
- Latest commit date: 2026-02-25 14:21 UTC
- Total commits: 155

## Release Discipline

1. Run `./scripts/update_release_history.sh`.
2. Update `VERSION` if releasing.
3. Commit both `docs/RELEASE_HISTORY.md` and `VERSION`.
4. Push to GitHub so the repository history docs stay current.

## Full Commit Ledger

| Commit | Date (UTC) | Author | Message |
|---|---|---|---|
| `d2535e6` | 2026-02-17 15:24 UTC | James Gardner | Initial app baseline with UI/admin improvements |
| `379901f` | 2026-02-17 15:44 UTC | James Gardner | Fix Excel 4:45 slot matching and harden login import |
| `1cfc4f3` | 2026-02-17 15:58 UTC | James Gardner | Fix leaver slot sync by day/time and add leaver checklist fields |
| `ac0aac1` | 2026-02-17 16:09 UTC | James Gardner | Unify admin followups, add club fees flow, and cloud preflight |
| `65dd5bf` | 2026-02-17 16:18 UTC | James Gardner | Redesign dashboard full-width with programme today lists and add Render runbook |
| `46c2c61` | 2026-02-17 16:27 UTC | James Gardner | Refine dashboard clock/list sizing and add cloud backup action |
| `a52c8e3` | 2026-02-17 16:30 UTC | James Gardner | Add PWA install support and run/domain hosting guides |
| `242f030` | 2026-02-17 16:41 UTC | James Gardner | Add tasterist.com canonical host support and GoDaddy deploy steps |
| `5e9bc7b` | 2026-02-17 16:59 UTC | James Gardner | Harden auth: owner-only account creation, CSRF, rate limiting, and secure defaults |
| `adbe1ce` | 2026-02-17 17:23 UTC | James Gardner | Fix startup crash by running init_db after helper definitions |
| `fc3f07b` | 2026-02-17 17:37 UTC | James Gardner | Reduce Render gunicorn worker count to prevent boot restart loops |
| `0a726c2` | 2026-02-17 17:40 UTC | James Gardner | Fix Render sqlite lock boot loop with single worker and DB init retries |
| `5f66d3c` | 2026-02-17 17:51 UTC | James Gardner | Add break-glass owner password reset via env var |
| `6418786` | 2026-02-17 18:20 UTC | James Gardner | Relax password policy to 7 chars with upper/lowercase only |
| `fddd72c` | 2026-02-17 18:24 UTC | James Gardner | Adjust password policy to uppercase + number + min 7 |
| `bf7e75e` | 2026-02-17 18:32 UTC | James Gardner | Switch cloud to manual imports and allow Excel sync in configured sheets folder |
| `24d5cdf` | 2026-02-17 18:44 UTC | James Gardner | Make cloud import path safer and prevent table clear on missing sheets |
| `902dc84` | 2026-02-18 13:44 UTC | James Gardner | Add upload-based manual import workflow and storage status visibility |
| `c862e86` | 2026-02-18 13:58 UTC | James Gardner | Fix importer to use app DB path and bootstrap missing tables |
| `ae61f0b` | 2026-02-18 14:05 UTC | James Gardner | Handle GET on import endpoints with friendly redirects |
| `9db9b56` | 2026-02-18 14:27 UTC | James Gardner | Polish admin console, import flow, and name normalization |
| `8ad68bc` | 2026-02-18 14:54 UTC | James Gardner | Make imports safe against empty/unreadable workbook runs |
| `600e93f` | 2026-02-18 14:59 UTC | James Gardner | Pin 2025 imports to local archive and improve import guidance |
| `5fbac3b` | 2026-02-18 15:08 UTC | James Gardner | Restore class grid fallback and relax 2025 local import strictness |
| `326d40a` | 2026-02-18 15:28 UTC | James Gardner | Improve class scheduling UX and make imports merge-safe by default |
| `9fc9971` | 2026-02-18 16:23 UTC | James Gardner | Add SQLite-to-Postgres migration tooling and runbook |
| `ecb64e8` | 2026-02-18 23:39 UTC | James Gardner | Deploy latest app fixes (security, email, UI, admin) |
| `b69e197` | 2026-02-18 23:56 UTC | James Gardner | Add unknown-class filter + diagnostics and PM time fixer |
| `0364f2e` | 2026-02-19 00:25 UTC | James Gardner | Switch runtime DB to Postgres primary and harden startup |
| `4965f3e` | 2026-02-19 00:30 UTC | James Gardner | Split manual add into dedicated screens and remove OneDrive sync toggle |
| `090fe54` | 2026-02-19 00:48 UTC | James Gardner | Apply manual class timetable and tighten weekly schedule fallback |
| `ec06b3a` | 2026-02-19 01:05 UTC | James Gardner | Fix admin password loop and disable forced strong-password flow |
| `1ae633e` | 2026-02-19 01:18 UTC | James Gardner | Fix Postgres admin-day upserts and sidebar flash placement |
| `53d3562` | 2026-02-19 01:29 UTC | James Gardner | Dock today summary cards and switch UI dates to UK format |
| `cec95a2` | 2026-02-19 01:30 UTC | James Gardner | Trim Pennine Gymnastics prefix in admin to-action class labels |
| `0f65b0b` | 2026-02-19 01:31 UTC | James Gardner | Simplify admin tasks subtitle to past 3 months |
| `3cee970` | 2026-02-19 01:35 UTC | James Gardner | Skip orphan preschool Saturday rows during workbook import |
| `999de34` | 2026-02-19 01:40 UTC | James Gardner | Harden taster date guardrails and polish button motion |
| `e4d6bab` | 2026-02-19 11:52 UTC | James Gardner | Refine dashboard navigation, settings links, and app-taster export |
| `9628434` | 2026-02-19 16:35 UTC | James Gardner | Add Cloudflare email worker config and leaver form/back-button fixes |
| `46ece77` | 2026-02-19 18:20 UTC | James Gardner | Fix Cloudflare email MIME headers (Date/Message-ID) |
| `15bd3a2` | 2026-02-22 10:46 UTC | James Gardner | Add taster delete + changelog, login version label, and email worker header fix |
| `a889468` | 2026-02-22 14:32 UTC | James Gardner | Add retry + better error formatting for email send |
| `b72f3fa` | 2026-02-22 14:35 UTC | James Gardner | Fix retry bug by rebuilding EmailMessage per attempt |
| `c1f7468` | 2026-02-22 14:39 UTC | James Gardner | Add worker send_email diagnostic log |
| `3e47f7b` | 2026-02-22 15:40 UTC | James Gardner | Add worker binding diagnostic flags on GET |
| `373b1c2` | 2026-02-22 15:51 UTC | James Gardner | Use destination-address binding recipient resolution for send_email |
| `e212318` | 2026-02-22 16:12 UTC | James Gardner | Stabilize Cloudflare email sender config and worker naming |
| `02a6806` | 2026-02-22 17:06 UTC | James Gardner | Use native Cloudflare send_email API and tighten binding |
| `6980e21` | 2026-02-23 17:18 UTC | James Gardner | Support Render Secret Files for auth and critical config |
| `8f7cce4` | 2026-02-23 17:21 UTC | James Gardner | Support BOOSTRAP typo key and apply owner bootstrap once for recovery |
| `657b499` | 2026-02-23 18:19 UTC | James Gardner | Remove Excel sync warnings and add minimal bottom-left settings cog |
| `d270136` | 2026-02-23 19:03 UTC | James Gardner | Add compact mobile/tablet dashboard mode |
| `aed66ab` | 2026-02-23 19:13 UTC | James Gardner | Allow admins to access admin menu and show compact nav link |
| `3867cbf` | 2026-02-23 19:15 UTC | James Gardner | Hide unknown rows from admin totals while keeping To Action |
| `66e0ca0` | 2026-02-23 19:18 UTC | James Gardner | Filter stats to current reporting window |
| `ade5d03` | 2026-02-23 19:20 UTC | James Gardner | Replace month jump dropdown with year-month button grid |
| `3f6c237` | 2026-02-23 19:22 UTC | James Gardner | Add upcoming hint and viewport-sized scrollable week columns |
| `92f8c2a` | 2026-02-23 19:28 UTC | James Gardner | Upgrade find-taster filters and add actioned changelog workflow |
| `c77ac14` | 2026-02-23 19:29 UTC | James Gardner | Fit admin To Action panel to viewport with internal scroll |
| `4c1eca3` | 2026-02-23 19:33 UTC | James Gardner | Force non-destructive imports and add explicit clear-data action |
| `bf765d3` | 2026-02-23 19:34 UTC | James Gardner | Restrict import monitor to admins and neutralize top metric colors |
| `48c3009` | 2026-02-24 13:49 UTC | James Gardner | Add owner email diagnostics endpoint |
| `451cb03` | 2026-02-24 13:58 UTC | James Gardner | Align email worker owner/allowlist to james@penninegymnastics.com |
| `721bdc5` | 2026-02-24 14:23 UTC | James Gardner | Add templated weekly email + Resend backend support in worker |
| `b87d1d4` | 2026-02-24 15:43 UTC | James Gardner | Checkpoint: UI updates, layout fixes, and workflow improvements |
| `63941ba` | 2026-02-24 15:58 UTC | James Gardner | Weekly email: add New This Week tasters table; refine Today header layout |
| `2a51035` | 2026-02-24 16:04 UTC | James Gardner | Weekly email: add header logo and high-visibility quick links |
| `9d78d39` | 2026-02-24 16:16 UTC | James Gardner | Fix weekly email scope in owner-only mode |
| `4e4d960` | 2026-02-24 16:28 UTC | James Gardner | Email updates: admin-day scope visibility and account signup emails |
| `2cacde6` | 2026-02-24 16:36 UTC | James Gardner | Email scheduling: per-user send day and admin test-send recipient picker |
| `ad47041` | 2026-02-24 17:08 UTC | James Gardner | Admin email test: default recipient checkboxes to off |
| `4485ba0` | 2026-02-24 17:19 UTC | James Gardner | Email routing toggle and support report popup |
| `74dfabf` | 2026-02-24 17:33 UTC | James Gardner | Improve support report email readability and templating |
| `c224314` | 2026-02-24 17:46 UTC | James Gardner | Release 1.0.2 and refresh release history |
| `f47e047` | 2026-02-24 17:49 UTC | James Gardner | Center dashboard live datetime with long UK-style format |
| `93bc5dd` | 2026-02-24 17:53 UTC | James Gardner | Move day location selector to bottom-right above snapshot |
| `dfe51f0` | 2026-02-24 17:55 UTC | James Gardner | Add login contact line for james@tasterist.com |
| `73a773e` | 2026-02-24 18:02 UTC | James Gardner | Refine login hero layout and contact link styling |
| `d6c2c2f` | 2026-02-24 19:10 UTC | James Gardner | Enable Postgres upload-based imports and restore import monitor metrics |
| `1cdeace` | 2026-02-24 19:12 UTC | James Gardner | Push pending UI updates for settings, add/leaver, find, day and admin tasks |
| `74b9dfd` | 2026-02-24 19:21 UTC | James Gardner | Fix white-screen fallback and improve Find Taster table editing/layout |
| `df43354` | 2026-02-24 19:50 UTC | James Gardner | Fix psycopg boot crash on literal percent in no-param SQL |
| `7ac08dc` | 2026-02-24 19:58 UTC | James Gardner | Harden postgres SQL translation for literal percent signs |
| `0c85677` | 2026-02-24 20:47 UTC | James Gardner | Fix import staging schema: create class_sessions table |
| `3579227` | 2026-02-24 20:51 UTC | James Gardner | Add staging schema guard for Postgres import temp DB |
| `a8824a2` | 2026-02-24 20:53 UTC | James Gardner | Fix Find Taster min range handle layering |
| `dd53a25` | 2026-02-24 20:58 UTC | James Gardner | Add Reschedule Taster shortcut to sidebar and mobile menu |
| `289e26f` | 2026-02-24 21:05 UTC | James Gardner | Move Cloud Preflight checks into Admin Console |
| `3459051` | 2026-02-24 21:09 UTC | James Gardner | Grey out non-operating month days by programme |
| `02c0ee0` | 2026-02-24 21:16 UTC | James Gardner | Place Cloud Preflight above Action Logs in Admin Console |
| `f06bf0f` | 2026-02-24 21:17 UTC | James Gardner | Reorder sidebar actions: place Reschedule between Add and Leaver |
| `c5f984e` | 2026-02-24 21:22 UTC | James Gardner | Split reschedule flow into summary step and change-class screen |
| `2b986ff` | 2026-02-24 21:23 UTC | James Gardner | Remove sidebar reschedule shortcut and highlight Find Taster reschedule button |
| `9ca5a37` | 2026-02-24 21:26 UTC | James Gardner | Narrow Club Fees/BG/Badge columns in Admin Tasks actions |
| `867649e` | 2026-02-24 21:36 UTC | James Gardner | Add admin taster edit and duplicate merge backend |
| `8effcff` | 2026-02-24 21:41 UTC | James Gardner | Wire admin taster edit UI, dedupe action, and compact admin-task check buttons |
| `a2edde7` | 2026-02-24 21:55 UTC | James Gardner | Fix Find Taster timeframe slider handle layering |
| `a5765d8` | 2026-02-24 21:56 UTC | James Gardner | Refine Admin Tasks mini check buttons sizing and spacing |
| `7218624` | 2026-02-24 21:57 UTC | James Gardner | Add explicit spacing between Cloud Preflight and Action Logs cards |
| `f71fc6c` | 2026-02-24 22:02 UTC | James Gardner | Speed up Find Taster render and remove empty duplicate indicator |
| `74198f3` | 2026-02-24 22:07 UTC | James Gardner | Always show class calendar during reschedule and polish reschedule header card |
| `f4bdbf4` | 2026-02-24 22:07 UTC | James Gardner | Default Find Taster unknown/alien filter to off |
| `c810dff` | 2026-02-24 22:10 UTC | James Gardner | Always show day bottom summary cards and move location switch to top-right |
| `d3400a6` | 2026-02-24 22:12 UTC | James Gardner | Rework sidebar account actions and move help into Actions section |
| `cc606bd` | 2026-02-24 22:17 UTC | James Gardner | Dock day summary cards to bottom when no tasters |
| `58a8c38` | 2026-02-24 22:18 UTC | James Gardner | Use day-style location selector in month view header |
| `852d19d` | 2026-02-24 22:26 UTC | James Gardner | Dock month jump panel and add dashboard mini calendar layout |
| `312c4b2` | 2026-02-24 22:36 UTC | James Gardner | Reflow dashboard cards and dock month jump menu with richer programme chart styling |
| `a08f00a` | 2026-02-24 22:38 UTC | James Gardner | Show leavers as orange segment in dashboard programme graph |
| `5e75235` | 2026-02-24 22:53 UTC | James Gardner | Refine dashboard fill and expand stats summary panels |
| `8ca5014` | 2026-02-24 22:57 UTC | James Gardner | Fix stats admin-day queries to use weekday from taster_date |
| `919488f` | 2026-02-24 23:00 UTC | James Gardner | Tighten month-view scaling and page-fit responsiveness |
| `abc07b6` | 2026-02-24 23:15 UTC | James Gardner | Fix dashboard/month layout and stabilize stats chart rendering |
| `e01dfd6` | 2026-02-24 23:23 UTC | James Gardner | Tighten dashboard today section and remove programme totals footer |
| `ccb426c` | 2026-02-24 23:26 UTC | James Gardner | Make add/leaver class tiles stretch to fill calendar column height |
| `7dd1912` | 2026-02-24 23:33 UTC | James Gardner | Fix add/leaver tile sizing and refine admin task action bar spacing |
| `78c36b2` | 2026-02-24 23:40 UTC | James Gardner | Fix add page height fill and switch sidebar signout to anchored popover |
| `2b94c08` | 2026-02-24 23:47 UTC | James Gardner | Remove add-page shell scroll and add active sidebar indicators |
| `7935b59` | 2026-02-24 23:48 UTC | James Gardner | Adjust admin task divider to separate large and small action groups |
| `7217c03` | 2026-02-24 23:55 UTC | James Gardner | Refine sidebar active dot, admin action dividers, and add find-taster sorting |
| `1fd1dd1` | 2026-02-24 23:58 UTC | James Gardner | Hard-lock add/leaver pages to remove residual shell scrolling |
| `f62b7dc` | 2026-02-25 00:05 UTC | James Gardner | Standardize today taster row height and eliminate admin tasks shell scroll |
| `4f68a4d` | 2026-02-25 00:25 UTC | James Gardner | Fix stats programme bars and prevent mirrored duplicate tasters |
| `4e0ec56` | 2026-02-25 00:34 UTC | James Gardner | Refine Add Taster class tiles and upcoming badge visibility |
| `c415415` | 2026-02-25 00:40 UTC | James Gardner | Refine email preferences layout and schedule field visibility |
| `7e3a77a` | 2026-02-25 00:42 UTC | James Gardner | Pin add/leaver Today badge to header top-right |
| `479f7c3` | 2026-02-25 00:46 UTC | James Gardner | Remove upcoming counters and tighten add/leaver class tiles |
| `d0b421d` | 2026-02-25 00:47 UTC | James Gardner | Enforce uniform day and tile widths in add/leaver grids |
| `aae1fd3` | 2026-02-25 00:50 UTC | James Gardner | Tune Add Taster tile typography and compact spacing |
| `1840847` | 2026-02-25 00:51 UTC | James Gardner | Shorten class display names by removing Pennine Gymnastics prefix |
| `6594dcb` | 2026-02-25 00:58 UTC | James Gardner | Remove Sunday columns from calendar and scheduling UIs |
| `3bc35c4` | 2026-02-25 01:00 UTC | James Gardner | Restyle add selector chips and remove redundant back buttons |
| `25944b9` | 2026-02-25 01:02 UTC | James Gardner | Restore Sunday in month view and focus Add Taster on form panel |
| `4cc9f44` | 2026-02-25 01:04 UTC | James Gardner | Restore Sunday column in dashboard mini month view |
| `6eb0532` | 2026-02-25 01:14 UTC | James Gardner | Remove back-to-month button from day view header |
| `d41cd0b` | 2026-02-25 01:23 UTC | James Gardner | Remove Active Filters card from admin tasks layout |
| `814f893` | 2026-02-25 01:27 UTC | James Gardner | Make day-view names clickable and prefill Find Taster search |
| `e1eb1ce` | 2026-02-25 01:32 UTC | James Gardner | Refine day-view name links with larger click targets and spacing |
| `a369348` | 2026-02-25 01:39 UTC | James Gardner | Auto-clean mirrored duplicate tasters during import and dedupe |
| `658df8d` | 2026-02-25 01:39 UTC | James Gardner | Make day-view name lane a clean full-width clickable area |
| `279b6ac` | 2026-02-25 01:47 UTC | James Gardner | Harden mirrored duplicate cleanup to ignore class label drift |
| `e4bc1e5` | 2026-02-25 01:55 UTC | James Gardner | Auto-correct non-duplicate early Lockwood/Honley sessions to PM |
| `044ecf1` | 2026-02-25 02:01 UTC | James Gardner | Add week-class dropdown for taster date selection |
| `d019d21` | 2026-02-25 02:02 UTC | James Gardner | Allow reschedule location switch to change programme |
| `eb09e45` | 2026-02-25 02:05 UTC | James Gardner | Make add-taster date dropdown weekly for selected class |
| `11585e9` | 2026-02-25 02:07 UTC | James Gardner | Make reschedule taster card span full header width |
| `ef66a25` | 2026-02-25 02:10 UTC | James Gardner | Replace leaver email field with reason and notes |
| `02cf14f` | 2026-02-25 12:41 UTC | James Gardner | Improve cross-device desktop scaling and fit |
| `7ba5faa` | 2026-02-25 12:47 UTC | James Gardner | Relax desktop scaling for true 100% zoom |
| `aa71d0d` | 2026-02-25 12:49 UTC | James Gardner | Default desktop UI scale to 90 percent |
| `5e29d86` | 2026-02-25 12:59 UTC | James Gardner | Include incomplete leavers in admin to-action and fix overflow scaling |
| `0566e63` | 2026-02-25 13:43 UTC | James Gardner | Upsert duplicate leavers safely and expand stats monthly columns |
| `bb69a94` | 2026-02-25 14:15 UTC | James Gardner | Speed up Find Taster and lock day-row height with notes |
| `71a2ecc` | 2026-02-25 14:21 UTC | James Gardner | Adjust dashboard/stats metrics and fix month view clipping |
