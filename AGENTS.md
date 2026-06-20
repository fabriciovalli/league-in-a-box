## Agent skills

### Issue tracker

GitHub Issues in `fabriciovalli/league-in-a-box`; external PRs are not a triage surface. See `docs/agents/issue-tracker.md`.

### Triage labels

Five canonical triage roles mapped to identically named GitHub labels. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context layout: `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.

## Plan Mode

- Make the plan extremely concise. Sacrifice grammar for the sake of concision.
- At the end of each plan, give me a list of unresolved questions to answer, if any.

## Cursor Cloud specific instructions

- This is a docs + static-prototype repo only: no package manager, no build system, no
  dependencies, and no automated tests or lint config. The update script is intentionally a no-op.
- The runnable "application" is the self-contained, single-file prototype at
  `docs/league-in-a-box-prototype.html` (all CSS/JS inlined; mockups in
  `docs/league-uiux-mockups.html`, PRD in `docs/league-product-requirements-document.html`).
- To run it, serve the repo root with any static server and open the file, e.g.
  `python3 -m http.server 8000` then visit `http://localhost:8000/docs/league-in-a-box-prototype.html`.
  Opening via `file://` also works but a server avoids browser quirks.
- Core interactive demo: the "What the Captain sees" Result flow (enter GAA goals-points, click
  "Confirm result") recalculates and reorders the live Standings table; the top toggle switches
  between the public league surface and the Club Intelligence surface.
- The prototype deliberately defers/fakes most logic (see `docs/prototype-walkthrough.md`); only
  Conor Reilly's club profile is wired, and standings sort can show display quirks — these are
  prototype limitations, not bugs to fix.