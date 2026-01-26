**To**: Gökcan Kahraman
**Subject**: CSE 552 Project Report - Technical Review Feedback

---

Dear Gökcan,

I have completed a technical review of your CSE 552 project report on football match outcome prediction using Bayesian hierarchical ordinal logistic regression.

## Review Process

Your manuscript was reviewed using a systematic 8-module technical audit framework covering:

- Structural integrity
- Mathematical rigor
- Algorithmic contribution
- Experimental design
- Literature positioning
- Writing quality
- Visualization quality

## What You Will Receive

I am sharing three documents with you:

1. **reconnaissance.md** - Initial assessment identifying the manuscript type, methodology, and areas requiring attention

2. **complete-review.md** - Detailed findings organized by module, with specific locations, explanations, and recommended fixes

3. **issue-tracker.md** - A prioritized checklist of all issues you can use to track your revisions

## Summary of Findings

| Priority | Count |
|----------|-------|
| Critical | 2 |
| Major | 8 |
| Minor | 14 |

### Critical Issues (Must Fix)

1. **Outcome encoding inconsistency**: Line 112 defines y=0 as "Home Win", but the ordinal structure at lines 54-55 implies the opposite ordering. This needs to be standardized throughout the document.

2. **Elo rating temporal validity**: The paper does not specify when Elo ratings were obtained relative to match dates. Please clarify that pre-match ratings were used to confirm there is no data leakage.

### Key Major Issues

- Duplicate paragraph about GNN (lines 78-81) - appears twice
- Notation inconsistencies between different equation blocks
- Figures 1-4 are labeled "Correlation Matrix" but they are confusion matrices
- Missing statistical significance tests for the accuracy comparison

## Strengths of Your Work

I want to highlight several positive aspects:

- Sound methodology choice (Bayesian ordinal logistic regression is appropriate)
- Good temporal causality design with the 30-minute cutoff
- Honest discussion of limitations, especially draw prediction difficulty
- Rich feature engineering combining multiple advanced metrics

## How to Proceed

1. Start with the two **critical issues** - these affect the validity of your results
2. Address the **major issues** related to notation and figures
3. Work through the **minor issues** as time permits
4. Use the issue-tracker.md to mark items as resolved

Please let me know if you have questions about any of the feedback. I am happy to discuss specific issues in more detail.

Best regards,

[Your Name]

---

*Review conducted: 2026-01-21*
*Framework: ML Manuscript Technical Audit*