# League in a Box

Two connected surfaces on one platform for Irish grassroots sport (GAA and soccer):
a free-to-low-cost live-league tool for the volunteer running a league, and a paid,
multi-season player-history layer for club management. The same underlying record of
who played whom feeds both.

## Language

### Shared spine

**Match**:
A single game between two Teams. It moves through a lifecycle — scheduled, result-submitted,
confirmed, enriched — rather than existing as separate records. The Match is the one fact both
surfaces consume: the league surface reads its score, the club surface reads its lineup and detail.
_Avoid_: game, fixture (a Match in its scheduled state), result (the score that confirms a Match)

**Season**:
A bounded competitive period (e.g. 2026) that scopes Standings, Appearances, and history. The
unit across which a Player's involvement trend is measured.
_Avoid_: campaign, year

### League surface

**Organiser**:
The volunteer who runs a League — a county secretary, a 5-a-side coordinator, an SFAI division
coordinator. The buyer of the league surface.
_Avoid_: admin, league owner

**League**:
A competition run by an Organiser, containing one or more Divisions of Teams over a Season.
The top-level aggregate of the league surface.
_Avoid_: competition (a League may contain several), tournament

**Division**:
A single table of Teams within a League. Teams play each other and are ranked by Standings.
_Avoid_: group, section, tier

**Team**:
The entity that appears in a Division's table. Usually fielded by one Squad of one Club, but a
Squad may field more than one Team (an A and a B side). The join between the league surface and
the club surface.
_Avoid_: side, club (a Club is the parent organisation, not the table row)

**Captain**:
The person who submits a Match's score on behalf of a Team. Acts without an account, via a Result Link.
_Avoid_: manager (a club-surface role), team admin

**Result Link**:
A single-use, signed URL scoped to one Match, dropped into a team chat. Lets a Captain submit a
score with no app, account, or password. Expires after the Match.
_Avoid_: invite, magic link, token

**Result**:
The score a Captain submits, which transitions a Match from scheduled to result-submitted and
triggers recalculation of Standings.
_Avoid_: outcome, scoreline

**Standings**:
The ranked table of Teams in a Division, recomputed automatically when a Result is confirmed.
Ranking obeys the Division's Scoring Format.
_Avoid_: league table, rankings, ladder

**Scoring Format**:
The per-League configuration of how a score is expressed (a single integer for soccer, goals-and-points
like `2-08` for GAA) and how Standings points are awarded (e.g. 3/1/0 or 2/1/0 for win/draw/loss).
_Avoid_: rules, points system

### Club surface

**Club**:
The parent sporting organisation (e.g. St. Mochta's GAA). Owns Squads and Players, holds the
subscription, and is the data controller for its Players' personal data. Top-level aggregate of the
club surface and the unit of monetisation.
_Avoid_: organisation, account, customer

**Squad**:
An age-grade and gender cohort within a Club (e.g. U14 Boys, Adult). The unit Players belong to and
the level at which a Coach works. Fields one or more Teams.
_Avoid_: team (a Team is the league-table row), age group, panel

**Player**:
An individual tracked across Seasons within a Squad. For underage Squads, a Player has a Guardian and
is subject to the consent rules.
_Avoid_: member, athlete

**Guardian**:
The parent or legal guardian of an underage Player. The party whose consent unlocks the consent-gated
tier, and the intended recipient of a Player's own journey view.
_Avoid_: parent (a Guardian need not be a parent), next of kin

**Coach**:
The club-surface user who records Matches and views player history for a Squad. Holds the
manager's-notes and (where consented) the Rating.
_Avoid_: manager (used loosely in drafts; prefer Coach), trainer

**Appearance**:
A Player's participation in a Match — minutes, position, and involvement. Aggregated into the
season-by-season involvement trend.
_Avoid_: cap, selection

**Match Report**:
A Coach's free-text account of a Match, attached to the Match record.
_Avoid_: notes (reserved for player-level Manager's Notes), write-up

**Rating**:
A Coach's internal assessment of a Player. Consent-gated: never public, never visible to other Clubs,
and disabled until guardian Consent is on file.
_Avoid_: score (reserved for Match scores), grade

**Trial Recommendation**:
An internal flag that a Player is ready for a representative or county trial. Consent-gated, same rules
as a Rating.
_Avoid_: scouting note, nomination

**Consent**:
A per-Player record of guardian permission that unlocks the consent-gated tier (Rating, Trial
Recommendation) for that Player. Distinct from the lawful basis the Club relies on for ordinary
Appearance history.
_Avoid_: opt-in, permission, agreement
