# Tasterist Development Documentation

## Purpose

This document captures how Tasterist has evolved, how the codebase is structured, and how to maintain release-quality changes safely.

It is intended to be GitHub-facing project documentation and should be updated alongside major product changes.

## Product Scope

Tasterist is an operations-first web app for class/taster administration:

- Dashboard, Today, Month, and Stats operational views.
- Add Taster and Record Leaver flows.
- Find Taster search/edit/reschedule workflow.
- Admin Tasks workflow for attendance/admin checklist completion.
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
- `users`: Auth accounts with roles and password-change enforcement flags.
- `user_admin_days`: User ownership mapping for admin day/programme cells.
- `class_sessions`: Timetable/session templates.
- `audit_logs`: App-level operational and security actions.
- `app_settings`: Feature switches and environment-backed behavior.

## Security and Access Controls

- CSRF enforced on write operations.
- Login rate limiting and lockout windows.
- Owner/admin role checks for privileged operations.
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
