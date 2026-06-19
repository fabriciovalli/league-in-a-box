# Captains submit results via a signed, single-use Result Link, not an account

A Captain confirms a Match score through a single-use, signed URL (the Result Link) scoped to one
Match, with no account, app, or password. We chose this deliberately: zero-friction submission is the
core wedge of the league surface, and requiring captain accounts is exactly the friction competitors
impose. The trade-off is weaker authentication and a dispute/spoofing surface — mitigated for the pilot
by trust-based single submission with an Organiser override (edit any Result), and an optional
opponent-confirmation window deferred to a later phase. Recorded because a future reader will reasonably
ask why result submission has no auth.
