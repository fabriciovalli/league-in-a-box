# The Club is the data controller; the platform is the processor; ratings need per-player guardian consent

The Club is the data controller for its Players' personal data and the platform is the data processor,
governed by a data processing agreement. The consent-gated tier (Rating, Trial Recommendation) is
disabled per Player until guardian Consent is on file; ordinary Appearance history relies on the Club's
own lawful basis. We chose per-Player consent over a single blanket club-registration consent because
Ireland's digital age of consent is 16, the DPC flags sports clubs as child-accessed services, and the
highest-risk feature (rating minors) warrants explicit, revocable, per-child permission rather than a
buried registration checkbox. This is load-bearing and hard to reverse: it shapes the data model, the
consent UX, the terms of service, and the build sequencing (ship low-risk team/match data first, hold
the consent-gated tier behind a formal data-protection review and a single pilot club).
