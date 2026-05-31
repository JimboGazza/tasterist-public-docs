# Tasterist Beautify and Fix Agent Checklist

Use this when you want an agent to do a design-polish and fix-focused review of the app after a functional pass.

This checklist is intentionally biased toward:

- making the app feel more professional
- spotting awkward spacing, clutter, and visual inconsistency
- finding small logic issues that damage trust
- proposing restrained improvements, not broad redesigns

## Review goal

Review the current Tasterist app like a careful product designer plus QA partner.

Focus on:

- visual polish
- spacing and alignment
- information density
- action clarity
- trust and safety cues
- stale or confusing logic
- places where the UI feels messy, noisy, cramped, or inconsistent

Do not turn this into a feature brainstorm. Stay anchored to improving the existing product.

## Test setup

- Test the target environment we actually care about: local, staging, or live.
- Start with an owner account.
- If possible, also spot-check:
  - one staff or manager account
  - one tablet account
  - one logged-out/public session
- Test first at a normal desktop/laptop width.
- Then test again at a narrower laptop width.
- Small-phone behaviour can be noted, but this pass is desktop-first.
- Avoid destructive actions unless the environment is safe for them.

## What to look for

### 1. Visual polish

- Does the page feel balanced, or does one area feel too empty or too cramped?
- Are cards aligned cleanly?
- Do sections have clear hierarchy?
- Are headings, helper text, and buttons spaced consistently?
- Do grids, tables, and action rows feel deliberate rather than crowded?
- Are there places where one odd component breaks the visual language?

### 2. Workflow clarity

- Can the user tell what to do next?
- Do multi-step flows offer Back or Cancel where expected?
- Do save, confirm, archive, delete, and toggle actions feel safe and understandable?
- Is feedback immediate and obvious after actions?
- Do search/filter actions make it clear that something happened?

### 3. Trust and professionalism

- Do metrics feel believable and internally consistent?
- Are warnings and destructive actions clearly explained?
- Does the app avoid “messy admin tool” energy?
- Are backup, email, health, and settings surfaces calm and trustworthy?
- Is copy confident and clean rather than internal or technical?

### 4. Logic confidence

- Does the visible state match the summary cards and counters?
- Do toggles make sense and update consistently?
- Do rows disappear, move, or update when users would expect them to?
- Do filters, counts, and summaries feel stale anywhere?
- Are there places where the user could misunderstand the meaning of a status?

### 5. Accessibility and readability

- Is text contrast good enough?
- Are important states communicated by more than color alone where practical?
- Are tap targets/buttons large enough on dense rows?
- Are focus states and hover states noticeable?
- Does any important action look too similar to a secondary one?

## Pages to review closely

### Public surfaces

- `/`
- `/login`
- `/set-password`

Check:

- hero spacing
- trust signals
- CTA hierarchy
- support visibility
- overall professionalism

### Core operations

- `/dashboard`
- `/today`
- `/month`
- several `/day/...` pages
- `/tasters`

Check:

- visual density
- action row clarity
- empty states
- note visibility
- summary-card trustworthiness
- search/result visibility

### Admin operations

- `/admin/tasks`
- `/hotleads`
- `/admin/move-requests`
- `/account`
- `/account/admin`
- `/backups`
- `/cloud/preflight`

Check:

- whether admin pages feel clean or overloaded
- whether destructive actions feel safe
- whether settings and system pages feel calm and well explained
- whether backup and health surfaces look trustworthy enough for a business product

## Priority lens

When reporting issues or suggestions, prioritise them like this:

- `now`: damages trust, clarity, safety, or the feeling of professionalism
- `soon`: noticeable clutter, awkwardness, or repeated friction
- `later`: nice-to-have refinement that can wait

## Issue report format

Use this for bugs, layout problems, confusing logic, or trust issues:

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
Why it matters:
Severity: blocker / logic / polish
Priority: now / soon / later
Attach:
```

## Suggested change format

Use this when the page technically works but still feels off:

```text
Suggestion title:
Area:
Current behaviour:
Why it feels off:
Suggested change:
What good would look like:
Priority: now / soon / later
Optional screenshot:
```

## Guidance for the agent

- Prefer small, high-confidence improvements over sweeping redesign ideas.
- Be specific. “Spacing is bad” is not useful; explain which section, what feels crowded, and what would improve it.
- Separate visual polish from logic issues.
- Treat trust problems seriously, even if the app still works.
- Call out if something only feels wrong on a very small screen and is otherwise acceptable on desktop.
- Do not invent problems just to fill the report.
- If something is intentionally dense but still usable, say that plainly.

## Desired outcome

At the end of the pass, we should have:

- a short list of the highest-value beautify/fix changes
- a clear split between real bugs and visual polish
- enough detail to implement the next pass without guesswork
