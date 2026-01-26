# Reconnaissance Report: CSE 552 Project Report

**Author**: Gökcan Kahraman
**Date Reviewed**: 2026-01-21
**Document Type**: Course Project Report (Computational Project)

---

## Paper Summary

This paper proposes a Bayesian hierarchical ordinal logistic regression framework to predict football match outcomes (home win, draw, away win) using event-based features from the first 30 minutes of matches. The work focuses on Premier League data from the Wyscout dataset and aims to produce calibrated probability estimates rather than point predictions.

---

## Methodology Classification

| Aspect | Classification |
|--------|----------------|
| **Primary Method** | Bayesian Hierarchical Modeling |
| **Model Type** | Ordinal Logistic Regression |
| **Inference** | MCMC (NUTS/Hamiltonian Monte Carlo) |
| **Feature Engineering** | Event-based (xG, xT, VAEP) + Elo ratings |
| **Domain** | Sports Analytics / Football Prediction |

---

## Claimed Contributions

1. **Hybrid feature framework** combining rich event-based metrics (xG, xT, VAEP) with general match statistics within Bayesian ordinal structure
2. **Early-match prediction** using only first 30 minutes to ensure temporal causality
3. **Hierarchical structure** with team-level random effects and Big-6 home advantage
4. **Ordinal outcome modeling** treating draw as natural intermediate category

---

## Dataset

| Property | Value |
|----------|-------|
| **Source** | Wyscout Open Event Dataset |
| **League** | English Premier League |
| **Data Type** | Event-level football data |
| **External Data** | Elo ratings from ClubElo |
| **Train/Test Split** | 80/20 by match date |

---

## Evaluation

| Metric | Proposed Model | Best Baseline |
|--------|---------------|---------------|
| **Accuracy** | 63.8% | 54.7% (Random Forest with params) |
| **Comparison** | +9.1pp vs RF, +13.1pp vs Logistic |

---

## Document Inventory

| Element | Count | Notes |
|---------|-------|-------|
| **Sections** | 7 | Intro, Contribution, Related Work, Methodology, Results, Discussion, Conclusion |
| **Equations** | ~30 | Heavy on model specification |
| **Figures** | 9 | Pipeline diagram, confusion matrices, correlation plots, xT/VAEP tables |
| **Tables** | 0 inline | Results reported in text |
| **References** | 9 | Mix of sports analytics and Bayesian methods |
| **Pages** | ~12 | Including appendix |

---

## Initial Impressions

### Strengths (Preliminary)

1. **Clear temporal causality design** - 30-minute window explicitly addresses leakage concerns
2. **Principled Bayesian framework** - Appropriate for uncertainty quantification
3. **Rich feature set** - Combines multiple advanced metrics (xG, xT, VAEP)
4. **Honest limitations discussion** - Acknowledges draw prediction difficulty
5. **Good experimental comparison** - Multiple baselines with same data split

### Areas Requiring Deep Review

1. **M2 (Mathematical)**: Multiple formulations of ordinal model appear - need to verify consistency
2. **M4 (Experimental)**: Baseline fairness needs scrutiny (hyperparameter tuning asymmetry?)
3. **M5 (Literature)**: Only 9 references - may be undercited for thesis-level work
4. **M7 (Visualization)**: Figure captions minimal; "Correlation Matrix" labels are misleading (they're confusion matrices)
5. **M1 (Structural)**: GNN paragraph appears twice (lines 78-81 duplicate)
6. **M2 (Mathematical)**: Horseshoe prior mentioned but Carvalho et al. citation incomplete

### Potential Issues Flagged

| Issue | Location | Severity (Est.) |
|-------|----------|-----------------|
| Duplicate paragraph | Lines 78-81 | Minor |
| Elo ratings timing unclear | Abstract, Methodology | Major |
| Missing citation for Horseshoe | Line 336 | Minor |
| Figure labels incorrect | Figs 1-4 say "Correlation Matrix" but show confusion matrices | Minor |
| Outcome encoding inconsistent | Line 112 vs elsewhere | Major |

---

## Recommended Module Priority

| Priority | Module | Rationale |
|----------|--------|-----------|
| 1 | **M4** | Experimental design critical for validity |
| 2 | **M2** | Mathematical consistency issues spotted |
| 3 | **M1** | Structural issues (duplicate text, notation) |
| 4 | **M5** | Literature coverage concerns |
| 5 | **M7** | Figure/caption quality |
| 6 | **M6** | Writing polish |
| 7 | **M3** | Algorithmic novelty assessment |
| 8 | **M8** | Not applicable (course project, not thesis) |

---

*Generated using ML Manuscript Technical Audit Framework - Reconnaissance Phase*
