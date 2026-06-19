# League in a Box & Club Intelligence — Product Requirements Document

**Version:** 1.2 · **Prepared:** June 2026 · **Market:** Ireland — GAA-led, soccer to follow · **Status:** Concept & prototype

> This Markdown file is the canonical, context-managed version of the PRD. The styled
> `league-product-requirements-document.html` is the presentation artifact; keep this `.md` as the
> source of truth for terminology and decisions. Domain terms used here are defined in
> [`CONTEXT.md`](../CONTEXT.md); load-bearing decisions are recorded in [`docs/adr/`](./adr).

---

## 1. The problem

Irish grassroots sport runs on volunteers, spreadsheets, and WhatsApp — and the tools built to fix
that have settled around serving the club's membership and payments, not the people running the
league itself or the coaches trying to develop players over years.

Every week, hundreds of amateur GAA and soccer leagues across Ireland are organised by a volunteer
manually maintaining Standings, chasing Captains for results over text, and recalculating the table by
hand. At the same time, clubs developing young players from U8 through to adult keep almost no
structured record of a Player's journey — Appearances, position, involvement, development — despite
this being exactly the information that helps a Club retain players and identify who's ready for
representative trials.

This product leads with GAA (see [ADR-0005](./adr/0005-gaa-first-sequencing.md)). The grassroots GAA
league space is less saturated than soccer, the GAA's county-board structure is a ready-made
distribution wedge with no soccer equivalent, and the GAA's goals-and-points Scoring Format is the
differentiated feature. The platform stays sport-agnostic and soccer is a fast-follow.

Existing platforms (ClubZap, Pitchero, ClubSpot, LeagueRepublic, SportLoMo) are built around club
membership, payments, registration, and communications — and skew toward soccer. Standings are a
secondary feature, and long-term player development data isn't meaningfully addressed by any of them at
the grassroots level.

## 2. Two surfaces, two buyers

One platform, two priced surfaces sold to two different people (see
[ADR-0001](./adr/0001-one-platform-two-surfaces.md)). They solve different problems and have different
willingness to pay, but share one data model — the link is the Team and the Match.

| | **League in a Box** — the funnel | **Club Intelligence** — the business |
|---|---|---|
| **Sold to** | The Organiser running a League (county secretary, 5-a-side coordinator, SFAI division coordinator) | Club management (chairperson, underage development officer, committee) |
| **Job to be done** | Replace the spreadsheet with live, shareable Standings and a zero-friction Result submission flow for Captains | Give the Club a structured, multi-season record of every Player's Match history, involvement, and development |
| **Strategic role** | Free or near-free by design. Its value is distribution — every Club in a League sees it weekly, feeding the second surface | Clubs have budget (membership fees, sponsorship, lotto) and a real reason to pay monthly. This is the revenue case |

## 3. Market size

The addressable market is the Club, not the county. There are far more Clubs than counties, and Clubs
are the entity with both the data need and the budget.

| Segment | Size | Source basis |
|---|---|---|
| GAA clubs, Republic of Ireland | ~2,200 | 32 counties, GAA-affiliated |
| Soccer grassroots clubs, FAI-affiliated | ~1,180 | 78 affiliated leagues |
| **Combined addressable clubs** | **~3,380** | GAA + soccer, Republic only |
| Organised leagues above club level | ~110+ | 32 SFAI + 78 FAI leagues + county boards |

### What this means for revenue

| Scenario | Penetration | Blended price | Monthly revenue |
|---|---|---|---|
| Early traction, one or two counties | 5% | €25/mo | ~€4,200 |
| Established, word-of-mouth across counties | 10% | €25/mo | ~€8,500 |

> **Note on the blended price.** €25/mo is a planning placeholder between the Club Intelligence Standard
> (€19) and Plus (€39) tiers; actual blended ARPU depends on the Standard/Plus mix. Treat these as
> illustrative.

**Why penetration matters more than price.** Raising the price on a small base barely moves the
outcome. Moving from 5% to 15% penetration within a county changes the business entirely. The GAA's own
structure — county boards sitting above individual Clubs — is a natural distribution wedge: one
relationship with a county board can open warm introductions to every affiliated Club underneath it.

## 4. Competitive landscape

| Platform | Built for | Where the gap is |
|---|---|---|
| **ClubZap / ClubSpot** | Club membership, payments, comms | Standings and player history are secondary features, not the focus |
| **Pitchero** | Club websites, basic attendance/scorer tracking | No structured multi-season player development record |
| **LeagueRepublic** | Fixture and competition management | Reviewers describe it as overwhelming; desktop-era interface |
| **SportLoMo** | Governing-body registration (SFAI, GAA Foireann) | Built for compliance and registration, not day-to-day league or development use |

None treat the Organiser as a first-class user, and none offer a Coach-facing, multi-season player
development view designed around retention rather than ranking. The competitive set also skews toward
soccer and governing-body registration; grassroots GAA league tooling is comparatively underserved,
which is why we lead there.

## 5. League in a Box — functional spec

### Core features (Phase 1)

- **League creation** — Divisions, Teams, Season dates, and a per-League Scoring Format. **GAA
  goals-and-points (e.g. `2-08`) and GAA Standings points (typically 2/1/0) ship in Phase 1** as the
  lead format; single-integer soccer scores with 3/1/0 are supported as the fast-follow.
- **Public League page** — live Standings, fixtures, and results, no login required to view.
- **Result submission** — a Captain opens a single-use **Result Link** (no account) to confirm a score
  (see [ADR-0004](./adr/0004-accountless-captain-submission.md)).
- **Automatic recalculation** — Standings recompute the moment a Result is confirmed.
- **Trust-based submission with Organiser override** — for the pilot, a single Captain submits and the
  Organiser can edit any Result. Opponent confirmation is deferred.
- **Sponsor banner slot** on the public page (Organiser can sell locally).

### Phase 2 additions

- Automated fixture scheduling across shared pitches.
- Referee / umpire assignment.
- Optional opponent-confirmation window and dispute handling.
- Multi-season public archive (a long-term SEO and trust asset).

## 6. Club Intelligence — functional spec

A Match is the shared spine; the club surface enriches the same record the league surface scored (see
[ADR-0002](./adr/0002-match-as-shared-spine.md)).

### Per Match

- Opposition, venue (home/away), competition/Division, pitch surface.
- Formation / team structure.
- Final score, scorers, assists, and timing of scores (which quarter/half).
- Coach's free-text **Match Report**.

### Per Player

- **Appearance** history — minutes, position, season-by-season involvement trend.
- Scoring and assist record over time.
- Manager's notes tied to specific Matches.
- A Player's-own-journey view, intended to be shareable with the Player's Guardian (framed as the
  Player's journey, not a ranking against teammates).

This is the layer the prototype demonstrates: a Squad roster, an individual Player profile with a
season-by-season involvement trend, and a Match log. See
[`prototype-walkthrough.md`](./prototype-walkthrough.md).

## 7. Data & child-safety approach

This is the most important section for any partner. Player development data on minors is high-value and
high-risk, and the product is deliberately structured to keep tiers separate. The **Club is the data
controller** and the **platform is the data processor** (see
[ADR-0003](./adr/0003-club-is-controller-platform-is-processor.md)).

> **Context.** Ireland's digital age of consent is 16; processing a child's personal data generally
> requires parental consent below that age, and the Irish Data Protection Commission explicitly flags
> sports clubs as organisations likely to be accessed by children, triggering extra obligations. The DPC
> has shown real enforcement appetite against far larger organisations over exactly this category of risk.

### Feature tiers, by risk

| Tier | Feature | Rule |
|---|---|---|
| **Low risk** | Team & Match-level record (formation, scorers, results, surface, Division, Match Report) | No individual ranking. Safe to build first, any age group. |
| **Build with care** | Individual Appearance history (minutes, position, involvement trend) | Framed as the Player's own journey, not a comparison. Visible to Coaches and, ideally, the Player's Guardian. Relies on the Club's lawful basis. |
| **Consent-gated** | Ratings & Trial Recommendations | Highest value, highest risk. Internal to the Club only, never public or visible to other Clubs, and disabled per Player until guardian **Consent** is on file. Requires a formal data-protection review before broad rollout. |

Build sequencing follows this risk order: ship the low-risk tier broadly first; ship the care tier for
adult and consenting youth Players; hold the consent-gated tier behind a legal review and a single pilot
Club before wider release.

**Consent model.** Per-Player guardian Consent (not a single blanket club-registration checkbox) unlocks
the consent-gated tier for that Player; it is explicit and revocable. Retention and right-to-erasure: a
Player record is retained while the Club subscription is active subject to a configurable retention
policy; on Club churn, data is exported then deleted after a grace period. Details to be confirmed in the
data-protection review.

## 8. Pricing & revenue model

| Product | Tier | Price | Includes |
|---|---|---|---|
| League in a Box | Free | €0 | 1 League, up to 10 Teams, live Standings & fixtures |
| League in a Box | Organiser | €15/mo | Unlimited Divisions, custom domain, sponsor slot |
| Club Intelligence | Standard | €19/mo | Full Match & Player history, involvement tracking, notes |
| Club Intelligence | Plus | €39/mo | + Consent-gated Ratings, Trial Recommendation flags, data export |

League pricing is intentionally low — its function is adoption and distribution, not margin. Club
Intelligence carries the revenue case, priced against what a Club committee can reasonably approve as a
recurring software line, comparable to existing club software spend.

## 9. Roadmap

1. **Build & pilot League in a Box (GAA-first).** Launch with one or two real GAA Leagues, shipping the
   goals-and-points Scoring Format from day one (see [ADR-0005](./adr/0005-gaa-first-sequencing.md)).
   Validate the Captain-submission flow and table-sharing behaviour in practice. Soccer follows once the
   GAA flow is proven.
2. **Layer in Club Intelligence, low-risk tier only.** Team and Match-level record, offered to Clubs
   already inside a piloted League. No individual Ratings yet.
3. **Data-protection review.** Formal review of Consent flows, retention, and deletion policy ahead of
   enabling individual Ratings and Trial Recommendations.
4. **County-board-led expansion.** Approach county boards directly as a distribution wedge into their
   affiliated Club base, rather than selling Club by Club. No official GAA/FAI affiliation is assumed;
   growth is bottoms-up, with county boards as a warm-introduction channel.

## 10. Open questions for partners

- Does a partner have existing relationships with GAA county boards or FAI-affiliated leagues that could
  shortcut the pilot stage?
- Confirm the legal structure for guardian Consent capture in the data-protection review (the working
  decision is per-Player, per-feature for the consent-gated tier — see ADR-0003).
- Should Club Intelligence integrate with existing club registration systems (e.g. Foireann), or remain
  fully standalone?
- Is there appetite to pilot the consent-gated Ratings tier with a single trusted Club before broader
  rollout?
