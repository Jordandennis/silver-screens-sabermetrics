---
date: 2026-06-12
status: accepted
---

# ADR 0002: Salary Treated as Optional Input on Financial Axis

## Decision

Salary data is an optional enhancement to Axis 1 (Financial Efficiency), not a required input. Its absence degrades precision on that axis but does not break the model. Outputs will indicate whether a given score uses the base or enhanced calculation.

## Context

Actor and director salary data is largely private. The Numbers publishes some figures; Kaggle datasets capture historical data for major releases. Coverage is uneven and biased toward high-profile films and talent. Requiring salary data would exclude the majority of the filmography database and create selection bias toward commercially prominent work.

## Rationale

The financial axis's base metric — box office revenue / production budget — is fully sourceable and meaningful on its own. It measures a film's return on investment without requiring knowledge of any individual's cut.

When salary data is available, the axis upgrades to a true ROI figure: revenue attributable to individual / individual's salary. This is the closer analogue to baseball's WAR (where replacement-level salary is an explicit input), but the base version is still a valid and interpretable metric.

A tiered model is preferable to a broken one. The base score is always available; the enhanced score is available where data supports it.

## Tradeoffs

**Loses:** Full WAR analogy purity — baseball WAR implicitly incorporates salary through the replacement-level framing.  
**Gains:** Complete filmography coverage; no selection bias toward films with public salary data; honest about what the data actually supports.

## Future

If a reliable comprehensive salary dataset becomes available, the model is designed to accept it as a drop-in upgrade to Axis 1 without structural changes.
