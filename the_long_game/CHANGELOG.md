# Changelog

## Unreleased

## 0.1.47 - 2026-11-15

- CI: coverage from the unit tests and the three PostgreSQL test runs is combined before
  the threshold is enforced, so the database code counts too. The threshold stays at 86%.

- **Read to me (optional).** Story lessons show a *Read to me* button that reads the
  story, the question and the answers aloud with the device's own voice. It appears only
  when the browser can speak; nothing is sent anywhere. Tap again to stop.

- **The two archive pages are gone.** *Scenarios (archive)* and the read-only per-chapter
  market setup were removed (about 900 lines). Their old addresses lead to *Yearly
  cycles*. Stored data is untouched; a child without a running year still plays market
  chapters with the setup published earlier, but that setup can no longer be viewed.

## 0.1.46 - 2026-11-14

- **Lessons C09-C50 need the right answer too.** Each of the 34 lesson chapters after
  Stage 1 now offers the right answer, a common mistake and *ask for help*; only the right
  answer completes it (definition version 2). Lessons already opened or completed keep
  their version. Calculation and market chapters are unchanged.
- **Children make their own saving goals.** *My money* has a *Make your own goal* form:
  a name and whole francs from 1 to 10 000, up to five goals per child. The goal from
  lesson C05 stays; C05 now says that own goals can be added.
- **Shorter Stage 1 lesson titles** for ages 6-8, for example *Money and Prices* instead
  of *Money, Prices and Value*. The curriculum keeps the full titles.
- **Age group check for each new year.** *Plan next year* shows the child's age group
  and asks whether it is still right; a change is saved with the new year. Nothing else
  about the child changes, and no birth date is stored.
- **Reset a forgotten parent password in Home Assistant.** Two new optional app
  settings, `reset_parent_sign_in_name` and `reset_parent_password`, set a new password
  for a parent account at the next start. Only Home Assistant administrators can change
  app settings. The password is never logged; the log says whether the reset was applied
  and reminds you to remove the settings. Child accounts are not affected.

## 0.1.45 - 2026-11-14

- **Start-up messages are visible again.** Running the database migrations at start
  switched off the app's other log messages. Since 0.1.43 this also hid the
  `Cannot start:` line that names a database problem, and the log never showed
  `Application startup complete.`, which the installation guide tells you to look for.
  Both now appear.
- Documentation: history notes that earlier version bumps had changed by mistake show
  their true versions again (archive since 0.1.38, editor code removed after 0.1.42).

## 0.1.44 - 2026-11-14

- **Add a child by name.** The *Add child* form asks only for a first name or nickname
  and the age group. The app creates the sign-in name from it (`Lena` -> `lena`,
  `Zoë Müller` -> `zoe_muller`, `lena_2` when the name is taken) and never reuses an
  account that already exists. Participant ID and principal ID are no longer typed.
- **Children are shown by name** in Manage, the overview, approvals, activity and
  mastery, instead of "Child 1". Children added earlier keep "Child 1" until you give
  them a name on their card; renaming never changes the sign-in name.
- **Short sign-in names.** Children sign in with `lena` and parents with `alex`; the
  `principal_` prefix is no longer needed (it still works). The password check is
  unchanged.
- Database migration `0019_child_display_name`: one optional column for the child's
  name (1-40 characters). No data changes on upgrade.
- Installation guide rewritten as six steps, all inside Home Assistant: install
  PostgreSQL and pgAdmin, connect pgAdmin, create the database, install from the app
  repository (experimental warning, *Show in sidebar*), start with the exact log lines to
  look for, and a final checklist. Installing from a local bundle moved to a developer
  section.

## 0.1.43 - 2026-11-14

- **The log says why the app cannot start.** When the database cannot be used, the
  app stopped without a reason. The log now shows one line starting with
  `Cannot start:` that names the cause (wrong user or password, missing database,
  unknown host, server not reachable, missing permission). The URL and the driver's
  message are never logged.
- Installation guide: step-by-step PostgreSQL setup in pgAdmin, how to build the
  database URL, and troubleshooting for a lost URL, a forgotten parent ID or password,
  and an unexpected *Create a parent account* page.


## 0.1.42 - 2026-11-13

- The app repository is https://github.com/hubj66/the-long-game-app. The app is published
  as *experimental* (an early test release, not ready for family use yet); Home Assistant
  warns before installing it.

- Public documentation (issue #234): a shorter README, an installation guide, an upgrade
  guide and a release process. `scripts/build_app_repository.py` builds the Home Assistant
  app repository for a release, and the publish workflow stops when the tag does not match
  the app version.

- Removed about 300 lines of code that could no longer be reached: the old per-chapter
  market editor page, its review panel and the scenario write path. The read-only archive
  pages are unchanged. No change for parents or children.

Fixes from a review of the learning path for ages 6-8 (issue #307):

- **A lesson needs the right answer.** In Stage 1 (C01-C08) any click, even *Help me
  count*, completed the lesson and unlocked the next one, so a child could unlock the
  fund without one right answer. Lessons now have one right answer, one wrong answer and
  *ask a grown-up*; only the right answer completes the lesson, and the child can try
  again as often as needed.
- **The story comes before the question.** Lessons are now Read, Answer, Check, Done
  (four steps instead of six). The market-story texts ("what happened to an investment",
  "no more months") no longer appear in lessons, and the answers no longer show a hint
  that gives the right one away.
- **New lesson texts (definition version 2)** with shorter sentences. C05 explains a
  savings account and interest; C08 explains what a fund is. Version 1 stays registered:
  lessons already opened or completed keep their version and stay readable.
- **The chapter list shows the current stage only** (and earlier ones), not all 50
  chapters.
- Lessons of later stages also use the story steps; they still complete with any choice
  until they get right answers of their own.

- Development dependency `setuptools` updated to 84.2.1 (low-severity advisory that
  affects wheel installation on Windows only; the add-on was not affected).

## 0.1.41 - 2026-11-13

- **Simpler child home page for ages 6-8.** The home page no longer shows activity
  cards that say *Resting for now*; only steps the child can use today appear. The menu
  is shorter: Home, My year, My money and Chapters. *Invest* and *Check plan* are reached
  from the home page and show in the menu only while the child is on them.

## 0.1.40 - 2026-11-13

Fixes from a two-year walkthrough with a simulated clock (real factory and PostgreSQL)
and a phone/tablet check:

- **Choice windows stay open for at least 14 days.** Five years of history in one year
  make a period month about six real days, so a decision moment was open for only about
  six days and easy to miss. The start and each decision window now last at least two
  weeks (never past the next decision or the year end); a choice still applies only from
  the month after the months already played. The child sees *You can choose until …*.
- **Publishing a locked tool needs an explicit early unlock.** A year with a tool the
  child has not unlocked yet (for example *One fund* before C08) was published silently.
  Publishing now stops with a clear message; the draft is kept; *Make available early*
  or switching the tool off resolves it.
- **Signup and sign-in accept natural device labels** such as "Joel's iPad" or "PC".
  Such labels used to create the account and then fail with *Invalid form data*.
- A largest fall of zero is shown as *0.0%*; long history IDs wrap on small screens.

## 0.1.39 - 2026-11-13

Fixes from an end-to-end walkthrough of a fresh install through the HA ingress path
(parent signup → household → child → starter library → example plan → publish → child
plays):

- Fix: on a fresh install the household setup page failed (500) after signing up,
  because the market permission check ran before a household existed. Manage now
  continues; other pages ask to create the household first.
- Fix: the child home page offered a fixed *Demo World ETF* that was not in the
  household's library. It now links to the child's real investment choices.
- Child home starts with **My investing year** and a *See my year* button.
- My year before the start: shows the start date, the planned split in CHF (*Your plan
  for the start*) and *Your choice counts from the start of your year*. A flat chart is
  drawn in the middle.
- My year after a mid-year choice: shows the split that applies now and *from next
  month: …%*, instead of the new choice next to the old amounts.
- A year can no longer be planned or published with a start date in the past (the
  child would miss the start). While a year has not started, the parent can **change
  its start date** on the setup page; the overview never proposes a past date.
- Empty Library: points to the starter library first; *Add investment* stays folded.

## 0.1.38 - 2026-11-13

- The per-chapter market editor and the scenarios page are a **read-only archive**
  (#287): they show the former setup (period, decision, investments, how many children
  started with it) with a link to Yearly cycles; forms sent to them return 410 and write
  nothing. Menu label *Scenarios (archive)*. Started chapters and archived setups keep
  working for children without a running year. Removal follows in a later release.
- Tests that edited through these pages now check the archive; the publish rules they
  covered are checked on the service directly, and coverage bars on the cycle setup page.

## 0.1.37 - 2026-11-13

- Market chapters read the child's running yearly cycle (#287): a chapter the child
  starts while a year is running uses that year's period and investments (first decision
  moment as the decision month). Without a running year the household's chapter setup
  applies as before. A started chapter never changes for the child. The parent chapter
  setup explains this. No migration.

- Upgrade rehearsal from 0.1.30 (#287): `scripts/qualify_upgrade_from_0_1_30.py` builds a
  synthetic 0.1.30 database, upgrades it to the current head twice and checks that all
  old rows and service reads are unchanged and that yearly cycles work on the upgraded
  data. CI runs it as a new `release-0130` database state. No application change.

## 0.1.36 - 2026-11-13

- One **Library** page (#287, part 2) at `/parent/household/library`: investments with
  their history state, then price histories (starter library and imports), then the
  older market packs. The detailed list of imported histories is folded (it opens when a
  history needs a quality review). The menu shows *Library* instead of *Investments*
  and *Data library*.
- Old URLs redirect: `/parent/household/investments` and `/parent/household/dataset`
  (and GET `/parent/household/investments/history`) lead to the Library. Forms from tabs
  opened before the update still work. No migration.

## 0.1.35 - 2026-11-13

- Parent menu follows the model (#287, part 1): **Overview**, then **Learn** (Chapters,
  Mastery), **Invest** (Yearly cycles, Portfolio, Approvals, Activity) and **Library**
  (Investments, Data library). *Legacy scenarios* moves under *Tools & settings*. Yearly
  cycles is now in the menu and marked as the current page on cycle pages. No URL
  changes, no migration.

## 0.1.34 - 2026-11-13

- Cycles overview and example plan (#286): **Yearly cycles** shows each child's years as a
  timeline with status, dates, tools used, balances and a *Now* marker on the running
  year. A four-year **example plan** ships with the package (synthetic starter data only):
  Year 1 savings with interest, Year 2 + one world fund, Year 3 + bonds and inflation,
  Year 4 + single companies. *Plan Year N from the example* and *Fill Year N from the
  example* turn a year into a normal draft; *Remove example plan* hides it (and *Show it
  again* brings it back). Your own cycles are never changed.
- Adding the starter library no longer creates the example draft on chapter 28; the
  example plan replaces it. Existing chapter drafts stay as they are.
- PostgreSQL migration `0018_cycle_settings` allows `setting` cycle records
  (append-only). Take a backup before upgrading.

## 0.1.33 - 2026-11-13

- The child plays the yearly cycle (#285): a new **My year** page (`/child/cycle`) shows
  the money now, the change since the start, how much of the year is done, a value chart
  and where the money is. In the start window and at each decision moment the child
  splits the money in whole percentages (the rest stays as cash); a choice applies from
  the next period month, so it never uses returns the child could not see yet. After the
  year the page shows a review, including what an all-savings year would have given.
- `cycle_simulation` replays a year from stored records with `Decimal` (34 digits) and
  half-even cent rounding, with golden fixtures. A finished year closes itself
  idempotently on the next child or parent visit and carries the money over.
- Parents can set **starting money** for the first year (default CHF 100.00); later years
  carry over. Publishing is refused when a price history does not cover the period.
- PostgreSQL migration `0017_cycle_choices` allows `choice` cycle records (append-only).
  Take a backup before upgrading.

## 0.1.32 - 2026-11-13

- Add parent pages for yearly cycles (#284): **Chapters → Yearly cycles** lists each
  child's years and plans the next one; each year has a setup page with 1. tools (with
  unlock reasons and *Make available early*), 2. investments from the library with
  return, largest fall and a coverage bar, 3. period and decision moments (*Fit period*,
  *Update preview*), 4. *Save draft* / *Save and publish*. A started year is read-only.
- Add interest rates as a time series (#282): value kind `annual_interest_rate`
  (one twelfth credited per month), import support, and `savings_interest.accrue` with
  half-even cent rounding and golden fixtures. Starter pack `2026.12` makes the Savings
  Account a synthetic interest-rate series; pack `2026.11` stays unchanged.

- Add yearly cycles (ADR-0007, #283): `CycleService` with create, draft, publish, tool
  overrides and idempotent close; one-year cycles per child with a free start date, no
  overlap, a setup fixed once running, and the closing balance carried over to the next
  cycle. `cycle_close.close_cycle` converts holdings to cash with golden fixtures.
  PostgreSQL migration `0016_yearly_cycles` adds append-only cycle tables.

- Add the tool ladder (ADR-0007, #281): eight tools from *savings account with interest*
  to *foreign currency*, unlocked by finishing chapters, with parent overrides,
  investment-to-tool mapping, selection checks and parent-facing explanations. Pure
  policy; no screen or stored data yet.

## 0.1.31 - 2026-11-13

- Fix: **Review publication** no longer asks to save an unchanged draft after a new
  import. Drafts are compared only on the investments they use (#277).
- **Make visible to children** button on hidden rows in the chapter editor, and a
  **Show new investments to children** option in the table import. Hidden and disabled
  investments are still never published (#278).
- Plain words in the editor (*Chart shows*, *Start*, *End*, *Decision moment*, *Largest
  fall*); history versions moved under *Advanced*. Import value types explained; source
  and notes optional with recorded defaults. "Scenario presets" renamed *Legacy scenarios*;
  older market packs collapsed under *Advanced* (#279).

## 0.1.30 - 2026-11-13

- Show why an investment cannot be used in a chapter period: "Outside period" with the
  months its history covers, summarized missing-month ranges instead of long month lists,
  and a coverage bar comparing each history with the chapter period (issue #271).
- Allow choosing an investment whose history does not fit the period yet, and add
  **Fit period to selected investments**, which sets the period to the months all chosen
  investments share. Publishing still requires every choice to be ready.
- Hide return figures and charts for rows without data in the period.

## 0.1.29 - 2026-11-13

- Add a multi-series table import: one CSV with up to 20 series, tolerant of Swiss and
  other spreadsheet exports (semicolons, decimal commas, `DD.MM.YYYY`, daily values
  reduced to month end), a preview with charts before saving, per-series choice of new
  or existing investment, and a downloadable example file (issue #271).

- Prefill the first market chapter with a labelled example draft built from the starter
  library, and add **Reset chapter** to discard any chapter draft through an immutable
  reset marker. Publications and pinned child chapters are unaffected (issue #271).

- Add a parent "Starter library" action that adds the 12 importable synthetic starter
  series to a household as catalog investments with validated immutable histories.
  It is idempotent, preserves existing items, and makes child visibility an explicit
  choice (issue #271).

- Ship a deterministic synthetic starter Market Data Pack (13 monthly series over
  300 synthetic months) as package data, with `the_long_game.starter_data`,
  `scripts/generate_starter_pack.py` and a `--check` mode. All values are invented;
  no third-party market data is redistributed (issue #271).

- Add operator-only verification and idempotent repair of a consumed demo
  investment's display projection from its immutable PostgreSQL result. Repair
  never reposts the ledger operation and requires a stopped app and intact
  baseline. Add independent ledger-amount/balance checks and process-interruption
  qualification for projection write failures (issues #233/#263).

- Reconcile parent approval and the demo portfolio with the core's applied
  contribution and completed allocation, including matching, fees and exact
  purchased units. Persist HA demo state by default, write it atomically and
  retain transaction references to prevent duplicate projection updates. Invalid
  state no longer silently resets balances (issues #263 and #233).

Status snapshot, 2026-10-18: the HA follow-up entries below are included in the
follow-up application/test update on PR #264. See
[Project status](docs/project-status.md) for qualification evidence and limits.

- Published HA follow-up fixes in `6721782`; all four CI jobs passed. Recorded
  synthetic approval/restart/confirmation/replay evidence and demo findings,
  subsequently fixed and retested in `db1092a`; release remains unqualified.

- Bound investment eligibility and pending plans to an opened, explicitly
  unlocked future market chapter. Revalidate its override and progress on every
  check; pausing, clearing access, completion or returning to another lesson
  invalidates its pending plans without losing pinned publication versions (issue #263).
- Aligned child market-story labels with each published chapter's scaled
  timeline, including reveal/reflection. Legacy future labels now continue
  after observed months instead of restarting at month 1 (issue #263).
- Fixed missing CSRF fields on chapter administration and other plain-rendered
  pages so unlock/pause and sign-out forms can submit with server protection
  intact. Added browser-equivalent form and session-revocation regressions (issue #263).
- Aligned parent chapter progress and pause/unlock controls with the child's
  canonical curriculum and runtime. All registered chapters now appear with
  matching access states; legacy period completions are not reassigned (issue #263).
- Fixed HA ingress asset paths so the parent market workspace JavaScript loads
  under the app's ingress prefix; external and already-prefixed asset URLs are
  preserved (issue #263).
- Gated tag-triggered HA image publication on the complete reusable quality
  workflow. Added the market suite to PostgreSQL CI, populated pre-market
  migration/repeated-startup qualification, no-skips integration evidence, and
  three-process market approval tests for republishing, pausing and advancement.
  Added a candidate/physical-HA acceptance checklist (issue #263).
- Fixed parent-market review findings: paused/unavailable chapter contexts now
  deny investment choices, pending plans revalidate the current chapter before
  confirmation, and gapped histories retain jump findings for observed intervals
  so valid subwindows can be reviewed without overriding missing coverage.
- Added a real parent market workspace (issue #263): searchable investments,
  validated monthly CSV histories, interactive shared chapter dates, Decimal
  KPIs/charts, explicit investment toggles, quality review, saved drafts and
  reviewed immutable publications. Production wiring uses PostgreSQL; new
  migration `0015_chapter_market_workspace` stores histories, publications,
  child version bindings and pending-plan binding tokens. Started chapters
  retain their versions, child plans recheck availability, and educational
  playback preserves source returns without changing purchase prices or money
  clocks. Live PostgreSQL qualification remains environment gated.
- Simplified the parent investment catalog to show each name once, with a
  separate child-facing alias only when different. Added full-width grouped
  editors, optional details, a saved child-view card, and preservation of
  currency, minimum stage, and catalog order when saving.
- Connected the parent chapter workflow to period and dataset setup, added
  prefilled period editors that preserve chapter identity and paused state,
  clarified the limits of in-app chapter and time-series editing, and removed
  the duplicate chapter-administration placeholder.
- Refreshed the A11 parent workspace with grouped responsive navigation, a
  calmer visual hierarchy, prominent financial summaries, expandable money
  actions and permission details, and keyboard-accessible mobile navigation.
- Clarified the self-service acceptance criteria: install once, then manage
  household setup, market-series imports, chapter publishing, and routine
  recovery through the application UI without Python coding or SQL. Separated
  the completed Issue #229 status/importer scope from the remaining in-app data
  management and chapter authoring work.
- Added a parent-only CSV upload for monthly market series. The importer validates
  bounded UTF-8 date/value files and provenance, writes a new immutable checksummed
  pack version, and exposes optional percentage-return scenarios to the existing
  chapter period selector with version-pinned IDs.
- Added a parent-side child preview for imported chapter periods. It shows only
  the pre-decision up/down/flat sequence, omits dates and returns, and displays
  the pinned dataset version in the selector and saved assignment summary.
- Added an ephemeral-CI-only synthetic PostgreSQL backup/restore qualification
  that migrates and seeds a fixed source database, creates chapter, ledger, and
  pending-approval state, restores a `pg_dump` into a separate database, and
  verifies credentials and application state through a fresh process and full
  dependency graph. Private-release reporting now distinguishes automated
  synthetic restore success from unvalidated physical family recovery.
- Replaced same-interpreter pending-plan restart evidence with an authorized
  PostgreSQL integration test that constructs three complete built-in Starlette
  dependency graphs in separate processes, re-authenticates after each restart,
  completes the durable parent-approval flow, and verifies exactly-once ledger
  persistence, plan consumption, pending-approval removal, and replay refusal.
- Added a fixed, validation-mode-only synthetic release household with a
  credential-safe deterministic reset command, three period-driven chapters,
  three child-eligible investments, zero-progress and zero-pending-plan reset
  state, collision refusal, cross-household isolation coverage, and a physical
  Home Assistant Yellow validation procedure.
- Fixed the durable child investment approval lifecycle so parent decisions
  survive application restarts, child confirmation reflects authoritative
  pending-plan state, completed plans are consumed exactly once, stale pending
  plans are superseded safely, and approval HTTP responses report conflicts and
  dependency failures truthfully. Added full transport and opt-in PostgreSQL
  restart-boundary regression coverage.
- Added integration safety review tests covering the full validate → adapt →
  flow-service pipeline, hidden notes absence from pipeline output, valid and
  invalid draft classification, LOCKED-draft adaptation with caller-supplied
  progress state, distinct chapter IDs, and no PII or real financial identifiers
  across all authoring drafts (Milestone 10, issue #156).
- Added a minimal adapter from validated synthetic drafts to existing
  ``GuidedChildChapterFixture`` chapter flow contracts with validation-gated
  conversion, hidden content separation, immutable input/result types, and
  focused tests for field mapping, rejection, and type safety
  (Milestone 10, issue #155).
- Added a synthetic chapter authoring fixture set with allowed, locked,
  hidden-content-attempt, non-synthetic-id, and unsafe-content draft examples
  for deterministic testing without real investing advice or family data
  (Milestone 10, issue #154).
- Added synthetic chapter draft validation with chapter ID prefix enforcement,
  hidden content leakage rejection, unsafe real-content pattern detection,
  immutable outcome/reason result types, and focused tests
  (Milestone 10, issue #157).
- Added a minimal synthetic chapter authoring contract with immutable
  ``SyntheticChapterDraft`` and ``SyntheticChapterDraftView`` value objects,
  explicit ``hidden_notes`` / ``safe_summary`` field separation, a
  ``build_draft_view`` projection that strips internal notes from child-safe
  views, and focused tests for deterministic structure, safe placeholders, and
  no hidden-content leakage (Milestone 10, issue #158).
- Added a framework-neutral A12 child-mode UI foundation with standards-based
  server-rendered HTML, simple ages-6-8 activity cards, guided preview and
  confirmation pages, process-scoped interruption recovery, shared-tablet
  privacy reminders, and database-free accessibility and no-secret-leakage
  coverage over the existing child session, read-model, and investment
  boundaries.
- Added A14 release-hardening coverage with a privacy-safe validation runbook,
  current browser-surface threat-model tests, release-doc consistency checks,
  and explicit Yellow/aarch64, ingress/TLS, packaging, backup, upgrade,
  rollback, and uninstall guidance without overstating unsupported deployment
  paths.
- Added a framework-neutral A13 progressive capability-presentation layer with
  age-profile defaults for 9-20+, capability-driven summaries, parent-cap and
  matching-ended visibility, ownership-transition placeholders, and
  database-free shell and contract coverage without granting authority from age
  alone.
- Added a framework-neutral A11 parent-mode UI foundation with standards-based
  server-rendered HTML, parent-only routes, login/logout and revocation UX,
  privacy-safe household, limits, and approvals surfaces, and database-free
  accessibility and no-secret-storage coverage without selecting a backend web
  framework yet.
- Added a versioned A10 Home Assistant synchronization boundary with
  privacy-safe compatibility and reconciliation status, parent-only automation
  policy, deterministic runtime listener cleanup, and database-free application
  and custom-integration coverage without making Home Assistant authoritative.
- Added a framework-neutral A9 child investment boundary with authoritative
  preview recomputation, approval-linked submission, scoped idempotency
  outcomes, atomic repository delegation, and database-free retry/conflict and
  no-failed-refresh coverage.
- Added a child-scoped A8 read-model boundary with session-derived participant
  scope, child-facing capability presentation, parent-limit and portfolio-view
  classifications, explicit privacy/shared-screen guidance, stale-cache reuse,
  and database-free cross-child and no-future-outcome coverage.
- Added a versioned framework-neutral parent-administration command boundary
  with parent-only capability-cap and explicit capability-state commands,
  trusted pending-approval orchestration, recovery decisions, process-owned
  replay/conflict handling, and database-free A7 coverage without editing
  immutable ledger history.
- Added a versioned framework-neutral application read-model foundation with
  repository-delegating assembly interfaces, safe parent and child projection
  envelopes, freshness and cache classifications, stale-cache fallback, and
  cross-scope child omission without duplicating SQL or financial formulas.
- Added a deny-by-default application authorization and capability policy with
  central household and participant scope checks, role and capability
  enforcement, parent approval gates, auditable decision contracts, cross-child
  IDOR coverage, and an explicit age-18 parent-matching eligibility boundary.
- Added a framework-neutral application authentication and in-memory secure
  session foundation with pluggable credential authentication, trusted Home
  Assistant ingress evaluation, expiry, rotation, revocation, recovery
  lockout, brute-force throttling, and database-free fixation and trust-boundary
  coverage.
- Added additive PostgreSQL household/profile persistence for application
  identity, child scope, role, and capability state with exact
  `HouseholdPolicyInput` reconstruction, migration `0003_application_profiles`,
  and synthetic repository round-trip and rollback coverage.
- Updated the README to reflect the completed deterministic financial and
  ledger core, implemented PostgreSQL and Home Assistant foundations, the still
  pending parent/child application, and exact Windows setup and quality-check
  commands.
- Enforced the existing 90% combined coverage baseline through shared coverage
  configuration for local and CI test runs.
- Added the import-safe `the_long_game_app` foundation with immutable version,
  configuration, lifecycle, health, and safe error contracts plus database-free
  architecture and wheel-isolation tests.
- Added ADR-0006 selecting a Home Assistant-centred hybrid parent and child
  application architecture with role, trust, privacy, lifecycle, deployment,
  age 6-20+ capability progression, and sequenced implementation boundaries.
- Added privacy-safe Home Assistant diagnostics, deterministic integration and
  project-wheel packaging, database-free end-to-end lifecycle coverage, and an
  honest Home Assistant Yellow/aarch64 installation and validation guide.
- Added privacy-safe Home Assistant readiness and summary-availability entities
  backed only by immutable no-poll coordinator state and deterministic lifecycle cleanup.
- Added validated Home Assistant investment and reversal actions with executor-isolated
  domain delegation, atomic PostgreSQL persistence, and credential-safe failures.
- Added a typed Home Assistant config-entry runtime and no-poll coordinator with
  executor-isolated PostgreSQL readiness, retry handling, and exact unload cleanup.
- Fixed PostgreSQL integration target verification for GitHub Actions service-container
  addresses and constrained packaged Alembic revision identifiers to its default
  version-table width while preserving fail-closed safety checks.
- Added a credential-safe singleton Home Assistant configuration flow with
  canonical PostgreSQL URL validation and deterministic reconfiguration reloads.
- Added the minimal versioned Home Assistant custom-integration foundation with
  a stable domain, isolated asynchronous lifecycle boundaries, and database-free
  metadata and import-safety tests.
- Added an explicitly authorized PostgreSQL integration workflow with live
  destination, migration, and schema verification, isolated CI coverage, and a
  no-shell backup and restore-verification helper and runbook.
- Added deterministic idempotent PostgreSQL ledger and investment-ledger writes
  with database-arbitrated concurrency conflicts and uncertain-commit resolution.
- Added atomic PostgreSQL persistence and exact no-replay reconstruction for
  completed investment results with their corresponding balanced ledger transactions.
- Added atomic PostgreSQL ledger transaction persistence with deterministic
  validated reconstruction, duplicate-reference handling, and opt-in integration tests.
- Added environment-only PostgreSQL connection configuration with credential
  redaction, caller-preserving transaction cleanup, explicit engine ownership,
  packaged migration assets, and operator-invoked Alembic helpers.
- Added the optional PostgreSQL SQLAlchemy schema and deterministic Alembic
  baseline for multi-journal Ledger data and replayable investment audits.
- Added a complete deterministic ledger orchestration boundary composing
  investment adaptation, posting, reversal, balances and reconciliation.
- Added deterministic immutable ledger reconciliation with sign-based debit and
  credit totals, exact account conservation and reversal-aware trial balances.
- Added deterministic immutable full ledger-transaction reversals with exact
  amount negation, caller-controlled references and append-only journal linkage.
- Added deterministic investment-proposal adaptation with explicit canonical
  account mappings, zero filtering and exact residual-cash balancing.
- Added deterministic immutable ledger account-balance derivation with exact
  cross-journal currency enforcement, conservation and golden coverage.
- Added deterministic immutable ledger-journal posting with canonical caller
  references, duplicate rejection and atomic balanced-transaction acceptance.
- Added immutable balanced ledger transactions with canonical accounts,
  deterministic sequencing, exact zero-sum validation and golden coverage.
- Added a deterministic complete investment transaction engine composing
  contribution, matching, fees, purchases and immutable proposed ledger data.
- Added deterministic dividend distribution with bounded withholding rates,
  optional purchase-based reinvestment and executable golden coverage.
- Added deterministic fixed-plus-percentage transaction fees with optional
  minimum and maximum bounds, immutable results and executable golden coverage.
- Added deterministic single-currency portfolio valuation with immutable
  position results, cost-basis reconciliation and executable golden coverage.
- Added deterministic fractional investment-unit sales with complete and
  partial fulfilment, conserved unit quantities and executable golden coverage.
- Added deterministic fractional investment-unit purchases with eight-decimal
  downward unit rounding, conserved residual cash and executable golden coverage.
- Added deterministic oldest-first, vesting-aware withdrawal allocation with
  proportional forfeiture, immutable per-lot results and golden-fixture coverage.
- Added a deterministic one-event annual app-cash interest policy with real-time
  clock enforcement, Decimal-only rates and executable golden-fixture coverage.
- Added deterministic real-time monthly vesting with explicit month-end and
  leap-year semantics, Decimal percentages and executable golden-fixture coverage.
- Fixed check-script repository-root resolution when invoked with arguments from
  the repository root, scripts directory, or an outside directory.
- Added a pure capped parent-matching policy with immutable allocation results,
  Decimal-only rates and executable golden-fixture coverage.
- Added a pure required-contribution policy with validated decimal rates,
  real-time clock enforcement and executable golden-fixture coverage.
- Added explicit dual-clock calculation classifications and deterministic
  Decimal log-return scaling, with ADR-0003 and synthetic worked examples.
- Added an immutable single-currency ledger with atomic posting, idempotency,
  contribution-to-match linkage, reversal invariants and ADR-0002.
- Added deterministic synthetic golden fixtures for contribution, matching,
  half-even rounding, vesting, interest, chapter compounding, idempotency and
  ledger reversal behaviour.
- Added Decimal-backed Money and InvestmentUnits value objects with explicit
  precision, rounding policies and arithmetic tests.
- Added the initial Python core package skeleton, health check, tests, quality workflow
  and Dependabot configuration.
- Added ADR-0001 documenting the private MVP architecture and proposed repository
  structure.

## Initial setup

- Added the initial Python package scaffold, quality workflow, contribution guidance,
  security policy and privacy policy.
