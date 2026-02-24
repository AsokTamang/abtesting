# 🧪 Hypothesis Testing – A/B Test on Website Background Color

## 📌 Overview

This project performs a **two-sample t-test** to analyze the results of an A/B experiment that tested whether changing a website's background color increased average user session duration.

---

## 📂 Dataset

**File-Link:** `https://drive.google.com/file/d/1LmP8kcYNBWpujYM5puIDjUOzMMySDse2/view?usp=sharing`

| Column | Description |
|---|---|
| `user_id` | Unique identifier for each user |
| `user_type` | Either `control` (old website) or `variation` (new website) |
| `session_duration` | Time spent on the website (in minutes) |

- **Total records:** 4,186 users
- **Control group:** Users who used the original website
- **Variation group:** Users who used the redesigned website

---

##  What Was Done

### 1. Data Loading & Exploration
- Loaded the CSV dataset using `pandas`
- Separated users into **control** and **variation** groups
- Computed the average session duration for each group:
  - Old website (control): **~32.92 min**
  - New website (variation): **~33.83 min**

### 2. Descriptive Statistics
- Calculated the **sample size (n)**, **mean (x̄)**, and **sample standard deviation (s)** for both groups using a custom `getstats()` function with `ddof=1` for an unbiased standard deviation estimate.

### 3. Degrees of Freedom (Welch's Formula)
- Since the two groups may have different variances, **Welch's t-test** was used instead of the standard Student's t-test.
- Implemented the **Welch–Satterthwaite equation** to calculate the degrees of freedom (~4182.97).

### 4. T-Statistic Calculation
- Implemented a custom `t_value()` function to compute the t-statistic.
- **Result:** t ≈ **1.64**

### 5. P-Value Calculation
- Used `scipy.stats.t` to get the t-distribution CDF and computed the **one-tailed p-value** (right tail), since the hypothesis is that the new website performs *better*.

### 6. Decision Making
- Implemented a `make_decision()` function to compare the p-value against different significance levels (α):

| Alpha (α) | Decision |
|---|---|
| 0.06 | ✅ Reject H₀ |
| 0.05 | ❌ Do not reject H₀ |
| 0.04 | ❌ Do not reject H₀ |
| 0.01 | ❌ Do not reject H₀ |

---

## 🧠 What Was Learned

- **Hypothesis Testing Framework:** How to formally set up a null hypothesis (H₀: no difference) and an alternative hypothesis (H₁: new website has longer sessions).
- **Welch's T-Test:** When to prefer Welch's t-test over a standard t-test — especially when the two groups have unequal variances or sample sizes.
- **Degrees of Freedom:** How the Welch–Satterthwaite equation approximates an effective degrees of freedom for unequal variance scenarios.
- **P-Value Interpretation:** How the p-value represents the probability of observing the data if H₀ were true, and how it changes with different significance thresholds.
- **Statistical vs Practical Significance:** A mean difference of ~0.9 minutes exists, but it is only statistically significant at a relaxed α of 0.06 — meaning we need stronger evidence to confidently claim the new design works.

---

## ❓ What Problem It Solved

> **"Did changing the website's background color actually improve user engagement?"**

The company ran an A/B experiment and observed that users on the new website spent slightly more time on average. However, **observing a difference in sample means is not enough** — it could be due to random chance.

This project applied **hypothesis testing** to determine whether the observed difference is statistically meaningful. The conclusion: at the standard significance level of **α = 0.05**, we **cannot reject the null hypothesis**, meaning the evidence is insufficient to confidently claim the new design increases session duration. A larger sample or a more impactful design change may be needed.

---

## 📦 Dependencies

```bash
pip install numpy pandas scipy
```

---

## 🚀 How to Run

1. Place `background_color_experiment.csv` inside a `data/` folder.
2. Open `Hypothesis_Testing.ipynb` in Jupyter Notebook or JupyterLab.
3. Run all cells sequentially.

---

## 📁 Project Structure

```
├── data/
│   └── background_color_experiment.csv
├── Hypothesis_Testing.ipynb
└── README.md
```
