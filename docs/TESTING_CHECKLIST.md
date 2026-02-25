# Tasterist Deployment Testing Checklist

Use this checklist before deployment and after any hotfix. Run at normal browser zoom (Actual Size / 100%) first, then verify one step zoomed-out if needed.

## 1. Auth and Password Gate

- Open `/login` and sign in with a non-owner account that still uses the default password.
- Confirm you are redirected to `/set-password` before app access.
- Confirm app navigation is blocked until password is changed.
- Set a new password and confirm redirect to dashboard.
- Sign out/in again and confirm no forced password screen appears.
- From Admin Console, set a custom password for a user and confirm that user can sign in without forced password reset.
- Confirm owner emergency login still works.

## 2. Add Taster: Date Suggestion Rules

- Open `/add` and pick a class tile for a weekday that has already passed in the current week.
- Confirm **Taster Date defaults to the next upcoming class date** (not past).
- Confirm dropdown still includes a couple of back-dated weekly options.
- Confirm future weekly options appear in 7-day increments.
- Save a taster and verify it appears in `/day/<date>` and `/month` correctly.

## 3. Day View -> Find Taster Flow

- In `/today` or `/day/<date>`, click a taster name lane.
- Confirm it opens Find Taster with search prefilled (`?q=` behavior).
- Confirm the search clear `×` appears on the right of the search box.
- Click `×` and confirm the search empties and all recent records show again.

## 4. Find Taster Filters and Performance

- Open `/tasters` with no query.
- Confirm filter toggles start deselected.
- Confirm with all filters off, records are still shown (recent-first list).
- Toggle one filter at a time and confirm filtering applies correctly.
- Type in search box and confirm UI responds quickly (no long lock-up).
- Verify sort dropdown still works after filtering/searching.

## 5. Regression: Core Workflows

- Add taster.
- Reschedule same class.
- Reschedule to new class.
- Record leaver.
- Open Admin Tasks and complete at least one checklist action.
- Open Stats and Dashboard to ensure no layout/graph regressions.

## 6. Layout/Scaling Checks

- Verify Dashboard, Today, Month, Add Taster, Record Leaver, Find Taster, and Admin Tasks fit viewport without unwanted outer-page scrollbars.
- Verify day cards/tiles remain uniform size where expected.
- Verify internal scroll containers scroll (instead of full page) where designed.

## 7. Data Safety Checks

- Try adding a duplicate taster for same child/programme/day/session and confirm duplicate guard behavior.
- Verify no incorrect AM duplicate is created when PM class exists.
- Confirm existing valid non-duplicate records are retained.

## 8. Smoke Errors and Logs

- Watch server logs while performing steps above.
- Confirm no unhandled exceptions or 500 responses.
- Confirm flash messages are clear and relevant.

## 9. Final Pre-Deploy Gate

- `python3 -m py_compile app.py`
- Confirm `docs/RELEASE_HISTORY.md` is current (`bash scripts/update_release_history.sh`).
- Confirm working tree only contains intended changes.
- Push to GitHub and verify commit appears on `main`.
