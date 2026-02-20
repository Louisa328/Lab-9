# Lab-9
# Recovering Experimental Truths via Propensity Score Matching

## Objective

Applied Propensity Score Matching (PSM) to correct for selection bias in observational labor market data, recovering a causally valid estimate of treatment effect from a dataset where naive comparison renders deeply misleading conclusions.

---

## Methodology

**Dataset:** Observational subset of the Lalonde (1986) dataset — a canonical benchmark for causal inference methods, containing individual-level earnings and demographic data from a job training program evaluation.

**Pipeline:**

- **Modeled Selection Bias:** Identified the confounders driving non-random selection into treatment (job training participation), including age, education, race, marital status, and prior earnings (`re75`, `re78`). Recognized that participants systematically differ from non-participants on pre-treatment characteristics, invalidating direct comparisons.

- **Estimated Propensity Scores:** Fit a logistic regression model to predict each individual's probability of receiving treatment given their observed covariates. The resulting propensity score compresses all confounding information into a single scalar — the balancing score — enabling like-for-like comparisons across treatment groups.

- **Applied Nearest Neighbor Matching:** Matched each treated unit to its closest control-group counterpart by propensity score distance, using a 1-to-1 nearest neighbor algorithm. This constructed a quasi-experimental matched sample in which treated and control units are comparable on all modeled confounders.

- **Estimated the Average Treatment Effect on the Treated (ATT):** Computed the post-matching difference in mean outcomes between treated and matched control units to recover an unbiased estimate of the causal effect.

---

## Key Findings

| Estimator | Estimated Treatment Effect |
|---|---|
| Naïve Difference in Means (raw, unadjusted) | −$15,204 |
| PSM-Adjusted Estimate (post-matching ATT) | +$1,800 (approx.) |

The naïve comparison produces a treatment effect estimate of **−$15,204**, suggesting that job training *reduces* earnings — a conclusion that is not only wrong but inverted. This is a textbook manifestation of **selection bias**: individuals who selected into training were systematically lower earners prior to the program, making the raw comparison confounded by pre-existing disadvantage rather than the effect of training itself.

After applying Propensity Score Matching to control for observed confounders, the estimated treatment effect shifts to approximately **+$1,800** — consistent with the experimental benchmark from the original randomized evaluation. This recovery of the true positive effect demonstrates that PSM, when properly implemented, can approximate experimental estimates from observational data, provided the **Conditional Independence Assumption** (unconfoundedness) holds.

> **Takeaway:** A swing of nearly **$17,000** separates the naïve and causal estimates — underscoring that in observational settings, selection bias does not merely attenuate estimates; it can reverse them entirely.

---

## Tools & Stack

`Python` · `Pandas` · `Scikit-Learn` · `Statsmodels` · `Seaborn`
