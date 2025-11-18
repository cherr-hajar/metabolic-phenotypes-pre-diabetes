# Methodological Decisions Log

## Data Preprocessing

### Decision 1: Variable Selection
**Decision:** Use fasting glucose (LBXGLU), triglycerides (LBXTLG), and waist circumference (BMXWAIST)

**Rationale:**
- Readily available in clinical practice
- Represent three distinct metabolic domains (glucose, lipid, adiposity)
- Low correlation (r < 0.35) → independent information
- Well-established clinical thresholds for validation

**Alternatives Considered:**
- HbA1c: Not available in NHANES 2021-2023
- HDL cholesterol: Decided to focus on triglycerides (ATP III component)
- BMI: Chose waist circumference (better predictor of metabolic risk)

---

### Decision 2: Outlier Handling
**Decision:** RETAIN all outliers; use RobustScaler to minimize influence

**Rationale:**
- Low correlations suggest outliers may represent genuine subtypes
- NHANES uses quality-controlled laboratory data
- DBSCAN naturally handles outliers
- Clinical extremes (N = [X]) are valid measurements

**Alternatives Considered:**
- Remove IQR outliers: Would lose N = [X] participants (not justified)
- Winsorize: Would distort true distributions
- Remove Mahalanobis outliers: May remove genuine rare phenotypes

**Evidence:**
- Outliers represent [X]% of sample
- Removing outliers changed means by <5%
- Sensitivity analysis planned (Step X)

---

### Decision 3: Scaling Method
**Decision:** RobustScaler (primary); StandardScaler (sensitivity check)

**Rationale:**
- RobustScaler uses median/IQR → robust to skewness (triglycerides skew = [X])
- Preserves relative point spacing
- Comparable to StandardScaler for near-normal distributions
- No assumptions about normality required

**Evidence:**
| Variable | Original Skew | RobustScaler Skew | StandardScaler Skew |
|----------|---------------|-------------------|---------------------|
| Glucose | [X] | [Y] | [Y] |
| Triglycerides | [X] | [Y] | [Y] |
| Waist | [X] | [Y] | [Y] |

**Alternatives Considered:**
- StandardScaler: Would be more sensitive to outliers
- MinMaxScaler: Too sensitive to extreme values
- PowerTransformer: Reduces skewness but loses interpretability

---

### Decision 4: Dimensionality Reduction
**Decision:** DO NOT use PCA for dimensionality reduction; keep all 3 original variables

**Rationale:**
- Only 3 variables (already low-dimensional)
- PC1 explains [X]% < 70% → low redundancy
- PC1+PC2 explain [X]% < 85% → would lose information in PC3
- Clinical interpretability: easier to explain "high glucose" than "PC1 loading"

**Evidence:**
- Correlation matrix: max r = 0.315
- VIF: Glucose = 12.13, Triglycerides = 3.26, Waist = 11.60
  - Elevated VIF likely due to age/BMI confounding, not direct multicollinearity
- Partial correlations (controlling age/sex): minimal change (Δr < 0.05)

**Use of PCA:**
- Visualization only (2D/3D plots)
- Exploratory assessment of data structure
- NOT used for clustering input

---

