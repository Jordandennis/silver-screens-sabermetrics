# Methodology

## The WAR Analogy

Silver Screens Sabermetrics borrows the philosophical framework of baseball's Wins Above Replacement (WAR) statistic. WAR asks: "How many wins did this player contribute compared to a freely available replacement-level player?" We ask the equivalent for film: "How much did this person's presence improve the outcome of a film compared to a replacement-level contributor in their role?"

WAR's lineage is collective — Pete Palmer's Total Player Rating (1984), Keith Woolner's VORP (1995), Tom Tango's codification of replacement level, through to the implementations at Baseball-Reference and FanGraphs. We borrow the *philosophy* of replacement level rather than any specific implementation, because film attribution differs fundamentally from baseball's clean per-plate-appearance accounting. The mechanics must be rebuilt from scratch.

## Replacement Level

### The Problem

Baseball's replacement level is cleanly defined: freely available minor league talent, acquirable off waivers at minimal cost. Film has no equivalent observable talent pipeline.

### Working Definition (v0.1)

A replacement-level performer is any individual who meets either condition:

1. **Billing threshold:** Career billing history has never placed them in the top 3 billing positions of any film's cast (per TMDB cast data).
2. **Budget floor:** Their entire filmography consists of films below a minimum production budget (TBD — likely ~$1M).

Billing order proxies "freely available" because it reflects the studio's own assessment of commercial draw. Top-3 billing corresponds to lead roles — the "bankable star" threshold above which studios pay a meaningful premium. Below that, the individual is closer to interchangeable market-rate talent.

**Open questions:**
- Whether top-3 or top-5 is the right cutoff (to be revisited after data distribution is available).
- The exact budget floor value.
- Whether debut-film performers should be a separate labeled pool rather than folded into this definition.

**Known limitation:** Courtesy billing — a recognized name billed high despite a minor role — introduces noise. Expected to be low-frequency; will be flagged in individual profiles where detectable.

See [ADR 0003](decisions/0003-replacement-level.md) for full rationale.

## Role Weights

Before individual scores can be computed, we need an empirical answer to: *how much does each role type (director, lead actor, supporting actor, production company) contribute to a film's outcome on average?* A lead actor is not worth the same as a sixth-billed supporting player, but the difference should be measured, not assumed.

### Approach: Gradient Boosted Trees + SHAP Attribution

We will train a gradient boosted tree model (XGBoost or LightGBM) on film outcome data with features encoding role identities and context:

- Director
- Billing-1, billing-2, billing-3 actor
- Production company
- Genre
- Budget tier
- Release window (season, competitive landscape)
- Sequel / franchise flag

SHAP (SHapley Additive Explanations) values will measure the marginal contribution of each feature class to the model's predictions — yielding empirical estimates of the form "the director feature accounts for ~X% of predicted outcome variance."

These structural weights drive Phase 3 attribution: once we know the director slot is empirically worth X%, we can compute how a specific director's films over- or under-perform that slot relative to a replacement-level director.

*Gradient boosted trees were chosen over neural networks for SHAP interpretability. See [ADR 0001](decisions/0001-gbm-over-nn.md).*

### The Frequent Collaborator Problem

Film presents a collinearity challenge baseball largely avoids. Creative partnerships recur: Scorsese/DiCaprio, Lanthimos/Stone/Plemons, Nolan/Murphy. Isolating individual contributions from the effects of the collaboration itself is genuinely hard, and the model should not pretend otherwise.

**Base approach:** Individuals are scored independently. Confidence intervals will be wide for individuals whose filmography is concentrated in one creative partnership.

**Planned extension (Phase 4+):** A "frequent group score" that explicitly models recurring creative partnerships as a unit, separate from individual scores. This would capture synergistic collaborations where the partnership exceeds the sum of its parts, and flag cases where an individual's score is heavily dependent on one working relationship.

## The Three Axes

Each individual receives scores on three dimensions, combined into a composite (SSWAR — Silver Screens Wins Above Replacement).

### Axis 1: Financial Efficiency

Measures the return on a film's investment attributable to a given individual.

- **Base metric:** Box office revenue / production budget (revenue multiple), era-normalized within budget tier and release year cohort.
- **Enhanced metric (when salary data is available):** Revenue attributable to individual / individual's salary — a true ROI figure. Salary data incorporated from The Numbers, Kaggle datasets, or other sources where available, and flagged as known vs. estimated.

Salary is treated as an optional input. Its absence reduces precision but does not break the model. See [ADR 0002](decisions/0002-salary-as-xfactor.md).

### Axis 2: Critical Reception

Measures the effect of an individual's presence on a film's critical reception.

- **Primary metric:** Normalized critic scores (Rotten Tomatoes Tomatometer, Metacritic Metascore), made cohort-relative — compared against films of similar budget tier, genre, and era.
- **Pending decision:** Final source selection and normalization scheme. RT (binary fresh/rotten aggregate) and Metacritic (weighted average) measure different things; the combination method requires a documented decision before implementation.

### Axis 3: Market Appeal

Measures the effect of an individual's presence on general audience reception, distinct from critical consensus.

- **Primary metric:** Audience scores (TMDB user rating, RT Audience Score).
- **Rationale for separating Axes 2 and 3:** Critical and audience reception can diverge sharply — prestige films vs. crowd-pleasers, cult films gaining reputation over time. Collapsing both into one score loses that signal.
- **Pending decision:** Source weighting when scores diverge significantly between TMDB and RT.

## Composite Score (SSWAR)

*TBD — pending role weight estimates from the gradient boosted model.* The composite will be a weighted combination of the three axes, where weights reflect empirical role contribution estimates. Assumed weights will not be used for the final score.

## Era Normalization

All financial figures adjusted to a common year (2024 USD) using CPI. Beyond inflation adjustment, scores are normalized against a cohort of films with similar budget tier, genre, and release era — a 1985 drama is compared against other 1985 dramas, not against 2024 blockbusters.

Cohort definition is a significant methodological choice that will be documented as an ADR when finalized.

## Uncertainty and Confidence Intervals

Scores will publish uncertainty estimates alongside point estimates, following the openWAR approach (Baumer & Tango, 2014). This is especially important given:

- Collinearity in frequent collaborator effects
- Sparsity for individuals with short or concentrated filmographies
- Salary data gaps on Axis 1

A score without a confidence interval is a false claim of precision.

## Known Limitations

| Limitation | Impact | Mitigation |
|---|---|---|
| Collinearity in frequent collaborations | Individual attribution is imprecise for concentrated careers | Wide confidence intervals; planned group score extension |
| Salary data sparsity | Axis 1 full form unavailable for most films | Salary is optional; outputs flagged when base vs. enhanced |
| Scope (US theatrical, budget floor TBD) | Results may not generalize to independent or international film | Clearly scoped in all outputs |
| Fixed role weights across genres | Director weight may be higher in auteur films vs. franchise entries | Noted limitation; interaction effects as future extension |
| Survivorship bias | Only films that were made are analyzed | Acknowledged; no direct mitigation possible |
| Courtesy billing noise | Billing order signal degraded for high-profile cameos | Flagged in individual profiles where detectable |
