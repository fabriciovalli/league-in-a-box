# Prototype walkthrough

A text spec of the interactive prototype in `league-in-a-box-prototype.html`, kept in Markdown for
context management. The prototype is a single HTML page with a view switch between two surfaces. Domain
terms are defined in [`CONTEXT.md`](../CONTEXT.md).

> Header disclaimer in the prototype: "Prototype · not affiliated with the GAA." Growth is bottoms-up;
> no official affiliation is assumed (see PRD §9).

## View switch

Two buttons toggle the two surfaces of the one platform:

- **Public league page** (default) — the League in a Box funnel surface.
- **Club view · coach login** — the Club Intelligence revenue surface.

## Surface 1 — Public league page (League in a Box)

The artifact people screenshot and share.

1. **Hero band** — League name (a GAA league, "Fingal Adult Football League"), Division ("Division 2"),
   round, and a pulsing **LIVE** pill. Subtitle: "updated automatically".
2. **Standings table** — rank, Team (with initials crest), P / W / D / L / Pts. Teams are GAA clubs
   (Skerries Harps, Naomh Maur, Fingallians, Round Towers Lusk, St. Mochta's, …). Standings use the GAA
   league points scheme (2 for a win, 1 for a draw). Top 3 rows are highlighted as qualifying positions.
   Recomputes and reorders on Result submission, flashing the changed row.
3. **What the Captain sees** — a phone mock of the Result Link flow: a single fixture
   (`Round Towers Lusk v Garristown`) entered in **GAA goals–points** format (separate goals and points
   inputs per team), a "Confirm result" button, and a success message ("Result submitted — table updated
   instantly"). No app, account, or password — the link is dropped straight into the team chat.
   - *Interactive:* confirming computes each side's total (goals × 3 + points), awards 2/1/0 standings
     points, recalculates the demo Standings, and scrolls to the reordered table.
4. **Competitor comparison** — three cards (ClubZap/Pitchero, LeagueRepublic, League in a Box)
   contrasting the Organiser-first, account-free, table-is-the-product positioning.
5. **Two surfaces, two buyers** — the funnel (League in a Box, Free €0 / Organiser €15) beside the
   revenue surface (Club Intelligence, Standard €19 / Plus €39). The platform is GAA-led; soccer is a
   fast-follow.
6. **Market panel** — addressable Clubs (~2,200 GAA, ~1,180 soccer, ~3,380 combined) and the
   penetration-driven revenue illustration.

## Surface 2 — Club view (Club Intelligence)

Labelled "CLUB PLUS · SEPARATE SUBSCRIPTION". A privacy note states it is visible to team management
only and built to flag Players who need support to stay involved — not to rank them against each other.

Header: "St. Mochta's GAA · U14 Boys" (a Club + Squad).

1. **Roster** (left) — the Squad list ("18 players"); each row shows name and position. Selectable.
   *(Prototype note: only Conor Reilly's profile is wired up; other rows reuse the same profile to show
   layout.)*
2. **Player profile** (right):
   - Header — name, position, "joined U8 (2021)", squad number, current Squad pill.
   - **Stat row** — Apps this season, Points scored, Assists, Years at the club.
   - **Involvement by season** — a sparkline of minutes played per Season; a low Season is flagged in a
     warning colour (the retention signal).
   - **Recent matches** — a Match log: date, opposition + venue, surface · Division · scoring detail,
     and a W/L score in GAA goals-and-points format (`W 2-08–1-06`).
   - **Manager's notes** — free-text tied to a Match date.
   - **Consent-gated card** (locked) — "Coach ratings & trial recommendation": internal-only Rating and
     a recommend-for-county-trial flag, never public, never visible to other Clubs, disabled until
     guardian Consent is on file. The mock shows a "Consent on file" tag for the selected Player.

## What the prototype demonstrates vs. defers

- **Demonstrates:** the funnel↔revenue split on one platform, the account-free Result Link flow, the
  GAA goals–points Scoring Format with 2/1/0 standings, live Standings recalculation, the Player journey
  framing, and the three-tier risk model with the consent-gate made visible.
- **Defers (not wired):** real auth, multi-player data, fixture scheduling, per-League Scoring Format
  configuration (soccer / alternate points schemes), opponent confirmation, and the actual Consent
  capture flow.
