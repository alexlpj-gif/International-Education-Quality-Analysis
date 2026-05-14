# International-Education-Quality-Analysis
What actually drives educational outcomes across nations? A multivariate statistical deep-dive into 145 countries across three decades — uncovering how government spending, teacher resources, and school structure shape the quality of education.

# 📊 International Education Quality Analysis (1960–1990)

> **What actually drives educational outcomes across nations?**  
> A multivariate statistical deep-dive into 145 countries across three decades — uncovering how government spending, teacher resources, and school structure shape the quality of education.

---

## 🔍 Project Overview

Education policy decisions affect millions of students, yet the relationships between resource allocation and actual outcomes are rarely quantified with rigor. This project analyzes the **Barro-Lee International Schooling Quality dataset** to identify which factors most strongly predict educational quality — and which conventional assumptions don't hold up under scrutiny.

**Key insight:** Government expenditure on education explains **96% of variance in education quality scores** (SEM coefficient = 0.96), and lower pupil-teacher ratios are directly linked to reduced dropout rates — a finding with direct implications for how policymakers should prioritize education budgets.

---

## ❓ Business / Research Questions

1. Do pupil-teacher ratios in primary schools (1970–1990) meaningfully impact educational quality — and by how much?
2. What is the quantified relationship between government expenditure and key student outcomes (dropout rates, repetition rates)?

---

## 📁 Dataset

| Attribute | Detail |
|-----------|--------|
| **Source** | Barro-Lee International Measures of Schooling Quality (World Bank Microdata) |
| **Coverage** | 145 countries, 1960–1990 (quinquennial — every 5 years) |
| **Key Variables** | Pupil-teacher ratios, government expenditure per pupil, teacher salaries, dropout rates, repetition rates |
| **Observations** | ~120 after preprocessing (outlier removal via Z-score method) |

---

## 🧪 Methodology

This project follows a structured, layered analytical pipeline — each method chosen to answer a specific question rather than applied mechanically.

### 1. Data Preprocessing
- Removed missing values and duplicates
- Excluded secondary school variables (out of scope for primary research questions)
- **Outlier treatment:** Z-score method (threshold: |z| > 3), chosen for its consistency and reproducibility across comparisons
- Standardized all variables (mean = 0, SD = 1) to ensure equal contribution in multivariate analyses

### 2. Principal Component Analysis (PCA)
- **Why:** Reduce dimensionality of 30+ correlated time-series variables before modeling
- PC1 explains **31.8%** of total variance; PC1 + PC2 combined explain **54.2%**
- Key finding: Teacher salaries (SALARP70–90) and government expenditure cluster strongly on PC1, indicating these are the primary drivers of educational variation

### 3. Factor Analysis (FA)
- **Why:** Identify latent constructs underlying the observed variables
- KMO = 0.76 → dataset suitable for FA
- Retained **4 factors** (parallel analysis method, more conservative than Kaiser criterion's 7)
- MR1 = Government expenditure + teacher salaries | MR3 = Dropout rates | MR4 = Repetition rates + pupil-teacher ratios

### 4. Clustering Analysis (CA)
- Tested 3 distance metrics: Euclidean (k-means), Manhattan (PAM), Cosine (hierarchical)
- **Chose hierarchical clustering with Cosine distance** — best captures directional similarity in high-dimensional data
- Silhouette method identified **2 optimal clusters**:
  - **Cluster 1 (High-performing):** High gov't expenditure, high teacher salaries, low pupil-teacher ratios, low dropout rates
  - **Cluster 2 (Underperforming):** Opposite profile across all metrics

### 5. Structural Equation Modeling (SEM)
- CFA confirmed reliability of 4 latent constructs (Cronbach's α: 0.85–0.95)
- SEM revealed:
  - Government expenditure → Education quality: **β = 0.96** (strong positive)
  - Teacher performance → Student outcomes: **β = 0.53** (positive)
  - Education quality → Student outcomes: β = -0.16 (counterintuitive; warrants further investigation)

---

## 💡 Key Findings

| Finding | Evidence |
|---------|----------|
| Government spending is the strongest predictor of education quality | SEM β = 0.96 between expenditure and education quality |
| Higher teacher salaries correlate with increased government education spending | High co-loading on FA factor MR1 (loadings 0.7–0.9) |
| Lower pupil-teacher ratios → lower dropout rates | High correlation on FA factor MR4; supported by cluster analysis |
| Countries cluster clearly into high vs. low performing groups | Silhouette-validated 2-cluster solution; primary separator = teacher salary + gov't expenditure |
| School time variables (SCHDAY, SCHTIME) show weak factor loadings | Suggests quantity of instruction time is less predictive than resource quality |

---

## 🛠️ Technical Stack

| Tool | Purpose |
|------|---------|
| **R** | Primary analysis environment |
| `FactoMineR` / `factoextra` | PCA and visualization |
| `psych` | Factor Analysis, KMO test, parallel analysis |
| `cluster` | k-means, PAM clustering |
| `lavaan` | Confirmatory Factor Analysis + SEM |
| `semPlot` | SEM path diagram visualization |
| `dplyr` / `tidyverse` | Data manipulation |

---

## 📂 Repository Structure

```
education-quality-analysis/
│
├── data/
│   └── QUALITY1.csv              # Source dataset (Barro-Lee)
│
├── scripts/
│   ├── 01_preprocessing.R        # Data cleaning & standardization
│   ├── 02_pca.R                  # Principal Component Analysis
│   ├── 03_factor_analysis.R      # Factor Analysis
│   ├── 04_clustering.R           # Cluster Analysis (k-means, PAM, hierarchical)
│   └── 05_sem.R                  # CFA + Structural Equation Modeling
│
├── output/
│   └── figures/                  # All plots (scree plot, biplot, dendrogram, SEM path)
│
└── README.md
```

---

## ▶️ How to Run

```r
# 1. Install required packages (first time only)
install.packages(c("dplyr", "ggplot2", "FactoMineR", "factoextra",
                   "psych", "cluster", "lavaan", "semPlot",
                   "GPArotation", "tidyverse", "car", "MASS"))

# 2. Run scripts in order
source("scripts/01_preprocessing.R")
source("scripts/02_pca.R")
source("scripts/03_factor_analysis.R")
source("scripts/04_clustering.R")
source("scripts/05_sem.R")
```

> 💡 Each script is self-contained and includes inline comments explaining the analytical rationale — not just the code.

---

## 📌 Limitations & Future Directions

- The negative SEM coefficient between education quality and student outcomes (β = -0.16) is counterintuitive and may reflect **model specification issues** or **omitted variable bias** — a key area for future refinement
- Dataset covers 1960–1990; findings may not generalize to post-2000 educational contexts
- Future work should incorporate **socioeconomic background variables** (family income, parental education) and **psychological factors** (student motivation, wellbeing) to build a more complete model

---

## 👤 Author

**Alexander Lau Poung Jie**  
Statistics — National Chengchi University  
📅 June 2024

---

*Dataset source: Jong-Wha Lee and Robert J. Barro. International Measures of Schooling Years and Schooling Quality (SYSQ) 1960–1990. World Bank Microdata Library.*
