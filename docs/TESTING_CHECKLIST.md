# Tasterist Full App Testing Checklist

Use this runbook before Render deploys, major demos, workflow changes, role changes, or any release that touches shared navigation, permissions, admin tasks, hotleads, payments, tablets, or data cleanup.

This checklist is intentionally comprehensive. A pass is not complete until every created test record is cleaned up and a final Search sweep proves no dummy data remains.

## 1. Test Rules

### Environment

- Prefer local or staging for full testing.
- Production testing is allowed only with obvious dummy names and immediate cleanup.
- Do not use real child details, real safeguarding notes, real payment details, or sensitive text in dummy records.
- Keep the browser console open for at least one full pass and note any visible errors.

### Dummy Data Naming

Use one unique prefix for the whole run:

```text
ZZ Test <initials> <date>
```

Examples:

```text
ZZ Test JG 3004 Taster
ZZ Test JG 3004 Hotlead
ZZ Test JG 3004 Payment
ZZ Test JG 3004 Leaver
ZZ Test JG 3004 Move
ZZ Test JG 3004 Insurance
```

### Cleanup Standard

Every created dummy record must finish in one of these states:

- fully deleted
- permanently removed after any soft-delete/archive step
- restored to its original state if testing against a real existing record

Before sign-off:

- Search `/tasters` for the run prefix and confirm zero unwanted dummy records remain.
- Check `/dashboard`, `/today`, `/admin/tasks`, `/hotleads`, `/admin/payments`, `/moves`, and `/insurance` for dummy residue.
- Check personal To-Do boards/lists, Staff Overview-assigned tasks, and Elle To-Do for dummy tasks.
- If a record first moves to `Completed`, `Archive`, `Paid`, `Deleted`, or a hidden bucket, continue cleanup until it is genuinely gone.
- If you cannot remove something, log it as a `logic` bug and do not sign off.

### Issue Capture

For every issue, capture:

- page or route
- account role
- exact steps
- expected result
- actual result
- severity: `blocker`, `logic`, or `polish`
- screenshot or recording

Use this format:

```text
Title:
Page/Route:
Role:
Steps:
1.
2.
3.

Expected:
Actual:
Severity:
Screenshot/Recording:
Cleanup needed:
```

## 2. Roles And Permissions

Test with at least these accounts:

- `admin`
- `manager`
- `staff`
- `willow`
- `elle`
- `tablet`

### Admin

- Can access all normal app pages.
- Can access Account Admin and admin settings.
- Can see all Hotleads.
- Can see all Overdue Payments.
- Can see all Admin Tasks and specific staff queues.
- Can access Staff Overview and staff profile pages.
- `/todos` redirects to Staff Overview.
- Can still access Elle To-Do.
- Can access Insurance Notices.
- Can access tablet routes if needed for support.

### Manager

- Can access normal app pages.
- Cannot access Account Admin/user management.
- Can see all Hotleads.
- Can see all Overdue Payments.
- Can see all Admin Tasks and specific staff queues.
- Can access Staff Overview and staff profile pages.
- `/todos` redirects to Staff Overview.
- Can still access Elle To-Do.
- Can access Insurance Notices.

### Staff

- Can access Dashboard, Today, Month, Stats, Search, Add Taster, Record Leaver, Add Mover, Hotleads, Overdue Payments, Move Requests, and Admin Tasks.
- Can edit and reschedule existing taster records.
- Hotleads view shows own claimed leads plus unclaimed leads.
- Overdue Payments defaults to own view only.
- Admin Tasks shows only own assigned tasks.
- Cannot access Account Admin.
- Cannot access Insurance Notices.
- To-Do sidebar opens the user's personal modal/board, not Staff Overview.

### Willow

- Same as staff for normal app use.
- Can access Insurance Notices and perform all actions there.
- Can edit, reschedule, and safely remove/archive permitted taster records.
- Hotleads, Overdue Payments, and Admin Tasks stay scoped like staff.

### Elle

- Manager-equivalent operational access.
- Can edit, reschedule, and safely remove/archive permitted taster records.
- Can access Staff Overview.
- To-Do buttons keep Elle board access where expected.
- Dashboard To-Do panel uses Elle's priority board sections.
- Elle's To-Do buttons route to the Elle board, while managers/admins reach staff boards through Staff Overview.
- Elle's personal tasks do not appear in other staff personal lists.

### Tablet

- Can access tablet register and tablet resource tools.
- Cannot access normal desktop ops pages.
- Cannot see private admin/settings areas.
- Cannot remove/archive desktop taster records.

## 3. Public, Auth, And Shell

### Logged Out

- `/` loads and uses current otter branding.
- `/login` loads with current version number.
- Invalid login shows a clear error.
- Protected pages redirect to `/login?next=...`.
- `/robots.txt` and `/sitemap.xml` load.

### Logged In Shell

For admin, manager, staff, willow, and elle:

- Sidebar loads with correct role-specific links.
- Top-left brand says `For Pennine Gymnastics`.
- Page navigation does not show blue browser outlines.
- Sidebar To-Do count is aligned on the far right.
- To-Do button behaves correctly for the role.
- Settings button works.
- Sign out works.
- Help/Suggestion modal opens, sends, and closes.
- Page loading spinner appears briefly on navigation and does not get stuck.

## 4. Dashboard

Test as admin/manager/staff/willow/elle.

- Dashboard loads quickly.
- KPI cards show plausible counts.
- Today's Tasters card shows programme, class type, time, and class length without repeated `Pennine Gymnastics`.
- Empty programme columns show sensible empty text.
- Hotleads summary card links/buttons work.
- Toolbox buttons work:
  - Add Taster
  - Search
  - To-Do / Notes
  - Admin Tasks
  - Hotleads
  - Overdue Payments
  - Settings
  - Sign Out
- Admin Days summary shows only useful current data.
- If user has To-Do items, dashboard shows the To-Do panel.
- If user has no To-Do items, dashboard falls back to the calendar panel.
- Dashboard To-Do rows can be edited, completed/uncompleted, and deleted.
- Completed To-Do rows grey out clearly.
- Elle account sees Elle `Urgent` tasks on Dashboard, not the normal personal To-Do list.
- No card overlaps, dead space, clipped text, or broken scroll areas at laptop and desktop widths.

Cleanup:

- Delete any dashboard-created To-Do test items.
- Confirm no dummy items remain in personal/team/Elle To-Do views.

## 5. To-Do Lists

### Personal To-Do Modal

Test as staff/willow.

- Sidebar To-Do opens modal.
- Top-right To-Do opens modal where present.
- Add a task.
- Confirm `Added by` and `Added` date show.
- Click task text to edit and save.
- Tick complete and confirm `Completed` date appears.
- Untick complete and confirm it returns to open state.
- Delete task with bin button.
- Modal does not refresh or close unexpectedly.

### Staff Overview

Test as admin/manager.

- Sidebar Staff Overview opens `/staff`.
- `/todos` redirects to `/staff` for admin/manager.
- Staff and willow sidebar To-Do still opens the personal modal, not `/staff`.
- Tablet cannot access `/staff`.
- Staff list shows operational staff accounts only: no QA accounts and no non-operational service/admin accounts.
- Staff cards show role, initials, admin days, open admin tasks, overdue tasks, payment workload, and claimed hotleads.
- Staff list does not show email, workload, To-Do, or Due stat clutter.
- Clicking a staff card opens `/staff/<id>`.
- Staff profile shows KPI cards and sections for Admin Tasks, To-Do List, Hotleads, Overdue Payments, and activity/stats.
- Scoped Admin Tasks link preserves the staff filter.
- Staff profile To-Do area has a clear Add Task action and an Open Board/Open List action.
- Add a task to another account from the profile with due date and neutral/!/!! priority.
- Extended-account tasks land in the matching priority board; normal accounts keep the standard personal list.
- Open Board/Open List opens `/staff/<id>/todo` for manager/admin review.
- Edit/complete/uncomplete/delete the task from the opened board/list.
- Completed rows grey out clearly.
- Elle profile keeps an Elle Board link.

### Elle Board

Test as admin/manager/elle.

- Elle button opens `/elles-todo`.
- Urgent, Important, and When I Can fill the top row.
- Notes and Monday Tasks sit below.
- Add/edit/complete/delete in:
  - Urgent
  - Important
  - When I Can
  - Monday Tasks
- Monday Tasks default list is present:
  - Clear Hello emails
  - Check schedule for upcoming week
  - Clear Connect Teams requests
  - Update workloads with failed payments
  - Publish schedule for 4 weeks in adv
- Notes save and persist.
- Elle account keeps Elle board behaviour and also has Staff Overview access where manager-equivalent access is expected.
- Admin/manager can return from Elle board to Staff Overview or their previous page.

Cleanup:

- Delete all dummy tasks from all lanes.
- Remove dummy notes or restore original notes.

## 6. Today, Day, And Month

### Today / Day

- `/today` opens or redirects to the correct day view.
- Programme sections render correctly.
- Taster rows show clean class labels without repeated `Pennine Gymnastics`.
- Notes, insurance indicators, and status pills render correctly.
- Action buttons work where present.
- Insurance notice warning is docked with the bottom snapshot area on desktop.
- If a BG/insurance notice exists for the day, tablet view shows a clear warning above Daily Spark.
- Day Snapshot and Session Progress stay attached to the bottom where designed.
- Scroll position is usable and not jumpy.

### Month

- `/month` loads quickly.
- Month navigation works.
- Day cells show counts correctly.
- Clicking a day opens the day view.
- Calendar scales on desktop/laptop widths.

## 7. Add Taster

### Normal Add Flow

- Open `/add`.
- Select a programme/class tile.
- Add `ZZ Test ... Taster`.
- Add a note.
- Continue to review.
- Change the class/date and confirm the review does not expire.
- Confirm the taster.
- Confirm redirect and success message.
- Verify the record appears in:
  - Day view
  - Today if date is today
  - Search
  - Admin Tasks if it requires action
- From Day view, open the taster `More` menu and confirm `Edit` opens for every non-tablet role.
- From Day view, confirm `Reschedule` is separate from `Edit` and opens Add Taster in reschedule mode.
- Save a safe edit and confirm it returns to the expected page, not Dashboard.

### Review Modal / Confirmation

- Back/cancel controls work.
- Taster details remain correct after changing class.
- Duplicate confirmation does not create accidental duplicates.
- Reschedule same-class and new-class paths keep the duplicate guard intact.

### Manual Add

- Open manual add path if available.
- Add a safe dummy record.
- Confirm it appears in Search and relevant day view.

Cleanup:

- Remove/archive the dummy taster from Day view, Search, and Edit Taster in separate passes.
- Confirm each destructive action opens a confirmation that names the child/date/type and explains the live views it will leave.
- As admin only, confirm permanent delete is available behind confirmation where supported.
- Confirm it is gone from Day, Today, Dashboard, Search, and Admin Tasks.
- Confirm archived/completed records are visible only from the correct archive/completed/cleanup views.

## 8. Search

Test as admin, staff, willow.

- `/tasters` loads quickly.
- Search input is visible at the top of the page without opening Filters.
- Search input placeholder is `Search name, class, date, notes...`.
- Typing is not laggy.
- Pressing Enter updates the URL/query/results.
- Clear search appears when a query is active and clears URL/query/results.
- Filters drawer search field mirrors the top search input.
- Default sort is `Date Added (Newest First)`.
- Back/Next paging works at 50 records per page by default.
- Helper text clearly shows current range, total, page, active search text, and active filter count.
- Record type pills fit and text is centered.
- Light and dark mode pills are readable.
- Search includes all record types for all non-tablet roles.
- Added By filter shows first names and hides people with no records in the current result set.
- Clicking an Added By initials box filters to that person.
- Test filters:
  - Record Type
  - Added By
  - Admin Days
  - Programme
  - Attendance
  - Taster Selections
  - LoveAdmin
  - BG
  - Hotlead Status
  - Unknown Class / Day
- Test sort options:
  - Date Added newest/oldest
  - Date of Class newest/oldest
- Open `More` on each available row type.
- Test row actions:
  - Open / View Details
  - View/Add Note
  - Edit
  - Reschedule where applicable
  - Remove/archive where allowed
- Row action icons have labels or tooltips.
- Selection/cleanup controls do not appear to non-admin users unless usable.
- Payment contact icons show call, voicemail, and email correctly.
- Payment rows have spacing between icons/Open/More consistent with other rows.

### Admin Cleanup Mode

- Cleanup Mode is visible only to admin users.
- Non-admin users cannot access cleanup preview or execute routes directly.
- Cleanup Mode checkboxes appear only when cleanup is active.
- Normal row actions are secondary/disabled while selecting records.
- Select visible dummy records only; do not use real records.
- Preview shows record type, name, date, status, and linked-data warning.
- Cleanup blocks without typing `DELETE SELECTED`.
- Cleanup blocks more than 25 selected records.
- Successful cleanup writes an audit log entry for every deleted record.
- Cleaned records disappear from Search and their source pages.

Cleanup:

- Search run prefix and remove every dummy record.

## 9. Admin Tasks

Test as admin, manager, staff, willow.

### Layout

- Title/header sits at the top.
- Top-right controls are not inside an unwanted background card.
- To Action is the dominant visible work area.
- To Action reaches the usable bottom of the screen and scrolls internally.
- Right rail/support cards do not overpower the task list.
- My Admin Days view combines At a Glance and Workload Mix into one compact Workload Overview card.
- All Days view keeps Open Admin Tasks in the right rail and that card reaches the usable bottom of the screen.
- Workload Mix shows only big labels and values, no subtext.
- No clipped rows, dead space, or trapped scroll areas.

### Visibility

- Admin/manager can switch:
  - My Admin Days
  - All Days
  - specific staff from Open Admin Tasks
- Staff/willow cannot switch to All Days or specific staff.
- Open Admin Tasks only appears for admin/manager all-days style views.
- Open Admin Tasks staff rows show Total first, omit HL Due/Movers, and scale without clipping.
- Open Admin Tasks staff rows link to the staff profile in Staff Overview.
- Manager/admin staff task navigation should prefer Staff Overview and staff-specific To-Do board/list links.
- Staff/willow only see their own assigned tasks.

### Ordering

- Overdue Hotleads appear first.
- Overdue Payments appear next.
- Tasters appear after payments.
- Leavers, movers, insurance, and other tasks follow.
- Recently Completed / Archive behaves separately from live To Action.

### Recently Completed / Archive

- Live mode shows the normal To Action queue.
- Header button says `Show archive only`.
- Recently Completed / Archive appears at the bottom when matching rows exist.
- The section includes:
  - manually archived admin tasks
  - completed tasters
  - completed leavers
  - completed movers
  - completed insurance notices
  - completed hotleads
  - completed/deleted payment chases
- Rows are newest first.
- Archived rows show `Archived`.
- Completed rows show `Completed`.
- Archived rows have a restore button.
- Completed-only rows do not have a restore button.
- Clicking `Show archive only` opens `/admin/tasks?...&archive=only`.
- Archive-only mode hides live To Action rows.
- Archive-only mode title is `Recently Completed / Archive`.
- Archive-only mode button says `Show live items`.
- In All Days with selected staff, archive-only preserves `view=all` and `staff_user_id`.
- Restoring an archived row from archive-only mode stays in archive-only mode.
- Staff/willow only see their own scoped completed/archive rows.
- Admin/manager can view selected staff completed/archive rows.

### Row Actions

Press every visible button type at least once on safe dummy records:

- Taster:
  - copy name
  - Attendance
  - Fees
  - BG
  - Badge
  - More
  - Mark Missed
  - Contacted
  - Note
  - Edit
  - Remove/archive
- Missed/contacted tasters stay visible and show indicators instead of disappearing.
- Payment Chase:
  - Add Contact
  - choose Call
  - choose Voicemail
  - choose Email
  - confirm it stays on Admin Tasks
  - Open
  - Archive
- Hotlead:
  - Open
  - archive/snooze if safe
- Leaver:
  - Inactive
  - BG
  - Board
  - Note
  - Edit
  - Remove/archive
- Mover:
  - Attendance
  - Moved
  - Note
  - Edit
  - Remove/archive
- Insurance:
  - Attendance/Missed
  - BG
  - Message Willow
  - Note
  - Remove from Admin Tasks

Cleanup:

- Undo toggles where needed.
- Delete or remove all dummy records.
- Confirm no dummy rows remain in To Action, Recently Completed / Archive, or archive-only mode.

## 10. Hotleads

Test as admin/manager/staff/willow/elle.

### Access

- Admin/manager can view all hotleads and stats.
- Staff/willow can see own claimed hotleads and unclaimed hotleads.
- Tablet cannot access.

### This Month Leaderboard

- Admin/manager can open the Hotleads leaderboard view.
- Admin/owner accounts are excluded from ranked staff rows.
- Claimed, booked, no-space, active, due today, overdue, and coming-up totals render correctly.
- Switching between My Hotleads, staff views, and leaderboard preserves the correct mode.
- Staff/willow cannot access all-user leaderboard controls.

### Add / Claim / Duplicate Safety

- Add `ZZ Test ... Hotlead` with optional note.
- Confirm it appears in Claim Hotleads.
- If note exists, claim row shows a note indicator but does not expose private note text.
- Open `...` on unclaimed row:
  - Add/View Note opens the same note modal
  - Cancel returns cleanly
  - Claim works
- Add the same hotlead again.
- Confirm no duplicate is created.
- Confirm existing active hotlead is updated/merged safely.

### Popup Workflow

- Click collapsed hotlead name cell.
- Popup opens quickly.
- Row does not expand inline.
- Hover highlights the whole card, not just the name.
- Copy name button works.
- X closes popup quickly and returns to Hotleads.
- Popup buttons do not full-page reload.
- Scroll position is preserved.

### Follow-Up Steps

For a dummy claimed hotlead:

- Initial Contact row shows correct buttons.
- Complete Call / Voicemail / Email as appropriate.
- Completed past step can be clicked and edited if a wrong button was pressed.
- Next Day Follow-up is due the day after first contact.
- Two Days Later and Final Follow-up schedule correctly.
- Final Follow-up marks done when required actions are recorded.
- Overdue steps are visually obvious.
- Queued steps are visible but muted.

### More Menu / Modals

- View History opens.
- Close History returns to the same open hotlead popup.
- View/Add Note opens.
- Cancel or close Note returns to the same open hotlead popup.
- Schedule Taster works.
- Mark No Space works.
- Complete/archive workflow works.
- Completed hotlead history shows completed date and `No contact` for attempts that were not needed.

Cleanup:

- Delete/archive the dummy hotlead until it no longer appears in Hotleads, Search, Dashboard, or Admin Tasks.

## 11. Overdue Payments

Test as admin/manager/staff/willow/elle.

### Access And Views

- Admin/manager/elle can switch See Mine/See All.
- Staff/willow see own payments only.
- All non-tablet users can add overdue payments.
- Page title changes between My/All Overdue Payments correctly.

### Add / Duplicate Safety

- Add `ZZ Test ... Payment`.
- Confirm `Added: <date>` appears.
- Add the same overdue payment again.
- Confirm it does not duplicate.
- If an old completed/deleted/plan record exists, confirm adding it reactivates the follow-up safely.

### Active Row Actions

Press every relevant button:

- Add Contact
  - Call
  - Voicemail
  - Email
- Add Note
- Paid
- Payment Plan
- Left the club
- More
  - Add Note
  - Edit
  - View History
  - Delete

### Indicators And History

- Contact icons show correctly:
  - phone for call
  - mic for voicemail
  - envelope for email
- Notes appear in dated history but do not inflate contact pill counts.
- Paid entries show green `Paid` pill.
- Payment Plan entries show plan status.
- Left the club entries show the left-club outcome and offer Add as leaver.
- Add as leaver opens Record Leaver with child/programme/reason context.
- Paid, Payment Plan, and Left the club all remove the item from the live Overdue Payments page and Admin Tasks payment chase rail.
- Deleted entries in Completed section show `Deleted` pill.
- Deleting active payment moves it to Completed/Deleted area.
- Deleting again from completed area permanently removes it.

Cleanup:

- Fully delete dummy payment.
- Confirm it is gone from Overdue Payments, Search, Dashboard, and Admin Tasks.

## 12. Record Leaver

- Open `/leaver`.
- Add `ZZ Test ... Leaver`.
- Confirm class table formatting matches Add Taster.
- Confirm record appears in:
  - Search
  - Admin Tasks
  - relevant day/month if applicable
- Test Admin Tasks buttons:
  - Inactive
  - BG
  - Board
  - Note
  - Edit
  - Remove/archive
- Edit leaver and confirm changes persist.
- Open manual leaver path if available.
- Add and remove a safe manual leaver record.

Cleanup:

- Delete or archive the dummy leaver fully.
- Confirm it is gone from Search and Admin Tasks.

## 13. Add Mover And Move Requests

### Add Mover

- Open `/mover`.
- Add `ZZ Test ... Move`.
- Confirm class table formatting matches Add Taster.
- Confirm it appears in Search/Admin Tasks.
- Test Admin Tasks buttons:
  - Attendance
  - Moved
  - Note
  - Edit
  - Remove/archive

### Move Requests

- Open Move Requests.
- Add a dummy request.
- Add the same child/DOB again with a changed requested day/programme.
- Confirm the existing active request updates or records a safe note trail instead of duplicating.
- Use Add Callback to create a callback/starter request with exact DOB, programme/facility, requested day, due date, and notes.
- Confirm callback/starter entries appear in the Callbacks / Starters list and match normal move-request row height.
- Filter by request/callback type and confirm each list behaves independently.
- Set a callback due date and confirm it appears in Admin Tasks for the user who added it when due.
- Toggle contacted, postpone callback, schedule taster, add note, archive, and reactivate linked hotlead where applicable.
- Test open/complete/archive/delete actions.

Cleanup:

- Remove dummy mover and move request.
- Confirm no dummy move records appear in Search or Admin Tasks.

## 14. Insurance Notices

Test as willow, manager, admin. Confirm staff cannot access.

- Open Insurance Notices.
- Add `ZZ Test ... Insurance`.
- Confirm notice appears in list.
- Add the same child/date/programme again.
- Confirm it updates existing notice instead of duplicating.
- Test:
  - Edit
  - Add/View Note
  - BG done
  - Attended/Missed
  - Message Willow
  - Archive/delete
- Confirm notice appears in Today/Day as a clear daily warning.
- Confirm tablet Today shows the laptop-check warning above Daily Spark.
- Confirm Admin Tasks shows insurance rows where relevant.

Cleanup:

- Delete/archive dummy notice.
- Confirm it is gone from Insurance Notices, Today, Search, and Admin Tasks.

## 15. Stats

- `/stats` loads without server error.
- Sections collapse and expand:
  - Overview
  - Tasters
  - Hotleads
  - Overdue Payments
  - Insurance
  - Movers / Leavers
  - Admin Days
- Collapsed/open section state persists after reload.
- Main totals render at top.
- Monthly Breakdown scrolls inside its card.
- This Month By Programme card height aligns with Monthly Breakdown.
- Charts render:
  - Taster Pipeline
  - Hotleads
  - Overdue Payments
  - Insurance Notices
  - Movers and Move Requests
- Supporting stat blocks have aligned chart heights.
- No chart is blank.
- Light and dark mode remain readable.

## 16. Account Settings And Admin Console

### Settings

- Profile fields load.
- Theme toggle works.
- Email Preferences show monthly dates with ordinals such as `7th`.
- Notes/settings save.

### Account Admin

Test as admin only.

- Account list loads.
- Create account form works with safe dummy account if needed.
- Role selector includes:
  - Admin
  - Manager
  - Staff
  - Willow
  - Elle
  - Tablet
- There is no old owner role.
- Old logo toggle is gone.
- Role changes apply correctly.
- Password/set-password flow works.
- Audit/log/action areas load.
- Manager cannot access Account Admin.

### QA Tools

Test as admin only.

- QA Tools card is visible in Account Admin.
- Shared QA password is shown only inside the admin-only QA Tools card.
- `Create Missing QA Accounts` creates only:
  - `qa-staff@tasterist.test`
  - `qa-willow@tasterist.test`
  - `qa-manager@tasterist.test`
  - `qa-elle@tasterist.test`
  - `qa-tablet`
- Running create again does not duplicate QA accounts.
- `Reset QA Passwords` resets only those fixed QA accounts to the shared QA default.
- `Force First-Login Reset` marks only non-tablet QA accounts for password reset.
- `Reset QA Admin Days` restores only QA admin-day defaults.
- QA Tools never edits/deletes/demotes the current admin account.
- QA Tools never touches real non-QA accounts.
- Audit Logs record every QA create/reset/admin-day action.
- Sign in as each QA account after reset and confirm the expected role path:
  - QA Staff: staff navigation and normal personal To-Do.
  - QA Willow: staff behavior plus Insurance Notices.
  - QA Manager: manager navigation and team To-Do visibility.
  - QA Elle: manager-equivalent access plus Elle board/urgent dashboard list.
  - QA Tablet: tablet setup/location flow.

Cleanup:

- Delete or disable any dummy account.
- Leave fixed QA accounts in place unless deliberately testing account removal.

## 17. Admin Maintenance And Support Utilities

Test as admin unless stated otherwise.

### Class Uploads / Import

- Open class upload/import area if enabled.
- Confirm page loads without exposing private data unexpectedly.
- Upload controls are visible and labelled clearly.
- Invalid file type or empty upload fails safely.
- Valid test upload path can be cancelled before committing, or tested only on local/staging.
- Any import preview uses clean class labels without repeated `Pennine Gymnastics`.
- No accidental duplicate sessions are created.

Cleanup:

- Remove any imported dummy sessions or records.
- Confirm Search and Month do not show import leftovers.

### Backups

- Open backups page if available.
- Confirm backup list loads.
- Confirm download/action buttons are present for allowed roles.
- Do not run destructive restore/delete actions on production.
- If local/staging, test one backup/download path and confirm no error.

### Cloud Preflight / Dev Tools

- Open cloud preflight page if available.
- Confirm checks render clearly.
- Confirm no secret values are printed to the browser.
- Confirm unsupported roles cannot access dev/admin-only tools.

### Support / Suggestions

- Submit a safe support suggestion.
- Confirm admin/support view shows it.
- Send a support reply if safe.
- Confirm reply template uses new branding.
- Confirm support report email path does not error.

Cleanup:

- Archive or mark handled any dummy support report if the app supports it.

### Taster Changelog / My Notes / Print

- Open Taster Changelog from Search or top-right button where present.
- Confirm entries load and filtering/navigation works.
- Open My Notes if linked.
- Confirm notes save and reload.
- Test print view from Day/Taster if available.
- Confirm print layout is readable and does not expose unrelated private UI.

Cleanup:

- Remove dummy notes or restore original note content.

## 18. Tablet Today

Test as tablet account on tablet-sized viewport.

- Tablet login works.
- First tablet login/location reset prompts for Honley or Lockwood.
- Honley tablet preference defaults tablet workflows to Honley.
- Lockwood tablet preference defaults tablet workflows to Lockwood and Preschool.
- Tablet location preference persists after leaving and returning to tablet pages.
- Change Location lets staff switch the tablet preference deliberately.
- Today Register loads fast.
- Refresh button works.
- Sign Out button works.
- Attendance button works desperately:
  - Mark Attended
  - Confirm it changes to Attended
  - Tap again if supported and confirm correct state
  - Refresh and confirm state persists
- No bottom device nav overlaps name/sign-out/footer controls.
- Daily Spark appears.
- Insurance notice warning appears above Daily Spark if there is a BG notice that day.
- Warning copy tells staff to check laptop.
- Tablet controls are large enough and easy to tap.

## 19. Tablet Resources

### Public Resources And Daily Spark

- `/resources` loads without login.
- `/tablet/resources` loads for tablet accounts.
- Daily Spark shows Joke of the Day, not historical On This Day content.
- Daily Spark does not clip under the tablet home button after refresh.

### True Or False

- Page loads.
- Category filters work.
- Difficulty filters work:
  - Easy
  - Medium
  - Hard
- Filters box fits tablet screen compactly.
- Only unused toggle works.
- Questions are not obvious/easy throwaways.
- True and False buttons work.
- Counts update.
- Reset/next question paths work.

### Deck Of Cards

- Deck loads.
- Otter card image appears.
- Tap to deal works.
- Reset deck works.
- Ring of Fire link works.
- Ring of Fire/deck state survives accidental navigation away and back.
- No old deck icon remnants show.

### Colour Score Game

- Colour Score Game loads from tablet resources.
- Score cards scale to tablet width.
- Scores can go into negative numbers.
- Green team shows the leaf icon.
- Scores persist on the device after navigation/refresh.

### Stopwatch / Timer

- Start, pause, reset work.
- Timer values are readable on tablet.
- No layout clipping.

### Whiteboard

- Drawing works.
- Clear/reset works.
- Screen scales on tablet.
- Whiteboard saves separately for Honley and Lockwood tablet preferences.

## 20. Emails And Notifications

Where safe, trigger or inspect:

- Account created email.
- Support reply email.
- Support report/admin notification.
- Weekly admin report.
- Willow notice message.
- Overdue payment email draft if present.

Confirm:

- new otter branding is used everywhere.
- no old logo/template variant is sent.
- plain text fallbacks exist.
- links point to the expected app routes.

## 21. Performance And UX Smoke

Run this after functional testing:

- Dashboard feels near-instant.
- Search input focuses immediately.
- Search paging is quick.
- Hotlead popup opens and closes quickly.
- Hotlead popup actions do not reload the page unnecessarily.
- Admin Tasks scrolling is smooth.
- Overdue Payments actions do not feel stuck.
- Tablet Today attendance is one-tap and fast.
- Page loading overlay appears only during real navigation and never gets stuck.
- No route produces an internal server error.

Suggested routes to smoke as authenticated admin:

```text
/dashboard
/today
/month
/tasters
/stats
/admin/tasks
/hotleads
/admin/payments
/moves
/insurance
/todos
/elles-todo
/account
/account/admin
/tablet/today
/tablet/resources
```

## 22. Final Cleanup Sweep

Use the unique run prefix and check every major area:

- Search `/tasters`
- Dashboard
- Today
- Relevant Day view
- Month
- Admin Tasks
- Hotleads
- Overdue Payments
- Move Requests
- Insurance Notices
- Personal To-Do boards/lists
- Staff Overview-assigned tasks
- Elle To-Do
- Support/Suggestions
- Import/Class Uploads
- Account Admin if a dummy account was created

For each dummy record:

- delete or archive it fully
- if deletion first moves it to a completed/deleted/archive bucket, delete it again there
- refresh the page
- search again
- confirm zero unwanted `ZZ Test ...` records remain

Final sign-off:

- no dummy tasters remain
- no dummy hotleads remain
- no dummy overdue payments remain
- no dummy move requests/movers remain
- no dummy insurance notices remain
- no dummy to-do tasks remain
- no dummy accounts remain
- no internal server errors seen
- no permission leaks found
- no page is unusably slow

## 23. Release Sign-Off Summary

```text
Run date:
Tester:
Environment:
Commit/deploy:
Roles tested:
Viewport/device notes:

Passed:
Failed:
Known issues accepted:
Cleanup confirmed:
Ready to deploy? yes/no
```
