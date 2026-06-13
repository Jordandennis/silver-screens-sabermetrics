---
date: 2026-06-12
status: draft — working definition, subject to revision once data is available
---

# ADR 0003: Replacement Level Working Definition

## Decision (Working)

A replacement-level performer is any individual who meets either condition:

1. **Billing threshold:** Career billing history has never placed them in the top 3 billing positions of any film's cast (per TMDB cast data).
2. **Budget floor:** Their entire filmography consists of films below ~$1M production budget.

## Context

Replacement level is the intellectual core of the WAR framework. In baseball it's cleanly defined as freely available minor league talent. Film has no equivalent observable talent pipeline, so the definition must be constructed from available proxies.

## Rationale

**Why billing order:** TMDB's cast billing order reflects the studio's own assessment of commercial draw at the time of production — the same kind of judgment a baseball front office makes when evaluating a waiver wire pickup. It is computable from existing data without requiring salary figures.

**Why top-3, not top-10:** Top-10 billing would classify significant supporting actors in large ensembles as replacement-level. The threshold we care about is "bankable star" — someone whose name on a poster moves tickets. Top-3 billing is a reasonable proxy for that. The 4th-billed actor in a blockbuster is not freely interchangeable with the lead.

**Why the budget floor clause:** An actor who has headlined a dozen micro-budget films has not demonstrated the ability to perform at mainstream commercial scale. The budget floor ensures the replacement pool reflects genuine market-rate performers, not anyone with screen credits.

**Why not debut films only:** Some debut actors receive top billing (Jennifer Lawrence in *Winter's Bone*). Debut status is a noisy proxy for commercial interchangeability; career billing history is more direct.

## Open Questions

- Whether top-3 or top-5 is the right cutoff — to be revisited once the billing distribution across the dataset is available.
- The exact budget floor ($500K? $1M? $5M?) — to be set relative to the dataset's budget distribution.
- Whether debut performers below the billing threshold should be a separately labeled sub-pool for analytical purposes.

## Known Limitation

Courtesy billing — a recognized performer billed high despite a minor role as a contractual courtesy — introduces noise into this definition. Expected to be low-frequency in the dataset; will be flagged in individual profiles where detectable.
