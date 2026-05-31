# Tasterist Documentation

This directory is the public documentation set for Tasterist. It mirrors the live app's operator-facing runbooks, QA guides, deployment notes, and release summaries.

## Start Here

- [`DEVELOPMENT_DOCUMENTATION.md`](DEVELOPMENT_DOCUMENTATION.md) - product scope, architecture, data model, roles, and current workflow notes.
- [`RELEASE_HISTORY.md`](RELEASE_HISTORY.md) - chronological release history and current release state.
- [`FULL_APP_TESTING_CHECKLIST.md`](FULL_APP_TESTING_CHECKLIST.md) - full manual QA pass for live/staging releases.
- [`TESTING_CHECKLIST.md`](TESTING_CHECKLIST.md) - shorter operational testing checklist.
- [`RUN_AND_HOSTING.md`](RUN_AND_HOSTING.md) - local run and hosting notes.

## QA And Release Runbooks

- [`BEAUTIFY_AND_FIX_AGENT_CHECKLIST.md`](BEAUTIFY_AND_FIX_AGENT_CHECKLIST.md) - UI polish and fix-pass guidance.
- [`FULL_LAP_QA_2026-04-04.md`](FULL_LAP_QA_2026-04-04.md) - older full-lap QA reference.
- [`PAIR_QA_AGENT_PROMPT.md`](PAIR_QA_AGENT_PROMPT.md) - paired QA agent prompt for structured passes.
- [`PRESENTATION_CHECKLIST.md`](PRESENTATION_CHECKLIST.md) - demo/presentation readiness checklist.

## Hosting, Cloud, And Data

- [`RENDER_PHASE1_DEPLOY.md`](RENDER_PHASE1_DEPLOY.md) - Render phase-one deployment notes.
- [`POSTGRES_MIGRATION.md`](POSTGRES_MIGRATION.md) - SQLite to Postgres migration notes.
- [`CLOUD_ROLLOUT.md`](CLOUD_ROLLOUT.md) - cloud rollout checklist.
- [`CLOUD_NEXT_STEPS.md`](CLOUD_NEXT_STEPS.md) - cloud follow-up work.
- [`CLOUDFLARE_EMAIL_SETUP.md`](CLOUDFLARE_EMAIL_SETUP.md) - historical Cloudflare email setup notes.

## Current Product Areas Covered

- Dashboard, Today, Month, Stats, Search, Add Taster, Record Leaver, Add Mover.
- Admin Tasks, Recently Completed / Archive, Hotleads Due, Payment Chases, and staff-scoped task views.
- Hotleads, Facebook leads, starter-interest callbacks, Request to Move, and Overdue Payments.
- Staff Overview, staff profiles, normal To-Do lists, extended priority boards, and Elle board behavior.
- Tablet resources, public `/resources`, joke of the day, Ring of Fire/deck persistence, Colour Score Game, and Padel tournament scoreboard.
- QA account tooling, celebrity-seeded QA test database mode, role-safe cleanup, and light/dark theme checks.

## Maintenance Rule

When the app behavior changes, update `DEVELOPMENT_DOCUMENTATION.md`, `RELEASE_HISTORY.md`, and whichever QA/deploy runbook owns the affected workflow in the same docs pass.
