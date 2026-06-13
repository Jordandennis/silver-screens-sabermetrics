---
date: 2026-06-12
status: accepted
---

# ADR 0001: Gradient Boosted Trees Over Neural Networks for Role Attribution

## Decision

Use gradient boosted trees (XGBoost or LightGBM) with SHAP values for the role attribution model, rather than a neural network.

## Context

The role weight model needs to answer: "how much does each role type (director, lead actor, production company, etc.) contribute to film outcome variance?" This requires not just predictive accuracy but interpretable attribution — the model's explanations are a primary output, not just a means to a prediction.

## Rationale

SHAP (SHapley Additive Explanations) values provide theoretically grounded feature attribution with guarantees of local accuracy, consistency, and missingness handling. They are most reliable and computationally tractable on tree-based models.

Neural network attribution methods (integrated gradients, LIME, attention weights) are noisier for tabular data, less theoretically grounded for this use case, and harder to audit and defend in a public writeup.

Gradient boosted trees also match the data characteristics well: tabular, mixed types, moderate dataset size (~tens of thousands of films), with feature interactions (e.g., genre × director weight). This is a regime where GBMs typically match or exceed neural network performance.

## Tradeoffs

**Loses:** Ability to model highly complex non-linear interaction patterns that very deep networks can learn. For a dataset of this size and type, this is unlikely to matter in practice.  
**Gains:** Interpretable SHAP attribution, faster iteration, more defensible methodology, lower compute cost.

## References

- Lundberg & Lee, "A Unified Approach to Interpreting Model Predictions" (SHAP), NeurIPS 2017
- Géron, *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow*, 3rd ed. (2023), Ch. 7 — Ensemble Methods
