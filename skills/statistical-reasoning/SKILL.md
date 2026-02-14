---
name: statistical-reasoning
description: >
  Hypothesis testing, A/B testing, statistical significance, p-values, confidence intervals,
  sample size calculation, correlation vs causation, regression analysis. Use when validating
  A/B test results, determining statistical significance, calculating required sample sizes,
  building predictive models, or distinguishing correlation from causation in marketing data.
---

# Statistical Reasoning Skill

Use this skill when the task requires: A/B test validation, hypothesis testing, statistical significance determination, confidence interval calculation, sample size planning, correlation analysis, regression modeling, or any decision requiring statistical rigor.

**Critical for marketing:** Prevents premature optimization, validates campaign changes, ensures data-driven decisions are actually statistically sound.

---

## 1. HYPOTHESIS TESTING FUNDAMENTALS

### The Scientific Method for Marketing
Every A/B test or experiment follows this structure:

1. **Null Hypothesis (H0)**: No difference exists (e.g., "Variant B has same conversion rate as Control A")
2. **Alternative Hypothesis (H1)**: A difference exists (e.g., "Variant B has higher conversion rate than Control A")
3. **Significance Level (alpha)**: Acceptable false positive rate (typically 0.05 = 5%)
4. **Test Statistic**: Calculate from data
5. **P-value**: Probability of observing data if H0 is true
6. **Decision**: If p-value < alpha, reject H0 (difference is statistically significant)

### Critical Concept: Statistical Significance != Practical Significance
- **Statistical significance**: Difference is unlikely due to random chance
- **Practical significance**: Difference is large enough to matter for business

**Example:**
- A/B test shows Variant B has 4.12% conversion vs Control A's 4.08% conversion
- With 100K visitors, p-value = 0.03 (statistically significant!)
- But 0.04% absolute difference = minimal revenue gain vs implementation cost
- **Verdict**: Statistically significant, NOT practically significant. Don't implement.

---

## 2. P-VALUES AND SIGNIFICANCE LEVELS

### What is a P-Value?
**P-value** = Probability of observing results this extreme (or more) if null hypothesis is true

**Interpretation:**
- p < 0.01: Very strong evidence against H0 (1% chance of false positive)
- p < 0.05: Strong evidence against H0 (5% chance -- industry standard)
- p < 0.10: Moderate evidence (10% chance -- sometimes acceptable for early-stage tests)
- p >= 0.05: Insufficient evidence to reject H0 (could be due to sample size)

### Common Mistakes with P-Values

**WRONG**: "p = 0.03 means the hypothesis has 97% probability of being true"
**CORRECT**: "p = 0.03 means if there's no real difference, we'd see results this extreme only 3% of the time"

**WRONG**: "p = 0.06 proves the variants are identical"
**CORRECT**: "p = 0.06 means we don't have enough evidence to claim a difference (yet)"

**WRONG**: "Lower p-value = bigger effect"
**CORRECT**: "Lower p-value = more confidence difference exists, not necessarily larger difference"

### Significance Level (alpha) Selection

**Industry Standards:**
- alpha = 0.05 (95% confidence): Default for most marketing tests
- alpha = 0.01 (99% confidence): High-stakes decisions (pricing changes, major rebrands)
- alpha = 0.10 (90% confidence): Early-stage experiments, rapid iteration environments

**Recommendations:**
- Use alpha = 0.05 for most A/B tests (landing pages, ad copy, email subject lines)
- Use alpha = 0.01 for fee changes, brand positioning shifts, major CRM migrations
- Use alpha = 0.10 for quick creative tests when speed > precision

---

## 3. CONFIDENCE INTERVALS

### Definition
A confidence interval gives a range of plausible values for the true parameter.

**95% Confidence Interval Interpretation:**
"If we repeated this experiment 100 times, 95 of those intervals would contain the true value"

### Formula (for proportions)
```
CI = p_hat +/- Z * sqrt(p_hat(1-p_hat)/n)

Where:
p_hat = sample proportion (e.g., conversion rate)
Z = Z-score (1.96 for 95% confidence, 2.58 for 99%)
n = sample size
```

### Example: Landing Page Conversion Rate
- Sample: 1,000 visitors, 45 conversions
- Conversion rate: p_hat = 45/1000 = 0.045 (4.5%)
- 95% CI = 0.045 +/- 1.96 x sqrt(0.045 x 0.955/1000)
- 95% CI = 0.045 +/- 0.013
- **95% CI: [3.2%, 5.8%]**

**Interpretation:** We're 95% confident the true conversion rate is between 3.2% and 5.8%

### Why Confidence Intervals > Point Estimates
**Report this:**
- "Variant B conversion rate: 5.2% [95% CI: 4.1%, 6.3%]"
- "Control A conversion rate: 4.8% [95% CI: 3.9%, 5.7%]"
- **Verdict**: Confidence intervals overlap -> not statistically significant

**Not this:**
- "Variant B is better: 5.2% vs 4.8%"
- **Problem**: Ignores uncertainty, could be random variation

---

## 4. SAMPLE SIZE CALCULATION

### Why Sample Size Matters
- **Too small**: Can't detect real differences (Type II error - false negative)
- **Too large**: Wastes time/money, detects tiny meaningless differences

### Formula for A/B Tests (Proportions)
```
n = (Z_alpha/2 + Z_beta)^2 x [p1(1-p1) + p2(1-p2)] / (p1 - p2)^2

Where:
Z_alpha/2 = Z-score for significance level (1.96 for alpha=0.05)
Z_beta = Z-score for power (0.84 for 80% power)
p1 = baseline conversion rate
p2 = expected conversion rate after change
```

### Simplified Calculator Approach
**Use online calculators:** Evan's Awesome A/B Tools, Optimizely Sample Size Calculator

**Inputs needed:**
1. Baseline conversion rate (e.g., 4%)
2. Minimum Detectable Effect (MDE) -- smallest difference worth detecting (e.g., +20% relative = 4% -> 4.8%)
3. Significance level (alpha = 0.05)
4. Power (1-beta = 0.80 means 80% chance of detecting real effect)

### Example: Landing Page Test
**Setup:**
- Baseline conversion: 4%
- Want to detect: +25% relative improvement (4% -> 5%)
- Significance: alpha = 0.05
- Power: 80%

**Required sample size per variant:** ~3,900 visitors

**Timeline:**
- Traffic: 500 visitors/day
- Time needed: 3,900 / 500 = 8 days per variant = 16 days total

**Decision:** If you can't wait 16 days, either:
1. Accept lower power (70% instead of 80%) -> smaller sample needed
2. Only test larger changes (+50% = 4% -> 6%) -> smaller sample needed
3. Find higher-traffic page to test on

---

## 5. A/B TESTING BEST PRACTICES

### When to Stop a Test (Decision Framework)

**Criteria for calling a winner:**
1. Reached minimum sample size
2. p-value < 0.05 (or chosen alpha)
3. Practical significance (effect size matters for business)
4. Consistent over time (no day-of-week effects)

**Common Mistakes:**

**Peeking**: Checking results daily and stopping when p < 0.05
- **Problem**: Inflates false positive rate from 5% to 20%+
- **Solution**: Pre-commit to sample size, check only at milestones (25%, 50%, 75%, 100%)

**Multiple Testing**: Running 10 tests simultaneously at alpha=0.05
- **Problem**: Expect 0.5 false positives even if nothing works
- **Solution**: Use Bonferroni correction (alpha/n) or control False Discovery Rate

**Switching Metrics**: Starting with "clicks" then switching to "conversions" when convenient
- **Problem**: Cherry-picking favorable metrics inflates false positives
- **Solution**: Pre-define primary metric before test starts

### Sequential Testing (Advanced)
For high-traffic scenarios where you can't wait for full sample:

**Use Sequential Probability Ratio Test (SPRT)** or **Bayesian A/B testing**
- Allows valid peeking at interim results
- Adjusts significance thresholds dynamically
- Tools: Optimizely Stats Engine, VWO

---

## 6. CORRELATION VS. CAUSATION

### The Golden Rule
**Correlation measures association. Causation requires experimentation.**

### Classic Examples of Spurious Correlation
1. Ice cream sales correlate with drowning deaths
   - **Real cause**: Summer weather (confounding variable)

2. Ad spend correlates with revenue
   - **But**: Does ads cause revenue, or do successful businesses spend more on ads?
   - **Test**: Randomized experiment (geo-split, hold-out groups)

3. High engagement score correlates with enrollment
   - **But**: Engaged parents were already likely to enroll (selection bias)
   - **Test**: Randomly assign content, measure enrollment difference

### Bradford Hill Criteria for Causation
When you can't run randomized experiments, use these heuristics:

1. **Strength of association**: Larger correlation -> more likely causal
2. **Consistency**: Same finding across multiple studies/contexts
3. **Temporality**: Cause precedes effect (essential!)
4. **Dose-response**: More of cause -> more of effect
5. **Plausibility**: Mechanism makes sense
6. **Experiment**: Gold standard

### Testing for Causation in Marketing

**Option 1: Randomized Controlled Trial (RCT)**
- Randomly assign users to treatment/control
- Measure outcome difference
- **Example**: Randomly show 50% of leads Variant B landing page, 50% Control A

**Option 2: Quasi-Experiments (when randomization impossible)**
- **Difference-in-Differences**: Compare change in treatment group vs control group over time
- **Regression Discontinuity**: Compare outcomes just above/below threshold
- **Instrumental Variables**: Find variable that affects treatment but not outcome directly

**Option 3: Controlled Before-After Studies**
- Measure baseline period, implement change, measure after period
- Control for seasonality, external factors

---

## 7. REGRESSION ANALYSIS FOR MARKETING

### Simple Linear Regression
**Model**: Y = B0 + B1*X + error

**Use case**: Predict enrollment based on marketing spend

**Interpretation of B1 = 0.12:**
- For every 1L increase in marketing spend, expect 0.12 additional enrollments

**Key Metric: R-squared**
- R-squared = 0.65 means marketing spend explains 65% of variation in enrollments
- Remaining 35% due to other factors (seasonality, competition, word-of-mouth)

### Multiple Regression
**Model**: Y = B0 + B1*X1 + B2*X2 + ... + Bn*Xn + error

**Use case**: Predict CPA based on multiple factors

**Example:**
```
CPA = B0 + B1(GoogleSpend) + B2(MetaSpend) + B3(Seasonality) + B4(CompetitorActivity)
```

**Coefficient Interpretation:**
- B1 = 0.002: 1 unit increase in Google spend -> 0.002 increase in CPA (holding other variables constant)
- B3 = -0.15: Peak season -> 0.15 lower CPA vs off-season

### Regression Diagnostics (Check Before Trusting Model)

1. **Linearity**: Scatterplot shows linear relationship
2. **Independence**: Observations not autocorrelated (use Durbin-Watson test)
3. **Homoscedasticity**: Residual variance constant across X values
4. **Normality of residuals**: Q-Q plot shows normal distribution

**Red Flags:**
- R-squared > 0.95 in marketing data (likely overfitting or spurious correlation)
- Coefficient sign opposite of expectation (more spend -> lower enrollments?!)
- P-values all <0.001 (multicollinearity -- variables too correlated with each other)

### Logistic Regression (Binary Outcomes)
**Use case**: Predict enrollment probability (Yes/No)

**Model**: log(p/(1-p)) = B0 + B1*X1 + B2*X2 + ...

**Interpretation of coefficients:**
- B1 = 0.8: One unit increase in X1 -> odds of enrollment multiply by e^0.8 = 2.23 (123% increase)

**Output:** Probability score (0-1) for each lead
- Score >0.6 -> High priority for counselor follow-up
- Score <0.3 -> Deprioritize or nurture campaign

---

## 8. COMMON STATISTICAL TESTS FOR MARKETING

### Test Selection Guide

| Scenario | Test | Example |
|----------|------|---------|
| Compare 2 proportions (A/B test) | Z-test or Chi-square | Conversion rate: Variant A vs B |
| Compare 2 means | T-test | Average order value: Campaign A vs B |
| Compare 3+ groups | ANOVA | CPA across 5 cities |
| Association between 2 categorical variables | Chi-square | Channel vs Enrollment (Yes/No) |
| Correlation between 2 continuous variables | Pearson correlation | Marketing spend vs Revenue |
| Time series comparison | Time series analysis / ARIMA | Monthly enrollment trends |

### Two-Sample T-Test (Comparing Means)
**Use case:** Compare average cost per lead between Google Ads and Meta

**Assumptions:**
1. Both samples approximately normally distributed (or n>30)
2. Independent samples
3. Equal variances (or use Welch's t-test)

**Example:**
- Google Ads CPL: 1,200 (n=150 campaigns)
- Meta CPL: 850 (n=120 campaigns)
- t-statistic = 2.4, p-value = 0.018

**Conclusion:** p < 0.05 -> Meta has significantly lower CPL than Google Ads

### Chi-Square Test (Categorical Association)
**Use case:** Is enrollment rate different across lead sources?

**Contingency Table:**
|        | Enrolled | Not Enrolled | Total |
|--------|----------|--------------|-------|
| Google | 120      | 480          | 600   |
| Meta   | 85       | 515          | 600   |
| Events | 95       | 505          | 600   |

**Chi-square statistic:** X2 = 5.8, df=2, p = 0.055

**Conclusion:** p > 0.05 -> No significant difference in enrollment rate by source (marginal, might need larger sample)

### ANOVA (Comparing 3+ Groups)
**Use case:** Compare CPA across 5 cities

**Null hypothesis:** All cities have same mean CPA
**Alternative:** At least one city differs

**If p < 0.05:** Reject null -> cities differ
**Follow-up:** Post-hoc tests (Tukey HSD) to identify which city pairs differ

---

## 9. BAYESIAN A/B TESTING (ALTERNATIVE APPROACH)

### Bayesian vs Frequentist
**Frequentist (traditional):**
- "What's the probability of seeing this data if there's no difference?"
- Binary decision: significant or not
- Requires pre-defined sample size

**Bayesian:**
- "What's the probability Variant B is better than Control A?"
- Continuous probability: 73% chance B is better
- Can stop test anytime (valid peeking)

### When to Use Bayesian
- High-traffic websites (can't wait for full sample)
- Business needs interim results
- Want probability statements ("85% sure Variant B wins")

### Bayesian Output Interpretation
**Probability B > A: 94%**
- Strong evidence Variant B is better
- Okay to implement if business accepts 6% risk

**Probability B > A: 68%**
- Weak evidence, likely need more data
- Don't implement yet

**Expected Loss if Wrong:**
- "If we choose B and it's actually worse, we expect to lose 0.3% conversion"
- Helps quantify business risk

### Tools for Bayesian A/B Testing
- VWO (Visual Website Optimizer)
- Optimizely Stats Engine
- Python: PyMC3, ArviZ libraries

---

## 10. DECISION FRAMEWORKS

### A/B Test Validation Protocol
**Before starting test:**
1. Define primary metric (e.g., enrollment rate, not clicks)
2. Calculate required sample size
3. Set significance level (alpha=0.05 default)
4. Document expected effect size
5. Estimate test duration

**During test:**
- No peeking until 50% sample reached
- Check sample ratio (should be 50/50 in randomized test)
- Monitor for technical issues (tracking errors, bot traffic)

**After reaching sample size:**
1. Check p-value < alpha
2. Check confidence intervals don't overlap
3. Verify practical significance (effect size worth implementing)
4. Check consistency across segments (mobile vs desktop, new vs returning)
5. If all pass -> Declare winner and implement

**If test is inconclusive:**
- p > 0.05 after full sample -> either no difference or need more power
- Calculate achieved power: "With this sample, we could detect >=X% difference"
- Decision: Run longer OR accept no significant difference exists

### Statistical Significance Quick Reference
**For A/B tests (proportions):**

| Sample Size per Variant | Baseline Rate | Minimum Detectable Effect (alpha=0.05, power=80%) |
|-------------------------|---------------|---------------------------------------------------|
| 1,000                   | 4%            | +50% (4% -> 6%)                                   |
| 5,000                   | 4%            | +22% (4% -> 4.9%)                                 |
| 10,000                  | 4%            | +16% (4% -> 4.6%)                                 |
| 50,000                  | 4%            | +7% (4% -> 4.3%)                                  |

**Implication:** Small sample -> can only detect large effects. Large sample -> can detect small effects (but may not be practically significant).

### Correlation Analysis Protocol
**When analyzing marketing metrics correlations:**

1. **Calculate Pearson correlation coefficient (r)**
   - r = 1: Perfect positive correlation
   - r = 0: No correlation
   - r = -1: Perfect negative correlation

2. **Test for significance**
   - H0: rho = 0 (no correlation in population)
   - If p < 0.05 -> correlation is statistically significant

3. **Check for causation red flags**
   - Could there be a confounding variable?
   - Does the correlation make logical sense?
   - Is the relationship consistent over time?

4. **If causation suspected, design experiment**
   - Randomize treatment assignment
   - Control for confounding variables
   - Measure outcome difference

---

## 11. COMMON PITFALLS & HOW TO AVOID THEM

### Pitfall 1: P-Hacking (Data Dredging)
**What it is:** Running many tests until one shows p < 0.05, then reporting only that one

**How to avoid:**
- Pre-register hypothesis before data collection
- Use Bonferroni correction: alpha_adjusted = 0.05/n
- Report all tests performed, not just significant ones

### Pitfall 2: Sample Size Too Small
**What it is:** Declaring "no difference" when test was underpowered to detect real effects

**How to avoid:**
- Calculate required sample size before starting
- If inconclusive, run power analysis
- Report confidence intervals to show uncertainty

### Pitfall 3: Confusing Statistical and Practical Significance
**What it is:** Implementing tiny improvements just because they're statistically significant

**How to avoid:**
- Pre-define Minimum Detectable Effect (MDE) based on business impact
- Calculate ROI of implementation before declaring winner
- Ask: "If this is the true effect size, is it worth implementing?"

### Pitfall 4: Ignoring Seasonality
**What it is:** Comparing metrics across different time periods without adjusting for seasonal patterns

**How to avoid:**
- Use year-over-year comparisons
- Include seasonality control in regression models
- Run concurrent A/B tests (randomized at same time)

### Pitfall 5: Survivorship Bias
**What it is:** Only analyzing data from users who completed the funnel, ignoring dropouts

**How to avoid:**
- Include full denominator in analysis
- Analyze funnel drop-off at each stage
- Consider selection bias when evaluating effectiveness

---

## 12. TOOLS & SOFTWARE

### Statistical Calculators (No Code)
- **Evan's Awesome A/B Tools**: Sample size, significance testing
- **Optimizely Sample Size Calculator**: Pre-test planning
- **GraphPad QuickCalcs**: T-tests, Chi-square
- **VassarStats**: Comprehensive statistical tests

### Spreadsheet Tools
**Google Sheets / Excel Functions:**
```
=CHISQ.TEST(actual_range, expected_range)  // Chi-square test
=T.TEST(array1, array2, tails, type)       // T-test
=CONFIDENCE.NORM(alpha, stdev, size)       // Confidence interval
=CORREL(array1, array2)                    // Correlation
=FORECAST.LINEAR(x, known_y, known_x)     // Linear regression
```

### Python Libraries
```python
import scipy.stats as stats
from statsmodels.stats.power import zt_ind_solve_power

# T-test
t_stat, p_value = stats.ttest_ind(group_a, group_b)

# Chi-square
chi2, p_value = stats.chi2_contingency(contingency_table)

# Correlation
r, p_value = stats.pearsonr(x, y)

# Sample size calculation
sample_size = zt_ind_solve_power(effect_size=0.2, alpha=0.05, power=0.8)
```

### R Packages
```r
# T-test
t.test(group_a, group_b)

# Chi-square
chisq.test(table)

# ANOVA
aov(outcome ~ factor, data=df)

# Regression
lm(y ~ x1 + x2, data=df)
```

---

## 13. REFERENCE FORMULAS & CRITICAL VALUES

### Z-Scores (Standard Normal Distribution)
| Confidence Level | Z-Score |
|------------------|---------|
| 90%              | 1.645   |
| 95%              | 1.96    |
| 99%              | 2.576   |

### T-Distribution Critical Values (Two-Tailed, alpha=0.05)
| df  | t-critical |
|-----|------------|
| 10  | 2.228      |
| 20  | 2.086      |
| 30  | 2.042      |
| 50  | 2.009      |
| 100 | 1.984      |
| inf | 1.96       |

### Effect Size Interpretation (Cohen's d)
| d     | Interpretation |
|-------|----------------|
| 0.2   | Small          |
| 0.5   | Medium         |
| 0.8   | Large          |

### Correlation Strength
| |r|     | Interpretation  |
|---------|-----------------|
| 0-0.3   | Weak            |
| 0.3-0.7 | Moderate        |
| 0.7-1.0 | Strong          |

---

## 14. DECISION RULES

### When to Call an A/B Test Winner (All Must Be True)
1. Sample size >= pre-calculated minimum
2. p-value < 0.05
3. Absolute improvement >= 0.5% conversion OR meaningful CPA reduction
4. Confidence intervals don't overlap
5. Effect consistent across device types (mobile vs desktop)
6. ROI positive: (Annual benefit - Implementation cost) > 0

### Statistical Significance Thresholds by Decision Type
| Decision Type | Required alpha | Rationale |
|---------------|---------------|-----------|
| Ad copy test | 0.10 | Low risk, fast iteration |
| Landing page change | 0.05 | Standard test |
| Fee increase | 0.01 | High stakes, need high confidence |
| Brand repositioning | 0.001 | Critical strategic decision |

### Minimum Sample Sizes for Common Tests
| Test Type | Baseline | Target MDE | Required n per Variant |
|-----------|----------|------------|------------------------|
| Landing page | 4% conv | +25% (->5%) | 3,900 |
| Email subject | 20% open | +15% (->23%) | 2,200 |
| Ad creative | 2% CTR | +50% (->3%) | 3,800 |
| Pricing (fee) | 70% acceptance | -10% (->63%) | 1,900 |

**If you can't reach minimum sample size:**
- Test on higher-traffic page
- Test larger effect sizes only
- Accept lower power (70% instead of 80%)
- Use Bayesian methods for early stopping
