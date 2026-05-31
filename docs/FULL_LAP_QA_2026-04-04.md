# Tasterist Full-Lap QA Pack

Date: 2026-04-04

This pass combines:

- code-level QA/debug review
- live public endpoint checks against Render
- a manual production walkthrough for the authenticated flows

What was verified live:

- `/` returned `200`
- `/robots.txt` returned `200`
- `/sitemap.xml` returned `200`
- `/health` returned `200`

Important limit:

- Authenticated live-browser flows were not directly driven from here.
- Findings below are based on code inspection, route/access review, and consistency checks across pages.
- Use the checklist in this doc to validate the live app end-to-end and capture anything visual or data-specific that only shows up with real production records.

## Findings

### 1. Blocker: non-admin users can delete tasters and leavers from Search

- Severity: blocker
- Pages: `/tasters`
- Roles affected: any signed-in non-tablet user who can access Search
- Expected: delete should be admin-only if edit is admin-only
- Actual: delete buttons for tasters and leavers are rendered without an admin guard, and the delete routes are not decorated with `@admin_required`

Code references:

- [templates/all_tasters.html:495](/Users/jamesgardner/Documents/Tasterist/templates/all_tasters.html#L495)
- [templates/all_tasters.html:603](/Users/jamesgardner/Documents/Tasterist/templates/all_tasters.html#L603)
- [app.py:10015](/Users/jamesgardner/Documents/Tasterist/app.py#L10015)
- [app.py:13620](/Users/jamesgardner/Documents/Tasterist/app.py#L13620)
- [app.py:13210](/Users/jamesgardner/Documents/Tasterist/app.py#L13210)

Why this matters:

- The UI already treats editing tasters/leavers as admin-only.
- Deleting those same records is currently less protected than editing them.
- That creates a real production data-loss risk.

### 2. Logic: Day view summary marks records “complete” when they are only attended

- Severity: logic
- Pages: `/day/<date>`
- Roles affected: owner, admin, staff
- Expected: “Completed”, “To Action”, and the completion percentage should use the same completion rules as the rest of the app
- Actual: the day summary uses `attended == 1` as the completion test, ignoring fees, BG, badge, and missed-as-complete behavior

Code references:

- [app.py:2445](/Users/jamesgardner/Documents/Tasterist/app.py#L2445)
- [app.py:8670](/Users/jamesgardner/Documents/Tasterist/app.py#L8670)
- [templates/day.html:387](/Users/jamesgardner/Documents/Tasterist/templates/day.html#L387)
- [templates/day.html:404](/Users/jamesgardner/Documents/Tasterist/templates/day.html#L404)

Why this matters:

- A row with attendance only is shown as completed in the Day Snapshot even if fees/BG/badge are still missing.
- A missed taster is treated as complete by `taster_is_complete(...)`, but the Day Snapshot currently counts it as still “To Action”.
- That makes the Day page disagree with Admin Tasks and Search filters.

### 3. Logic: Admin Tasks “Tasters To Members” summary ignores badge requirement and declined-place exclusions

- Severity: logic
- Pages: `/admin/tasks`
- Roles affected: owner, admin, staff
- Expected: “Tasters To Members” should match the same signed-up logic used in Stats and Search
- Actual: the monthly member count only requires attended + fees + BG

Code references:

- [app.py:10188](/Users/jamesgardner/Documents/Tasterist/app.py#L10188)
- [templates/admin_tasks.html:71](/Users/jamesgardner/Documents/Tasterist/templates/admin_tasks.html#L71)

Why this matters:

- Non-preschool tasters can be counted as members without a badge.
- Declined-place rows are not excluded in that summary query.
- The card can therefore overstate conversions relative to Stats/Search.

### 4. Logic: Dashboard admin/member KPIs use a different member definition than Stats

- Severity: logic
- Pages: `/dashboard`
- Roles affected: owner, admin, staff
- Expected: member/conversion counts should use the same rule everywhere
- Actual: the admin-day dashboard buckets count members using attended + fees + BG only in the month and recent sections

Code references:

- [app.py:7480](/Users/jamesgardner/Documents/Tasterist/app.py#L7480)
- [app.py:7504](/Users/jamesgardner/Documents/Tasterist/app.py#L7504)
- [app.py:7563](/Users/jamesgardner/Documents/Tasterist/app.py#L7563)

Why this matters:

- The top-level `converted_month` summary is badge-aware.
- The admin-day dashboard breakdown below it is not.
- That means two dashboard areas can disagree about what “member” means.

### 5. Polish: password-setup screen still shows Pennine branding

- Severity: polish
- Pages: `/set-password`
- Roles affected: any first-login/password-reset user
- Expected: shared Tasterist branding
- Actual: the left-side hero still says `Pennine Gymnastics`

Code reference:

- [templates/set_password.html:18](/Users/jamesgardner/Documents/Tasterist/templates/set_password.html#L18)

### 6. Polish: Admin Tasks still hard-strips “Pennine Gymnastics” from class names

- Severity: polish
- Pages: `/admin/tasks`
- Roles affected: owner, admin, staff
- Expected: class display should come from clean data or a shared formatter
- Actual: the template still manually strips `Pennine Gymnastics` with string replacement in multiple places

Code reference:

- [templates/admin_tasks.html:180](/Users/jamesgardner/Documents/Tasterist/templates/admin_tasks.html#L180)
- [templates/admin_tasks.html:198](/Users/jamesgardner/Documents/Tasterist/templates/admin_tasks.html#L198)

Note:

- This may be harmless in the current deployment, but it is still template-level club residue.

### 7. Decision needed, not automatically a bug: landing page still uses Pennine as the proof-point club

- Severity: low-priority idea
- Pages: `/`
- Expected: depends on product positioning
- Actual: the public “Trusted by” section is explicitly Pennine-branded

Code reference:

- [templates/landing.html:441](/Users/jamesgardner/Documents/Tasterist/templates/landing.html#L441)

Note:

- This is fine if the intention is “real first customer / proof point”.
- It only becomes a problem if you want the shared template to read as club-neutral everywhere.

## Manual Production Checklist

Use this on the live Render deployment. Run once on desktop at 100%, then repeat the main visual pages on a narrower laptop-sized viewport.

### 1. Public and access flow

- Open `/` logged out and check spacing, headings, buttons, footer, socials, and contact form.
- Open `/login` and check spacing, flash messages, and support details.
- Open `/robots.txt`, `/sitemap.xml`, and `/health`.
- Test invalid login.
- Test valid login.
- Test logout.
- Open a protected URL while logged out and confirm redirect to login.

### 2. Password gate and account access

- Sign in with a user that still requires password setup.
- Confirm redirect to `/set-password`.
- Confirm other app routes stay blocked until password is changed.
- Set password and confirm redirect into the app.
- Spot-check one owner/admin account, one staff/manager account, and one tablet account if available.

### 3. Dashboard and navigation

- Open Dashboard and check all cards for overflow/clipping.
- Click through Dashboard cards and confirm destinations are correct.
- Check sidebar active states on every main nav item.
- Confirm dashboard counts feel plausible against visible data.
- Compare Dashboard “Tasters To Members” and “To Action” against Stats/Admin Tasks and note mismatches.

### 4. Today, Month, and Day views

- Open Today and confirm it lands on the correct Day view.
- Open several Day pages: busy day, light day, and empty day.
- On Day pages test attendance, fees, BG, badge, note, and edit flows.
- Compare Day Snapshot “Completed” and “To Action” against the visible row states.
- Open Month, use prev/next, jump-to, and date clickthrough.
- Confirm month cells don’t clip initials or overflow badly.

### 5. Add, edit, reschedule, and leavers

- Add a taster from the weekly picker.
- Add a manual taster.
- Try a duplicate add and confirm the guardrail message.
- Reschedule same class.
- Reschedule to a different class.
- Record a leaver from the guided flow.
- Record a manual leaver.
- Edit an existing taster and leaver.

### 6. Search / record management

- Open `/tasters` with no search and confirm recent results load.
- Test text search, sort, date slider, and clear filters.
- Test filter chips one at a time:
  - type
  - programme
  - attendance
  - signup
  - completion
  - admin-day match
- Open notes/details modals on long rows and confirm the layout still looks tidy.
- If you are signed in as non-admin, confirm whether delete controls are visible. That is a known issue to log immediately.

### 7. Admin Tasks, movers, hotleads, move requests

- Open `/admin/tasks`.
- Check top summary cards.
- Check “To Action”.
- Check archived rows.
- Check the bottom open-admin-tasks area for spacing and single-line stat chips.
- Validate one taster row from Admin Tasks against Search and Day view.
- Open Hotleads and test claim/contact/note/schedule flows.
- Open Move Requests and test create/edit/archive/reactivate.
- Open Movers and test create/edit/delete if used in your workflow.

### 8. Stats and reporting

- Open `/stats`.
- Compare monthly cards against known recent records.
- Open the search links from the summary cards and confirm the filtered results make sense.
- Compare Stats “Tasters To Members” with Dashboard and Admin Tasks.
- Send a weekly email test from Admin Console.
- Open `/admin/email/diagnostics` and confirm it still shows healthy worker/email values.

### 9. Settings, admin console, and imports

- Open personal settings and test:
  - profile
  - password
  - email preferences
  - theme
  - admin days
- Open Admin Console and test:
  - create account
  - save account
  - feature toggles
  - email routing toggle
- Open `/cloud/preflight`.
- Open Import Centre and check the layout and latest-import status.
- Confirm export buttons/downloads still work.

### 10. Tablet and role restrictions

- Sign in as a tablet user and confirm redirect to `/tablet/today`.
- Confirm tablet users cannot access normal owner/admin pages.
- Confirm staff/managers do not see owner-only controls.
- Confirm restricted actions stay blocked by role, especially account/admin areas.

## Bug Capture Format

For each issue you find, log:

- Title: short issue name
- Page: exact route
- Role: owner / admin / staff / tablet
- Steps: 3-6 exact actions
- Expected: one sentence
- Actual: one sentence
- Severity: blocker / logic / polish / low-priority idea
- Attach: screenshot or short screen recording

## Suggested Fix Order

1. Lock down delete permissions for tasters and leavers.
2. Align Day view completion logic with `taster_is_complete(...)`.
3. Align Admin Tasks and Dashboard member counts with the badge-aware signup logic.
4. Clean the remaining Pennine-branded template residue.
