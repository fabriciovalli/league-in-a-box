# League in a Box - UX Architecture Spec (Refined)

## 1) Product UX Thesis

The product UX is a **two-surface system on one shared match spine**:

- **Surface A (League in a Box):** a high-speed operations tool for volunteer organisers.
- **Surface B (Club Intelligence):** a trust-forward intelligence tool for club committees and coaches.

Design language: **Touchline Command**. The UI should feel like matchday control software, not generic SaaS:
scoreboard typography, strong contrast, compact action density, and explicit trust states.

This spec replaces "reference-only tabbed mockups" with a full site and app structure.

---

## 2) Full Site Structure

## 2.1 Public website (marketing + conversion)

- `/` - Landing
- `/product/league-in-a-box`
- `/product/club-intelligence`
- `/pricing`
- `/faq`
- `/pilot-program`
- `/resources/case-studies`
- `/login`

## 2.2 Authenticated app

- `/app/organiser/home`
- `/app/organiser/league-control`
- `/app/organiser/results`
- `/app/organiser/sponsors`
- `/app/organiser/reports`
- `/app/club/:clubId/coach-hub`
- `/app/club/:clubId/squad/:squadId`
- `/app/club/:clubId/player/:playerId`
- `/app/club/:clubId/compliance/consent`
- `/league/:slug` (public standings artifact, no auth)
- `/r/:token` (captain match submission link, no auth, token scoped)

## 2.3 Core journeys

1. **League acquisition**
   - `/` -> `/pricing` -> `/pilot-program` -> `/app/organiser/home`
2. **Club upsell**
   - `/league/:slug` -> `/product/club-intelligence` -> sales/demo -> `/app/club/:clubId/coach-hub`
3. **Consent unlock**
   - `/app/club/:clubId/player/:playerId` (locked panel) -> `/app/club/:clubId/compliance/consent` -> unlock Plus modules

---

## 3) Navigation System Requirements (No Pill Tabs)

## 3.1 Marketing navigation

**Desktop**
- Top rail: Product, Solutions, Pricing, Case Studies, FAQ, Pilot Program.
- Right side fixed actions: `Log in` and `Start a league`.
- Product uses mega menu for two surfaces (League in a Box, Club Intelligence).

**Mobile**
- Drawer navigation with persistent bottom CTA (`Start a league`).
- FAQ and Pricing must be first-order links (not hidden in nested accordion).

## 3.2 App navigation

**Desktop**
- Left rail grouped by workflow: Command, Fixtures, Results, Sponsors, Reports, Club Intelligence, Compliance.
- Top command bar for context switching: league, season, round/matchday, global search, quick actions.

**Mobile**
- Bottom nav (max 4 fixed destinations) + `More` sheet.
- High-priority actions (`Resolve dispute`, `Submit result`, `Request consent`) must remain one tap away.

## 3.3 Global interaction pattern

- Add **Touchline Command Palette** (shortcut `/`) to jump routes and trigger actions.
- Must support command intents:
  - `create fixture`
  - `resolve result`
  - `send captain reminder`
  - `open consent vault`
  - `export county pack`

---

## 4) Marketing Page Requirements (Prescriptive)

## 4.1 Landing (`/`)

**Primary goal:** move organisers and club stakeholders to trial/demo intent.

**Must include**
- Split value proposition (organiser vs club committee).
- Proof metrics (submission speed, league setup time, retention indicator).
- Trust strip (consent model, data roles, no-affiliation clarity).
- CTA pair: `Start a league` + `Book club demo`.

**Non-negotiables**
- GAA-first messaging with soccer fast-follow.
- Above-the-fold includes both surfaces (avoid single-product framing).

## 4.2 Pricing (`/pricing`)

**Primary goal:** reduce pricing ambiguity; drive plan evaluation.

**Must include**
- Four-tier matrix (free, organiser, club standard, club plus).
- Explicit "who buys this" labels.
- Feature gates clearly tied to trust/compliance (ratings in plus + consent gate).

**Non-negotiables**
- Show funnel/revenue split (free league product feeds paid club product).
- Add planning-note clarity for blended ARPU assumptions.

## 4.3 FAQ (`/faq`)

**Primary goal:** remove trust blockers.

**Must include**
- Affiliation stance.
- No-login result flow explanation.
- Data controller/processor explanation.
- Guardian consent logic.
- GAA-first rollout rationale.

---

## 5) App Page Requirements (Prescriptive)

Each page below defines route, primary user, required modules, hard behavior rules, and acceptance criteria.

## 5.1 Organiser Home

- **Route:** `/app/organiser/home`
- **Primary user:** organiser / deputy organiser

**Required modules**
- KPI strip: pending results, disputes, pitch conflicts, sponsor variance.
- Priority feed of urgent events.
- Pending result queue with direct action.
- Deadline widget for county exports and fixtures lock cutoff.

**Hard behavior rules**
- Auto-refresh data every 30 seconds.
- Every card links to actionable destination; no dead-end widgets.
- Result queue sorted by urgency by default.

**Acceptance criteria**
- 90% of unresolved result tasks visible without scroll on desktop.
- User can resolve at least one result from this page in <= 3 clicks.

## 5.2 League Control

- **Route:** `/app/organiser/league-control`
- **Primary user:** organiser

**Required modules**
- Drag/drop fixture scheduler.
- Pitch occupancy map.
- Referee assignment panel.
- Conflict detector with suggested alternatives.

**Hard behavior rules**
- Detect overlap in <= 100ms after drag.
- Save blocked until critical overlaps are resolved or explicitly overridden with reason.
- Every schedule mutation writes immutable audit metadata.

**Acceptance criteria**
- Conflict resolution median time <= 3 minutes.
- 100% of fixture edits traceable in audit history.

## 5.3 Result Inbox

- **Route:** `/app/organiser/results`
- **Primary user:** organiser, dispute resolver

**Required modules**
- Submission timeline per match.
- Side-by-side captain/opponent score comparison.
- State machine controls: submitted, awaiting confirmation, disputed, resolved, locked.
- In-context communication shortcuts.

**Hard behavior rules**
- Dispute resolution requires a mandatory reason note.
- Locked results cannot be edited without privileged override.

**Acceptance criteria**
- Median dispute turnaround <= 4 hours.
- Zero unresolved disputes older than configured SLA without alert.

## 5.4 Captain Result Link

- **Route:** `/r/:token`
- **Primary user:** captain (no account)

**Required modules**
- Match context header.
- Goals and points input per team.
- Computed total preview (goals x 3 + points).
- Confirmation and receipt state.

**Hard behavior rules**
- Token scoped to one match only.
- Expired token shows clear fallback contact path.
- Duplicate submit is idempotent.

**Acceptance criteria**
- Median completion <= 30 seconds.
- Error recovery without data loss after connection interruption.

## 5.5 Public League Page

- **Route:** `/league/:slug`
- **Primary user:** public viewers, clubs, sponsors

**Required modules**
- Standings table.
- Upcoming fixtures and latest results.
- Last-updated timestamp.
- Sponsor placement slot and click tracking.
- Share metadata support.

**Hard behavior rules**
- No auth barriers.
- Cached aggressively with near-real-time invalidation after result confirmation.

**Acceptance criteria**
- LCP <= 2.5 seconds on median mobile.
- Table updates visible within one refresh cycle of confirmed result.

## 5.6 Coach Hub

- **Route:** `/app/club/:clubId/coach-hub`
- **Primary user:** coach, youth officer

**Required modules**
- Squad status board.
- Retention radar (minutes + attendance trend).
- Match prep panel with contextual notes.
- Action queue of players needing intervention.

**Hard behavior rules**
- Surface changes in engagement week-over-week.
- Avoid comparative language that implies public ranking of minors.

**Acceptance criteria**
- At-risk players are always visible in top viewport.
- Coaches can add intervention notes in <= 2 interactions.

## 5.7 Squad Roster

- **Route:** `/app/club/:clubId/squad/:squadId`
- **Primary user:** coach, manager

**Required modules**
- Roster list with role/position.
- Filters: availability, injury, attendance trend.
- Quick actions: note, contact guardian, mark availability.

**Hard behavior rules**
- Search and filter state persist across navigation in same session.

**Acceptance criteria**
- Search first result <= 200ms with 300-player dataset.

## 5.8 Player Profile

- **Route:** `/app/club/:clubId/player/:playerId`
- **Primary user:** coach, committee

**Required modules**
- Multi-season journey timeline.
- Appearance and context log.
- Match reports and manager notes.
- Ratings/trial panel with lock state tied to consent.

**Hard behavior rules**
- Consent check executed on every load.
- If consent absent/revoked, ratings module never renders data values.

**Acceptance criteria**
- Profile reveals full player context (season trend + latest match context) within one screen on desktop.

## 5.9 Consent Vault

- **Route:** `/app/club/:clubId/compliance/consent`
- **Primary user:** compliance admin, club officer

**Required modules**
- Per-player feature consent matrix.
- Guardian contact and status view.
- Request/reminder workflow.
- Immutable audit timeline.

**Hard behavior rules**
- Every consent mutation records actor, timestamp, channel, and legal copy version.
- Audit entries cannot be edited from UI.

**Acceptance criteria**
- Full consent history exportable on demand.
- Revoked consent propagates lock state immediately.

## 5.10 Sponsor Studio

- **Route:** `/app/organiser/sponsors`
- **Primary user:** organiser

**Required modules**
- Slot inventory by page/league.
- Campaign builder with date and placement constraints.
- Promised vs delivered analytics.
- Report generator for sponsor renewals.

**Hard behavior rules**
- Trigger warning when delivery is projected below commitment threshold.

**Acceptance criteria**
- Weekly sponsor report export in one click.

## 5.11 Reports and Exports

- **Route:** `/app/organiser/reports`
- **Primary user:** organiser, county liaison

**Required modules**
- County-board pack export.
- Discipline and standings summary.
- Sponsor campaign summary.
- Club intelligence overview export.

**Hard behavior rules**
- Every export stamped with season and generation timestamp.
- Support scheduled and manual delivery.

**Acceptance criteria**
- Export success/failure notifications are persisted and auditable.

---

## 6) Build Priority and Dependencies

## Phase 1 (market validation)
- Landing, Pricing, Pilot Program
- Organiser Home
- Captain Result Link
- Public League Page

## Phase 2 (operations scale)
- League Control
- Result Inbox
- Sponsor Studio
- Reports and Exports

## Phase 3 (club intelligence + compliance depth)
- Coach Hub
- Squad Roster
- Player Profile
- Consent Vault

---

## 7) Deliverables

- Architecture mockup: `docs/league-uiux-mockups.html`
- This spec: `docs/league-uiux-proposal.md`
