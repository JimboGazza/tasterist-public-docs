# Tasterist Development Documentation

## Purpose

This document captures how Tasterist has evolved, how the codebase is structured, and how to maintain release-quality changes safely.

It is intended to be GitHub-facing project documentation and should be updated alongside major product changes.

## Product Scope

Tasterist is an operations-first web app for class/taster administration:

- Dashboard, Today, Month, and Stats operational views.
- Add Taster (with two-step review/confirm flow) and Record Leaver flows.
- Mover workflow for planned class/session changes.
- Find Taster search/edit/reschedule workflow.
- Admin Tasks workflow for attendance/admin checklist completion.
- Hotleads pipeline for waiting-list contacts: claim, action, archive, and delete.
- My Notes page for personal to-do lists and free-text notes.
- Account management with role controls and password security gates.
- Email scheduling/reporting with Cloudflare worker integration.
- Import workflows for external class data.

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
- `movers`: Planned class moves with source/destination session data, attendance, LoveAdmin completion, and notes.
- `users`: Auth accounts with roles and password-change enforcement flags.
- `user_admin_days`: User ownership mapping for admin day/programme cells.
- `class_sessions`: Timetable/session templates.
- `hotleads`: Waiting-list contact pipeline entries. Status flags: `claimed`, `no_space`, `inactive`, `completed`. Access mode is admin-configurable via `app_settings`.
- `audit_logs`: App-level operational and security actions.
- `app_settings`: Feature switches and environment-backed behavior.

## Security and Access Controls

- CSRF enforced on write operations.
- Login rate limiting and lockout windows.
- Owner/admin role checks for privileged operations.
- Mandatory password-change flow for first login / default-password accounts.
- Emergency owner credential path controlled by env/secret values.
- Hotlead hard-delete (permanent removal) gated on admin role (`can_view_all_hotleads`).

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
9. Branding refresh, mover workflow rollout, search redesign, and light-mode polish.
10. Hotleads pipeline, My Notes page, Confirm Taster flow, and dashboard redesign.

## Operator Request Timeline (Recent Sprint — March 2026)

Recent delivery cycles were driven by production-like operator feedback. Main themes:

1. **Confirm Taster flow**: Two-step review/confirm on the Add Taster page so staff can check details before saving. Centered summary card shows taster date, class, session, location, child name, notes, and any hotlead link or reschedule context.
2. **Dashboard redesign**: Removed bigger/smaller density setting and personal To-Do & Notes section. Hotlead rows rendered as a clean borderless list (fixed 34px row height, no card gaps). "This Month By Programme" replaced with visual stat blocks per programme.
3. **My Notes page** (`/my-notes`): Dedicated page for personal to-do list (left) and free-text notes (right), linked from the sidebar OVERVIEW section. Sidebar nav shows open task count.
4. **Hotlead bin/delete actions**: "No Space" button replaced with a bin icon. In Claim Hotleads (unclaimed leads), bin permanently deletes the record (admin only). In My Hotleads (claimed leads), bin archives the lead (marks no_space, keeps record). "Show Completed" renamed to "Show Completed / Archive".
5. **Search hotlead improvements**: Delete button added to hotlead rows in search results (admin only). New filter groups: "Hotlead Status" (active / no space / inactive) and "Hotlead Claimed" (unclaimed / claimed).
6. **Light mode fixes**: Confirm taster card fully themed for light mode.
7. **Database resilience**: Deferred database init until first request, 503 fallback while DB is unavailable, reduced startup stalls.
8. **Hotleads access control**: Mode setting moved into Admin Console (`app_settings`) rather than environment variable.
9. **Search page viewport**: Panels constrained to viewport height, collapsible filter sidebar.
10. **Cloudflare worker workflow**: Automated deploy, local-only worker mode, preserved environment variables on redeploy.

## Operator Request Timeline (Late February to Early March 2026)

1. **Find Taster cleanup**: Button-triggered search, stabilized `View Note` modal wiring, compact source badges, and responsive scaling/reset fixes.
2. **Branding refresh**: Uploaded Tasterist logo and favicon assets replaced older branding across login UI and browser chrome.
3. **Mover workflow rollout**: Add/edit/search/delete flows for movers, source/destination session selection, cross-facility fixes, and Postgres-safe query/insert handling.
4. **Search redesign**: Wider flatter rows, status/icon cleanup, admin-day and class-fees filters, async toggles, known-record default, and faster day/month navigation.
5. **Light theme and settings polish**: Rebuilt light palette, settings card proportions, admin-task contrast fixes, and more stable email preference layouts.
6. **Dashboard and admin refinements**: Personal notes/todos, archived shortcut, action alignment, missed/leaver orange emphasis, and internal scrolling in the dashboard hotlead area.
7. **Hotlead rollout groundwork**: Initial hotlead workflow changes, default enablement, completion/no-space controls, and dashboard list behavior refinements.

## Operator Request Timeline (Previous Sprint — February 2026)

1. Dashboard and stats graph readability and placement of tasters/leavers context text.
2. Month/day/add/admin layout scaling so cards align to viewport without outer-page scroll.
3. Uniform card sizes and per-column/day internal scrolling instead of global page overflow.
4. Sidebar active-page indicator simplification (dot-based state cue).
5. Admin action-bar grouping/dividers and consistent button sizing.
6. Day-view row redesign with larger clickable name lane routing to Find Taster.
7. Duplicate session cleanup (AM/PM mirror issues), with safer dedupe and non-destructive correction rules.
8. Add/Leaver flows simplification (class naming cleanup, removed low-value "upcoming" signals).
9. Reschedule UX upgrades (date options in weekly cadence and cross-location behavior fixes).
10. Email preference/layout refinements in settings.
11. Data safety guards around duplicate leavers and import-side normalization.
12. Ongoing cross-device scaling adjustments for desktop monitor behavior.
13. Find Taster responsiveness improvements and filter model refinements.
14. Current hardening pass for deployment readiness.

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
- Add Taster flow (class selection, review card, confirm, duplicate guardrails) verified.
- Mover add/edit/search/delete flow verified, including source/destination persistence and cross-location handling.
- Confirm Taster summary card renders correctly in both dark and light mode.
- Hotlead bin actions: permanent delete in Claim section (admin), archive in My Hotleads section (all users).
- My Notes page: to-do items and notes save correctly; sidebar count reflects open tasks.
- Search hotlead filters (status, claimed) function correctly and are role-gated where expected.
- Day view checklist actions and Find Taster deep-linking verified.
- Find Taster search/filter interactions performant under representative data volume.
- Admin Tasks and layout/scroll behavior verified at default browser zoom.
- Stats/dashboard metric cards and graph labels verified for correctness.
- No unhandled server errors in add/edit/leaver workflows.

## Maintenance Notes

- `docs/RELEASE_HISTORY.md` is generated from git history.
- `docs/DEVELOPMENT_DOCUMENTATION.md` is intentionally curated and should include rationale, not only commit messages.
- If deployment behavior changes (Render/Cloudflare/env secrets), update corresponding runbooks in `docs/` in the same pull request.
