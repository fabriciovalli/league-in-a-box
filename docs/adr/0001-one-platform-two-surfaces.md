# One platform, two priced surfaces

League in a Box (free funnel) and Club Intelligence (paid revenue) are built as two surfaces of a
single platform sharing one data model, not two separate products. We chose this because the whole
strategy depends on the league surface feeding the club surface: a Team in a League is fielded by a
Squad of a Club, and the same Match record carries both the score the league reads and the detail the
club reads. Two codebases would duplicate that spine and break the funnel. The trade-off is that the
free surface and the paid surface share blast radius and a release cadence; we accept that to keep the
Club ↔ League link first-class.
