# Complete Technical Review: CSE 552 Project Report

**Title**: Football Match Outcome Prediction using Bayesian Hierarchical Ordinal Logistic Regression
**Author**: Gökcan Kahraman
**Reviewer**: Technical Audit Framework
**Date**: 2026-01-21

---

## Executive Summary

This project report presents a Bayesian hierarchical ordinal logistic regression model for predicting football match outcomes using early-match (0-30 min) event data. The work demonstrates solid understanding of Bayesian modeling principles and achieves meaningful performance improvements over baselines (63.8% vs 54.7%). However, several issues require attention: mathematical notation inconsistencies, potential baseline comparison unfairness, duplicate text, and figure labeling errors. The core methodology is sound, but the presentation needs refinement before submission.

**Overall Assessment**: Good project with solid methodology; needs revision for clarity and consistency.

---

## Module 1: Structural Integrity

### Strengths
- Clear section organization following standard academic structure
- Abstract provides comprehensive overview with quantitative results
- Logical flow from motivation → contribution → methodology → results

### Issues

**[MAJOR] M1-01: Duplicate Paragraph**
- **Location**: Lines 78-81 (Section 3: Related Work)
- **Issue**: The GNN paragraph appears twice verbatim:
  > "In this context, Graph Neural Network (GNN)-based approaches have begun to be used to represent player and team interaction networks [8]. However, these studies generally focus on specific game events..."
- **Impact**: Appears as copy-paste error; reduces professionalism
- **Recommendation**: Remove the duplicate occurrence (keep first instance at line 78-80)

**[MINOR] M1-02: Missing Notation Table**
- **Location**: Throughout document
- **Issue**: No consolidated notation table despite heavy mathematical content (~30 equations)
- **Impact**: Reader must search for variable definitions
- **Recommendation**: Add notation table after Introduction or at start of Methodology

**[MINOR] M1-03: Inconsistent Section Depth**
- **Location**: Section 4 (Methodology)
- **Issue**: Subsections go to 4.5.7 depth; excessive nesting
- **Impact**: Navigation difficulty
- **Recommendation**: Flatten structure; consider merging 4.5.1-4.5.7 into fewer subsections

**[MINOR] M1-04: Abstract in titlepage Environment**
- **Location**: Lines 18-28
- **Issue**: Abstract placed inside `\titlepage` environment without `\begin{abstract}` tag
- **Impact**: Non-standard LaTeX; may cause formatting issues
- **Recommendation**: Use proper `\begin{abstract}...\end{abstract}` after `\maketitle`

---

## Module 2: Mathematical Rigor

### Strengths
- Ordinal logistic model correctly specified
- Hierarchical structure well-motivated
- Prior specifications appropriate (Horseshoe, HalfNormal, HalfCauchy)

### Issues

**[CRITICAL] M2-01: Outcome Encoding Inconsistency**
- **Location**: Line 112 vs Lines 54-55, 227-229
- **Issue**: Line 112 states: `y_i ∈ {0,1,2}` where "0: Home Win, 1: Draw, 2: Away Win"
  But Lines 54-55 state: "outcome categories are ordered as Away Win, Draw, and Home Win"
  And Lines 227-229 use: `P(y_i = 0)` for first category without specifying which
- **Impact**: Fundamental ambiguity in model interpretation; reader cannot verify correctness
- **Recommendation**: Standardize encoding throughout. Suggest:
  ```
  y ∈ {0, 1, 2} where 0 = Away Win, 1 = Draw, 2 = Home Win
  ```
  This matches ordinal progression (away < draw < home from home team perspective)

**[MAJOR] M2-02: Latent Score Sign Convention**
- **Location**: Lines 58-61 vs Lines 194-199
- **Issue**: Two different formulations of latent score:
  - Eq at line 58: `η_m = x_m^T β + α + u_home - u_away + γ·Big6_home + δ·EloDiff`
  - Eq at line 195: `η_ik = X_i^T β_k + η^(prior)_ik + (u_{t_i^H,k} - u_{t_i^A,k}) + b_k·I{t_i^H ∈ Big6}`
  First uses scalar α, second uses η^(prior)_ik. Are these equivalent?
- **Impact**: Unclear if same model; confuses implementation
- **Recommendation**: Use single consistent formulation; explain relationship if both needed

**[MAJOR] M2-03: Cutpoint Notation Mismatch**
- **Location**: Lines 50-51 vs Lines 227-229
- **Issue**: First uses θ_k as cutpoints, second uses c_1, c_2
- **Impact**: Reader cannot verify mathematical consistency
- **Recommendation**: Use consistent notation (suggest c_k throughout)

**[MINOR] M2-04: VAEP Definition Inconsistency**
- **Location**: Lines 139-146
- **Issue**: VAEP defined two ways:
  1. `VAEP(a_t) = P(score_{t+1}) - P(concede_{t+1})`
  2. `VAEP = xT_after - xT_before`
  These are fundamentally different definitions
- **Impact**: Unclear which implementation was used
- **Recommendation**: Clarify that (1) is the conceptual definition and (2) is an approximation, or correct if error

**[MINOR] M2-05: Missing Citation for Horseshoe Prior**
- **Location**: Line 336 references "[Carvalho2010]"
- **Issue**: This citation is not in the bibliography (only 9 references, none is Carvalho)
- **Impact**: Incomplete attribution
- **Recommendation**: Add: Carvalho, C. M., Polson, N. G., & Scott, J. G. (2010). The horseshoe estimator for sparse signals. Biometrika, 97(2), 465-480.

---

## Module 3: Algorithmic Contribution

### Strengths
- Clear statement of contribution (hybrid feature + Bayesian ordinal framework)
- Appropriate methodology for the problem (ordinal outcomes, uncertainty quantification)
- Team random effects and Big-6 adjustment are sensible modeling choices

### Issues

**[MINOR] M3-01: Novelty Overclaiming**
- **Location**: Section 2 (Contribution)
- **Issue**: Claims "main contribution is the development of a hybrid modelling framework" but ordinal logistic regression with hierarchical effects is well-established
- **Impact**: May not meet novelty threshold for publication venues
- **Recommendation**: Reframe as "application and empirical validation" rather than "development"

**[MINOR] M3-02: No Complexity Analysis**
- **Location**: Missing
- **Issue**: No discussion of computational cost (MCMC chains, feature computation time)
- **Impact**: Reproducibility concern; practical applicability unclear
- **Recommendation**: Add brief note on: number of parameters, sampling time, hardware used

---

## Module 4: Experimental Design

### Strengths
- Temporal train/test split (80/20 by date) prevents leakage
- Same feature window (30 min) used for all models
- Multiple baselines compared (RF, Logistic, score-based)
- Honest reporting of draw prediction failure

### Issues

**[CRITICAL] M4-01: Elo Rating Temporal Validity Unclear**
- **Location**: Abstract line 22, Section 4.1
- **Issue**: "Elo ratings obtained from ClubElo" - no specification of WHEN ratings were obtained
  - If ratings are from after match date → data leakage
  - If ratings are from season start → may not reflect current form
  - If ratings are pre-match → should be explicitly stated
- **Impact**: Potential fundamental validity issue; results may be artificially inflated
- **Recommendation**: Explicitly state: "Pre-match Elo ratings (as of match day minus 1) were obtained from ClubElo"

**[MAJOR] M4-02: Unfair Baseline Comparison**
- **Location**: Section 4.6 (Experimental Setup), Line 251
- **Issue**: Bayesian model uses "1000 draws with 1000 tuning steps" while:
  - Random Forest uses "100 trees" with apparent default hyperparameters
  - Logistic Regression uses "lbfgs solver with max 500 iterations"
  No hyperparameter tuning reported for baselines
- **Impact**: Performance gap may be due to tuning effort asymmetry
- **Recommendation**: Either:
  1. Tune all methods comparably (grid search for RF/LR)
  2. Explicitly state "baselines used default hyperparameters" and discuss implications
  3. Report RF performance with more trees (500, 1000)

**[MAJOR] M4-03: Missing Statistical Significance**
- **Location**: Section 6 (Results)
- **Issue**: No confidence intervals, standard errors, or significance tests
  - 63.8% vs 54.7% - is this statistically significant?
  - No cross-validation or multiple random seeds reported
- **Impact**: Cannot assess whether improvement is real or due to variance
- **Recommendation**: Report:
  1. Posterior predictive accuracy with credible intervals
  2. Bootstrap confidence intervals for baseline accuracies
  3. McNemar's test or similar for pairwise comparison

**[MINOR] M4-04: Sample Size Not Reported**
- **Location**: Missing
- **Issue**: Number of matches in train/test sets not explicitly stated
- **Impact**: Cannot assess statistical power
- **Recommendation**: Add: "The dataset contains N matches, with N_train (80%) for training and N_test (20%) for evaluation"

**[MINOR] M4-05: No Calibration Assessment**
- **Location**: Missing
- **Issue**: Claims "calibrated probability estimates" but no calibration metrics (Brier score, reliability diagram)
- **Impact**: Key claim unsupported
- **Recommendation**: Add reliability diagram and Brier score comparison

---

## Module 5: Literature Positioning

### Strengths
- Good coverage of foundational Bayesian football models (Baio & Blangiardo)
- Mentions recent xG and GNN approaches
- Fair acknowledgment of ML vs statistical modeling tradeoffs

### Issues

**[MAJOR] M5-01: Limited Citation Count**
- **Location**: Bibliography (9 references)
- **Issue**: Only 9 references for a research project is minimal
- **Impact**: May miss important related work; appears underresearched
- **Recommendation**: Add citations for:
  - Dixon & Coles (1997) - foundational Poisson model
  - Maher (1982) - early football prediction
  - Rue & Salvesen (2000) - Bayesian football
  - Karlis & Ntzoufras (2003) - bivariate Poisson
  - SOCR/StatsBomb technical reports on xG

**[MINOR] M5-02: Missing Self-Positioning Against Key Work**
- **Location**: Section 3
- **Issue**: Baio & Blangiardo (2024) cited but not directly compared experimentally
- **Impact**: Unclear how proposed method compares to state-of-the-art Bayesian approach
- **Recommendation**: Discuss why their approach wasn't used as baseline (different problem formulation, data requirements, etc.)

---

## Module 6: Writing Quality

### Strengths
- Generally clear technical writing
- Good use of mathematical notation
- Appropriate academic tone

### Issues

**[MINOR] M6-01: Tense Inconsistency**
- **Location**: Throughout
- **Issue**: Shifts between past ("was found", "achieved") and present ("is defined", "captures")
- **Recommendation**: Use past tense for methods/results, present for general statements

**[MINOR] M6-02: Orphan Sentence**
- **Location**: Line 290, end of paragraph
- **Issue**: "These results show that while including advanced parameters in the model increases prediction accuracy." - incomplete sentence
- **Recommendation**: Complete the thought or merge with previous sentence

**[MINOR] M6-03: Percentage Formatting Inconsistent**
- **Location**: Throughout
- **Issue**: Mix of "63.8%" and "63.8 per cent"
- **Recommendation**: Use consistent format (% symbol preferred in technical writing)

**[MINOR] M6-04: British/American Spelling Mix**
- **Location**: Throughout
- **Issue**: Mix of "modelling" (British) and other American conventions
- **Recommendation**: Choose one convention and apply consistently

---

## Module 7: Visualization Quality

### Strengths
- Pipeline diagram (Figure in Appendix) provides good overview
- xT and VAEP tables included for reference

### Issues

**[MAJOR] M7-01: Incorrect Figure Labels**
- **Location**: Figures 1-4 (Lines 274-304)
- **Issue**: All labeled "Correlation Matrix" but they are confusion matrices
  - Fig 1: "Correlation Matrix of Random Forest without Main Parameters"
  - Fig 2: "Correlation Matrix of Logistic Regression without Main Parameters"
  - etc.
- **Impact**: Fundamentally incorrect terminology; confuses readers
- **Recommendation**: Change all to "Confusion Matrix of [Model Name]"

**[MAJOR] M7-02: Figure Captions Non-Descriptive**
- **Location**: All figures
- **Issue**: Captions don't describe what reader should observe
  - "The diagram of project pipeline" - what are the key stages?
  - "Main Model Correlation Matrix" - what does it show about performance?
- **Impact**: Reader cannot understand figures without extensive text reading
- **Recommendation**: Expand captions, e.g.:
  > "Confusion matrix for the Bayesian hierarchical model on test data. The model correctly predicts 48 home wins but misclassifies 12 draws as home wins, reflecting the known difficulty of draw prediction."

**[MINOR] M7-03: No Inline Results Table**
- **Location**: Section 6
- **Issue**: All quantitative results in prose; no summary table
- **Impact**: Difficult to compare model performance at a glance
- **Recommendation**: Add summary table:
  ```
  | Model | Accuracy | Home F1 | Draw F1 | Away F1 |
  |-------|----------|---------|---------|---------|
  | Random (30min score) | 53.2% | ... | ... | ... |
  | RF (no params) | 53.3% | ... | 0.00 | ... |
  | RF (with params) | 54.7% | ... | 0.00 | ... |
  | Logistic (with params) | 50.7% | ... | ... | ... |
  | Bayesian Ordinal | 63.8% | ... | ... | ... |
  ```

**[MINOR] M7-04: Figure Resolution**
- **Location**: Appendix figures
- **Issue**: PNG format may not scale well for print
- **Recommendation**: Use vector formats (PDF) for diagrams where possible

---

## Module 8: Research Trajectory

*Not applicable - this is a course project report, not a thesis progress report.*

---

## Summary of Findings

### By Severity

| Severity | Count | Key Issues |
|----------|-------|------------|
| **CRITICAL** | 2 | Outcome encoding inconsistency, Elo timing unclear |
| **MAJOR** | 7 | Notation inconsistencies, baseline fairness, missing stats, figure labels |
| **MINOR** | 14 | Duplicate text, citation gaps, writing polish |
| **SUGGESTION** | 0 | - |

### By Module

| Module | Critical | Major | Minor |
|--------|----------|-------|-------|
| M1: Structure | 0 | 1 | 3 |
| M2: Mathematical | 1 | 2 | 2 |
| M3: Algorithmic | 0 | 0 | 2 |
| M4: Experimental | 1 | 2 | 2 |
| M5: Literature | 0 | 1 | 1 |
| M6: Writing | 0 | 0 | 4 |
| M7: Visualization | 0 | 2 | 2 |

---

## Prioritized Action Items

### Must Fix (Critical)

1. **M2-01**: Standardize outcome encoding throughout document
2. **M4-01**: Clarify Elo rating temporal validity

### Should Fix (Major)

3. **M1-01**: Remove duplicate GNN paragraph
4. **M2-02**: Unify latent score formulation
5. **M2-03**: Consistent cutpoint notation
6. **M4-02**: Address baseline comparison fairness
7. **M4-03**: Add statistical significance measures
8. **M5-01**: Expand literature citations
9. **M7-01**: Fix "Correlation Matrix" → "Confusion Matrix"
10. **M7-02**: Improve figure captions

### Nice to Fix (Minor)

11. M1-02: Add notation table
12. M2-04: Clarify VAEP definition
13. M2-05: Add Horseshoe prior citation
14. M6-02: Complete orphan sentence
15. M7-03: Add results summary table

---

## Strengths Recognition

1. **Sound methodology**: Bayesian ordinal logistic regression is appropriate for this problem
2. **Temporal causality**: 30-minute cutoff design prevents obvious leakage
3. **Honest limitations**: Discussion openly addresses draw prediction difficulty
4. **Rich feature engineering**: Combines multiple advanced metrics meaningfully
5. **Clear presentation**: Despite issues, the overall narrative is followable

---

## Development Roadmap

**Phase 1: Critical Fixes (Required)**
- Fix outcome encoding consistency
- Clarify Elo timing

**Phase 2: Major Revisions**
- Unify mathematical notation
- Add statistical significance
- Fix figure labels and captions
- Expand literature review

**Phase 3: Polish**
- Remove duplicate text
- Add notation table and results table
- Writing consistency fixes

---

*Generated using ML Manuscript Technical Audit Framework*
*Review completed: 2026-01-21*
