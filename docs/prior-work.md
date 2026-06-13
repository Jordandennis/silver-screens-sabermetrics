# Prior Work

Positioning this project relative to existing work on quantifying film industry performance.

## What This Project Is Not

This is not a box office prediction service and not a studio greenlight tool. It is specifically an *individual contribution attribution* model — answering "how much did this person add above a replacement-level alternative?" The framing is retrospective and evaluative, not prospective.

## Related Work

### The Numbers (Nash Information Services)
**URL:** the-numbers.com  
**What it does:** Tracks box office performance, budgets, and production/distribution data. Publishes "bankability" rankings for actors and directors based on cumulative and average performance metrics.  
**Where it stops short:** Rankings are raw averages with no era normalization, no replacement-level baseline, and no uncertainty estimates. A director's 1990 output is implicitly compared against their 2024 output without cohort adjustment. No credit attribution model — an actor in a $200M ensemble gets the same credit as the lead.  
**What we borrow:** Box office and budget data; validation that "bankability" is a tractable and useful question.

### Ulmer Scale — James Ulmer
*[Verify: Ulmer published a star power ranking methodology, reportedly used by international distributors to price talent. Confirm publication/methodology details before citing publicly.]*  
Ulmer developed a quantitative "star power" index ranking actors by commercial draw in international markets. Directionally similar to Axis 1 (financial efficiency). Methodology is reportedly proprietary and not publicly reproducible, which is the gap this project aims to fill.

### Antipov & Pokryshevskaya
*[Verify: confirm paper title, venue, and year before citing. Believed to be ML-based box office prediction work relevant to the role weight model.]*  
Academic work on predicting film outcomes using machine learning methods. Relevant as prior art on feature selection for box office prediction — informs what to include in the gradient boosted tree role weight model.

### openWAR (Baumer & Tango, 2014)
**What it does:** Extended baseball WAR to publish simulation-based confidence intervals rather than point estimates only.  
**Relevance:** Direct methodological inspiration for SSWAR uncertainty quantification. The core argument — that a single WAR number overstates certainty — applies directly here given collinearity across collaborators and salary data gaps.  
**Citation:** Baumer, B. and Tango, T. (2014). "openWAR: An Open Source System for Evaluating Overall Player Performance in Major League Baseball." *Journal of Quantitative Analysis in Sports*, 10(2).

### Existing WAR Lineage (Baseball)
Understanding the baseball precedent is prerequisite to adapting it. Key works:
- Palmer, P. (1984). *Total Player Rating* — first runs-above-average framework
- Woolner, K. (1995). *VORP* — introduced replacement level as the explicit baseline
- Tango, T., Lichtman, M., Dolphin, A. (2006). *The Book: Playing the Percentages in Baseball* — codified modern replacement level
- Smith, S. (2008/2010). WAR implementation at Baseball-Reference — the specific methodology this project's audience will reference

## The Gap This Project Fills

Existing film analytics either (a) predict aggregate film outcomes without attributing credit to individuals, or (b) rank individuals by raw averages without a replacement-level baseline, era normalization, or uncertainty quantification. No publicly available work applies the complete WAR framework — replacement level + cohort normalization + empirical role weights + confidence intervals — to film industry contributors across all roles.
