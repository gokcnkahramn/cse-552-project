# Issue Tracker: CSE 552 Football Prediction Project

**Author**: Gökcan Kahraman
**Review Date**: 2026-01-21

---

## Quick Reference

| Priority | Definition | Action Required |
|----------|------------|-----------------|
| **CRITICAL** | Validity/correctness issues | Must fix before submission |
| **MAJOR** | Significant quality issues | Should fix |
| **MINOR** | Polish issues | Fix if time permits |

---

## Critical Issues

| ID | Module | Issue | Location | Status | Notes |
|----|--------|-------|----------|--------|-------|
| C1 | M2 | Outcome encoding inconsistency | Lines 112 vs 54-55 | OPEN | 0=Home vs Away? |
| C2 | M4 | Elo rating temporal validity unclear | Abstract, Sec 4.1 | OPEN | Potential leakage |

---

## Major Issues

| ID | Module | Issue | Location | Status | Notes |
|----|--------|-------|----------|--------|-------|
| M1 | M1 | Duplicate GNN paragraph | Lines 78-81 | OPEN | Copy-paste error |
| M2 | M2 | Latent score formulation inconsistent | Lines 58-61 vs 194-199 | OPEN | α vs η^prior |
| M3 | M2 | Cutpoint notation mismatch | Lines 50-51 vs 227-229 | OPEN | θ vs c |
| M4 | M4 | Baseline comparison potentially unfair | Section 4.6 | OPEN | Tuning asymmetry |
| M5 | M4 | Missing statistical significance | Section 6 | OPEN | No CIs or tests |
| M6 | M5 | Limited citations (only 9) | Bibliography | OPEN | Need ~15-20 |
| M7 | M7 | Figure labels incorrect | Figs 1-4 | OPEN | "Correlation" → "Confusion" |
| M8 | M7 | Figure captions non-descriptive | All figures | OPEN | Need expansion |

---

## Minor Issues

| ID | Module | Issue | Location | Status | Notes |
|----|--------|-------|----------|--------|-------|
| m1 | M1 | Missing notation table | - | OPEN | |
| m2 | M1 | Excessive subsection nesting | Sec 4.5 | OPEN | |
| m3 | M1 | Abstract in titlepage env | Lines 18-28 | OPEN | |
| m4 | M2 | VAEP definition inconsistent | Lines 139-146 | OPEN | Two definitions |
| m5 | M2 | Missing Horseshoe citation | Line 336 | OPEN | Carvalho 2010 |
| m6 | M3 | Novelty overclaiming | Section 2 | OPEN | |
| m7 | M3 | No complexity analysis | - | OPEN | |
| m8 | M4 | Sample size not reported | - | OPEN | |
| m9 | M4 | No calibration assessment | - | OPEN | |
| m10 | M5 | Missing positioning vs Baio | Section 3 | OPEN | |
| m11 | M6 | Orphan sentence | Line 290 | OPEN | Incomplete |
| m12 | M6 | Percentage format inconsistent | Throughout | OPEN | % vs "per cent" |
| m13 | M7 | No results summary table | Section 6 | OPEN | |
| m14 | M7 | PNG format for figures | Appendix | OPEN | Use vector |

---

## Issue Details

### C1: Outcome Encoding Inconsistency

**Module**: M2 (Mathematical Rigor)
**Priority**: CRITICAL
**Status**: OPEN
**Location**: Line 112 vs Lines 54-55, 227-229

**Description**:
Line 112 defines: `y_i ∈ {0,1,2}` where "0: Home Win, 1: Draw, 2: Away Win"
But Lines 54-55 state: "outcome categories are ordered as Away Win, Draw, and Home Win"
These are opposite orderings.

**Impact**:
- Reader cannot verify model correctness
- Implementation may have bugs if code doesn't match paper
- Ordinal interpretation depends on ordering

**Suggested Fix**:
Standardize to: `y ∈ {0, 1, 2}` where 0 = Away Win, 1 = Draw, 2 = Home Win
This matches natural ordinal progression from home team perspective.

---

### C2: Elo Rating Temporal Validity

**Module**: M4 (Experimental Design)
**Priority**: CRITICAL
**Status**: OPEN
**Location**: Abstract, Section 4.1

**Description**:
"Elo ratings obtained from ClubElo" but no specification of WHEN ratings were obtained relative to match date.

**Impact**:
If ratings include information from after match date, this constitutes data leakage and invalidates results.

**Suggested Fix**:
Add explicit statement: "Pre-match Elo ratings (as of the day before each match) were obtained from ClubElo."
Verify implementation matches this description.

---

### M7: Figure Labels Incorrect

**Module**: M7 (Visualization)
**Priority**: MAJOR
**Status**: OPEN
**Location**: Figures 1-4 (Lines 274-304)

**Description**:
All figures labeled "Correlation Matrix" but they clearly show confusion matrices (predicted vs actual class counts).

**Impact**:
Fundamental terminology error; correlation matrices show variable relationships, not classification performance.

**Suggested Fix**:
Change all captions:
- "Correlation Matrix of Random Forest" → "Confusion Matrix for Random Forest"
- etc.

---

## Progress Summary

### By Priority

| Priority | Total | Open | Resolved |
|----------|-------|------|----------|
| Critical | 2 | 2 | 0 |
| Major | 8 | 8 | 0 |
| Minor | 14 | 14 | 0 |
| **Total** | **24** | **24** | **0** |

### By Module

| Module | Critical | Major | Minor | Total |
|--------|----------|-------|-------|-------|
| M1: Structure | 0 | 1 | 3 | 4 |
| M2: Mathematical | 1 | 2 | 2 | 5 |
| M3: Algorithmic | 0 | 0 | 2 | 2 |
| M4: Experimental | 1 | 2 | 2 | 5 |
| M5: Literature | 0 | 1 | 1 | 2 |
| M6: Writing | 0 | 0 | 2 | 2 |
| M7: Visualization | 0 | 2 | 2 | 4 |
| **Total** | **2** | **8** | **14** | **24** |

---

## Revision Checklist

### Before Resubmission

- [ ] C1: Outcome encoding standardized throughout
- [ ] C2: Elo timing explicitly stated and verified
- [ ] M1: Duplicate paragraph removed
- [ ] M7: Figure labels corrected

### Recommended Additions

- [ ] Notation table added
- [ ] Results summary table added
- [ ] Statistical significance reported
- [ ] 5-10 additional citations added
- [ ] Figure captions expanded

---

## Sign-Off

### Reviewer Sign-Off

- [ ] All critical issues resolved
- [ ] All major issues resolved or justified
- [ ] Student aware of remaining items

**Reviewer**: ML Manuscript Technical Audit
**Date**: 2026-01-21

### Student Acknowledgment

- [ ] I have reviewed all issues
- [ ] I understand the required changes
- [ ] I have a plan to address remaining items

**Student**: _______________
**Date**: _______________
