# Match is the shared spine, with a lifecycle instead of separate Fixture/Result/Match records

A Match is one entity that moves through states — scheduled → result-submitted → confirmed → enriched —
rather than separate Fixture, Result, and Match tables. The league surface transitions it (a Captain
submits a Result) and the club surface enriches it (a Coach adds lineup, Appearances, Match Report).
We chose a single lifecycle entity because the two surfaces are reading and writing the same game from
different angles; modelling them as separate records would force brittle reconciliation between a
"fixture" and the "match" that is the same event. The trade-off is that one entity carries fields
relevant to different users at different stages; we accept that over duplicate-and-sync.
