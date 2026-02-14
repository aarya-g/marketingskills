---
name: quantitative-analytical-toolkit
description: >
  Distilled quantitative methods from Penn State Smeal courses: SCM 421 (Fermi estimation,
  modeling), SCM 301 (capacity, operations), FIN 408 (financial modeling, unit economics),
  BA 411 (business simulation, competitive dynamics), MIS 441/446/479W (process integration,
  digital value, data mining, visualization), Econ 102 (micro-economics, elasticity). Use when
  performing market sizing, financial modeling, capacity planning, unit economics analysis,
  Fermi estimation, process optimization, predictive modeling, data visualization, or any task
  requiring structured quantitative reasoning.
---

# Quantitative Analytical Toolkit Skill

Use this skill when the task requires: market sizing, Fermi estimation, unit economics
calculation, financial modeling, capacity analysis, pricing optimization, process design,
predictive modeling, data visualization, or structured quantitative reasoning under uncertainty.

Source files (archived to Notion):
- SCM 421: Craft of Modeling I -- Fermi Problems and Dimensional Analysis
- SCM 301: Operations and Supply Chain Management
- FIN 408: Financial Markets, Institutions, and Bank Management
- BA 411: Business Simulation and Competitive Dynamics
- MIS 441/446/479W: Information Systems, Digital Value, Enterprise Integration, Data Mining, Visualization
- Econ 102: Introductory Microeconomics

---

## 1. FERMI ESTIMATION & MODELING FRAMEWORK (SCM 421)

### The Modeling Framework
Every analysis follows: **Inputs -> Logic -> Outputs**
- Inputs: assumptions, constraints, constants (all explicit, never hard-coded)
- Logic: formulas connecting inputs to outputs
- Outputs: metrics driving recommendations

### Fermi Problem Method
**When to use:** Data is unavailable, quick order-of-magnitude judgment needed, sanity-checking claims in meetings, case interviews, market sizing.

**Steps:**
1. Define the target quantity clearly (with units)
2. Identify component sub-questions
3. Make common-sense assumptions for each component (state explicitly)
4. Multiply/add as appropriate, noting units at each step
5. Round to one significant figure

**Example -- Market sizing addressable families in Bangalore:**
- Bangalore population: ~13M
- Households: ~3.5M (avg 3.7 per household)
- Households with school-age children (5-18): ~35% = ~1.2M
- Income bracket affording 2L+/year fees: ~15% = ~180K
- Considering private English-medium: ~60% = ~108K
- In premium segment (4L+): ~25% = ~27K families
- Result: ~25,000-30,000 addressable premium families in Bangalore

### Dimensional Analysis
**Core principle:** Treat units like algebra -- chain conversion ratios so unwanted units cancel.

**Use when:** Converting between metrics, verifying formulas make sense, building cost models.

**Example:**
- Cost per lead x Leads per enrollment = Cost per enrollment
- 500/lead x 25 leads/enrollment = 12,500 CPA
- If Year 1 fee = 5L, then CPA as % of fee = 12,500/500,000 = 2.5%

### Estimation Levels (Time-Based)
| Level | Time | Use Case |
|-------|------|----------|
| Smell test | 60 seconds | "Is this claim in the right ballpark?" |
| Back-of-envelope | 5-15 minutes | Meeting prep, feasibility check |
| 1-day model | 1 day | Budget proposal, initial recommendation |
| 7-day deep dive | 1 week | Board presentation, strategy decision |
| Full model | 1 month | Annual planning, major investment |

### Modeling Rules
- **Never hard-code numbers** in formulas -- every assumption in a reference cell
- **Modularize**: each logical piece in its own section/tab
- **Simplicity first**: simpler models often work better; complexity later
- **Every assumption must be explicit** and justifiable
- **Idiot-check**: use estimation to sanity-check any spreadsheet output

---

## 2. UNIT ECONOMICS & FINANCIAL MODELING (FIN 408 + BA 411)

### Core Formulas

**DuPont Decomposition (ROE)**
```
ROE = ROS x Asset Turnover x Leverage
    = (Net Income / Sales) x (Sales / Assets) x (Assets / Equity)
```
- Use when: diagnosing whether low profitability stems from margin, productivity, or financing
- High ROE from high leverage != better performance -- check solvency risk

**Break-Even Analysis**
```
Break-Even Units = Fixed Costs / (Price - Variable Cost per Unit)
Contribution Margin = Price - Variable Cost
CM Ratio = CM / Price
```
- Example: Campus fixed costs 2Cr, avg fee 4L, variable cost 1.5L -> Break-even = 200/2.5 = 80 students

**Return on Investment**
```
ROI = (Gain from Investment - Cost) / Cost
```
- Example: Marketing spend 22L generates 15 new enrollments at 4L avg fee = 60L revenue -> ROI = (60-22)/22 = 1.73x

**Payback Period**
```
Payback = Investment / Annual Cash Flow from Investment
```
- Example: CRM system 15L, saves 5L/year in counselor efficiency -> 3 years payback

**Customer Lifetime Value (LTV)**
```
LTV = Average Annual Fee x Average Tenure (years) x Gross Margin %
```
- Example: 5L fee x 8 years x 40% margin = 16L LTV

**LTV:CAC Ratio**
```
Target: >= 3:1 (good), 5:1 (better), 7:1 (best)
Premium schools target: 4:1 to 8:1
```
- Example: LTV 16L, CAC 2L -> Ratio = 8:1 (excellent)

**CPA as % of Year 1 Fee**
```
CPA % = Total Marketing Cost per Enrollment / Year 1 Fee x 100
Target: < 2%
```

### Cash Conversion Cycle
```
CCC = Days Inventory Outstanding + Days Sales Outstanding - Days Payables Outstanding
```
- For schools: relevant as "time from lead to first fee payment"
- Lead -> Visit -> Application -> Assessment -> Admission -> Enrolled -> First Payment

### Liquidity Ratios
```
Current Ratio = Current Assets / Current Liabilities
Quick Ratio = (Current Assets - Inventory) / Current Liabilities
```

### Time Value of Money
```
FV = PV x (1 + r)^t
PV = FV / (1 + r)^t
Annuity PV = PMT x [(1 - (1+r)^-t) / r]
```
- Use when: evaluating multi-year enrollment revenue streams, campus investment decisions

---

## 3. OPERATIONS & CAPACITY ANALYSIS (SCM 301)

### Capacity Planning
```
Utilization = Actual Output / Design Capacity
Efficiency = Actual Output / Effective Capacity
```
- **For campuses**: Utilization = Current Enrollment / Maximum Capacity
- Low utilization (<70%) = marketing problem or over-built capacity
- High utilization (>90%) = need expansion or waitlist management

### Forecasting Methods
- **Moving Average**: Average of last N periods (simple, smooths noise)
- **Weighted Moving Average**: Recent periods weighted more heavily
- **Exponential Smoothing**: Forecast = a(Actual) + (1-a)(Previous Forecast)
  - Low a (0.1-0.3): stable demand, smooth forecasts
  - High a (0.7-0.9): volatile demand, responsive forecasts

**For enrollment forecasting:**
- Use 3-year moving average for stable markets
- Use exponential smoothing (a=0.5-0.7) for new campuses with volatile enrollment

### Inventory/Pipeline Management
**Applied to enrollment funnel:**
```
Safety Stock equivalent = Buffer leads above expected conversion
Reorder Point = Lead time demand + Safety stock
Lead Time = Average days from lead to enrollment
```
- If average conversion takes 45 days and you need 50 enrollments/month at 5% conversion, you need 1,000 leads/month entering pipeline at least 45 days before target

### Process Metrics
```
Throughput = Units processed per time period
Cycle Time = Time for one unit to pass through entire process
Bottleneck = Slowest step in the process
```
- **Applied to enrollment**: If campus visits are the bottleneck (capacity 10/day, need 200/month), then visit scheduling is the constraint to solve

### S&OP (Sales & Operations Planning)
- Align enrollment targets (demand) with campus capacity (supply)
- Aggregate planning: how many counselors, open house events, campus tours needed per quarter
- Match marketing spend to enrollment capacity -- no point generating leads a campus can't process

---

## 4. MICROECONOMIC REASONING (ECON 102)

### Price Elasticity of Demand
```
Ed = % Change in Quantity Demanded / % Change in Price
```
- |Ed| > 1: Elastic (price sensitive) -> lowering price increases revenue
- |Ed| < 1: Inelastic (price insensitive) -> raising price increases revenue
- |Ed| = 1: Unit elastic -> revenue maximized

**For K-12 premium schools:**
- Budget segment (0.77-2L): likely elastic -- parents compare heavily on price
- Mid-tier (2-4L): moderate elasticity -- balance of price and quality
- Premium (4-6.6L): likely inelastic -- parents paying for brand/outcomes, less price-sensitive

### Supply & Demand Framework
- Demand shifters: income growth, population growth, competitor entry, parent preferences
- Supply shifters: new campus openings, seat capacity changes, regulatory constraints
- Equilibrium: where enrollment demand meets available seats at a given fee level

### Market Structure
- **Monopolistic competition**: Most K-12 markets -- many differentiated competitors, low barriers
- **Oligopoly**: Some premium micro-markets -- 3-5 dominant players
- Pricing power comes from differentiation (curriculum, outcomes, brand) not from market dominance

### Opportunity Cost
- Every marketing rupee spent on Google Ads is a rupee NOT spent on referral programs
- Every campus tour slot given to a cold lead is a slot NOT available for a warm referral
- Decision framework: compare marginal return across channels before allocating

---

## 5. BUSINESS SIMULATION & COMPETITIVE DYNAMICS (BA 411)

### Strategy-Performance Linkage
```
Strategic Intent -> Functional Execution -> Financial Results -> Continuous Improvement
```
- Use SWOT to frame: SO (exploit), WO (correct), ST (resist), WT (defend)
- Every functional decision (R&D, Marketing, Production, Finance) must link back to strategic intent

### Assertion-Evidence Communication
- Every slide/chart needs: **Assertion headline** (complete sentence takeaway) + **Evidence visual** (chart proving it)
- Minimal text -- data drives narrative
- Use when: presenting to CMO/CFO/CEO

### Competitive Benchmarking
- Compare across: market share, product positioning, pricing, capacity utilization, financial ratios
- Track ratio trends over time, not just snapshots

---

## 6. DIGITAL VALUE & PROCESS INTEGRATION (MIS 441/446/479W)

### Business Model Canvas Application
- Customer value proposition, revenue model, capabilities, growth model
- Use when: evaluating new brand launch or market entry

### Value Creation vs. Value Capture
- Creating value != capturing value
- Schools create educational value; they capture it through fees, retention, referrals
- Value capture fails when: pricing doesn't match perceived value, or competitors offer similar at lower price

### Network Effects & Platform Thinking
- Parent referral networks = network effects in education
- Each satisfied parent increases the value of the school brand for future parents
- Alumni networks as long-term value multipliers

### ERP/Process Integration Logic (MIS 479W)
- End-to-end process: Lead -> Enrollment -> Student Management -> Fee Collection -> Alumni
- Integration points: CRM <-> Campus Management <-> Finance <-> Marketing
- "What gets measured gets done" -- KPIs must span the full process, not just marketing funnel top

---

## 7. DATA MINING & PREDICTIVE ANALYTICS (MIS 441 - RapidMiner)

**When to use:** Building predictive models without coding, lead scoring, churn prediction, segmentation, automated A/B test analysis.

### Core Workflows

**1. Lead Scoring Model**
- **Input**: Historical leads with enrollment outcome (Yes/No)
- **Features**: Source, city, campus visit (Y/N), email engagement score, form completion time
- **Algorithm**: Decision Tree or Random Forest
- **Output**: Probability score for each new lead (0-100%)
- **Use**: Prioritize high-probability leads for counselor follow-up
- **Expected Impact**: Increase conversion rate from 3-5% to 6-8% by focusing on qualified leads

**2. Enrollment Forecasting**
- **Input**: 3-5 years monthly enrollment data + external factors (competitor openings, fee changes, economic indicators)
- **Algorithm**: Time series (ARIMA) or Gradient Boosted Trees
- **Output**: Next 12 months enrollment forecast with confidence intervals
- **Use**: Budget planning, capacity allocation, hiring decisions
- **Expected Impact**: Reduce forecast error from +/-30% to +/-10%

**3. Student Churn Prediction**
- **Input**: Student demographics, grades, attendance, parent engagement, fee payment patterns
- **Features**: Attendance drops, grade trends, delayed payments, parent contact frequency, sibling enrollment
- **Algorithm**: Logistic Regression or Neural Network
- **Output**: Churn risk score (High/Medium/Low) for each student
- **Use**: Proactive retention campaigns before student leaves
- **Expected Impact**: Reduce churn from 8% to 5% annually

**4. Parent Segmentation (Clustering)**
- **Input**: Parent behavior data (website visits, open house attendance, email engagement, fee tier, location)
- **Algorithm**: K-means or Hierarchical Clustering
- **Output**: 3-5 distinct parent personas
- **Use**: Personalized marketing campaigns per segment
- **Expected Impact**: Increase campaign ROI by 40% through targeted messaging

**5. Campaign Performance Analysis**
- **Input**: Campaign data (spend, impressions, clicks, leads, enrollments) by channel
- **Process**: Automated cross-validation, feature importance analysis
- **Output**: Which channels drive highest quality leads? Which creative elements matter?
- **Use**: Budget reallocation, creative optimization
- **Expected Impact**: Reduce CPA by 25% through optimal channel mix

### RapidMiner Best Practices

**Model Development Process:**
1. Data preparation: Clean missing values, remove outliers, normalize features
2. Feature engineering: Create derived features (e.g., days_since_last_contact)
3. Train/test split: 70/30 or 80/20 for validation
4. Algorithm selection: Test 5-10 algorithms, compare performance
5. Cross-validation: K-fold (typically k=5 or k=10) to avoid overfitting
6. Model evaluation: Accuracy, precision, recall, F1-score, AUC-ROC
7. Feature importance: Identify which variables drive predictions
8. Deployment: Export model for production use

**Model Evaluation Metrics:**
- **Classification (Lead Scoring, Churn)**: Accuracy, Precision, Recall, F1-score, AUC-ROC
- **Regression (Enrollment Forecasting)**: MAE, RMSE, R-squared
- **Clustering (Segmentation)**: Silhouette score, Davies-Bouldin index

**RapidMiner Advantages:**
- Visual workflow (no coding required) -- business users can collaborate
- Auto feature selection -- algorithm picks best predictors
- Cross-validation built-in -- reduces overfitting risk
- Model comparison -- test 10 algorithms simultaneously, pick best
- Export models for deployment -- integrate with CRM/marketing tools

**Limitations:**
- Less flexible than Python for custom algorithms
- Desktop software (not cloud-native, though Server version exists)
- Paid licenses for enterprise features (free version limited to 10,000 rows)

**When to use RapidMiner vs Python:**
- **RapidMiner**: Quick prototyping, business user collaboration, standard models, time-constrained projects
- **Python**: Custom algorithms, large-scale deployment, API integration, when dataset >100K rows

---

## 8. INTERACTIVE VISUALIZATION (MIS 441 - Altair)

**When to use:** Creating interactive dashboards, exploratory data analysis, multi-dimensional visualizations, stakeholder presentations requiring drill-downs.

### Core Visualization Types

**1. Enrollment Trends with Drill-Downs**
```python
import altair as alt
import pandas as pd

# Interactive line chart: enrollment by brand over time
chart = alt.Chart(enrollment_data).mark_line(point=True).encode(
    x='month:T',
    y='enrollments:Q',
    color='brand:N',
    tooltip=['month', 'brand', 'enrollments', 'campus']
).interactive()

chart.save('enrollment_trends.html')  # Export for sharing
```
**Use case**: Monthly board reporting, identify seasonal patterns, compare brand performance

**2. CPA Heatmap (Brand x City)**
```python
heatmap = alt.Chart(cpa_data).mark_rect().encode(
    x='city:O',
    y='brand:O',
    color=alt.Color('cpa:Q',
                    scale=alt.Scale(scheme='redyellowgreen', reverse=True),
                    legend=alt.Legend(title='CPA %')),
    tooltip=['city', 'brand', 'cpa', 'enrollments', 'marketing_spend']
).properties(
    width=600,
    height=400,
    title='CPA Performance by Brand and City'
)
```
**Use case**: Identify underperforming brand-city combinations, guide budget reallocation

**3. Funnel Visualization with Alerts**
```python
funnel = alt.Chart(funnel_data).mark_bar().encode(
    x=alt.X('stage:O', sort=['Lead', 'MQL', 'SQL', 'Visit', 'Application', 'Enrolled']),
    y='count:Q',
    color=alt.condition(
        alt.datum.conversion_rate < 0.05,  # Flag stages with <5% conversion
        alt.value('red'),
        alt.value('steelblue')
    ),
    tooltip=['stage', 'count', 'conversion_rate', 'drop_off_percent']
).properties(
    title='Enrollment Funnel - Red indicates bottleneck stages'
)
```
**Use case**: Identify funnel leaks, prioritize optimization efforts

**4. Multi-Dimensional Scatter (Lead Quality)**
```python
scatter = alt.Chart(lead_data).mark_circle().encode(
    x='engagement_score:Q',
    y='time_to_enrollment:Q',
    size='fee_tier:Q',
    color='source:N',
    tooltip=['lead_id', 'source', 'engagement_score', 'enrollment_probability', 'counselor']
).interactive()
```
**Use case**: Lead prioritization, identify high-quality sources, optimize counselor assignment

**5. Geographic Bubble Map (City Performance)**
```python
bubble_map = alt.Chart(city_data).mark_circle().encode(
    longitude='longitude:Q',
    latitude='latitude:Q',
    size=alt.Size('enrollments:Q', scale=alt.Scale(range=[100, 3000])),
    color=alt.Color('cpa_percent:Q', scale=alt.Scale(scheme='redyellowgreen', reverse=True)),
    tooltip=['city', 'enrollments', 'cpa_percent', 'market_share']
).project('mercator').properties(
    width=800,
    height=600,
    title='Campus Performance by Geography'
)
```
**Use case**: Market entry decisions, identify expansion opportunities

**6. Time Series with Confidence Bands (Forecasting)**
```python
line = alt.Chart(forecast_data).mark_line().encode(
    x='month:T',
    y='enrollment:Q'
)

band = alt.Chart(forecast_data).mark_area(opacity=0.3).encode(
    x='month:T',
    y='lower_bound:Q',
    y2='upper_bound:Q'
)

forecast_chart = (line + band).properties(
    title='Enrollment Forecast with 90% Confidence Interval'
)
```
**Use case**: Present forecasts with uncertainty to CFO/Board

### Altair Best Practices

**Chart Design Principles:**
- **One message per chart**: Don't overload with multiple insights
- **Color for meaning**: Use red/yellow/green for performance, distinct hues for categories
- **Tooltips are essential**: Include all relevant context
- **Interactive by default**: Enable zoom, pan, selection
- **Export to HTML**: Share with non-technical stakeholders

**Data Encoding:**
- Quantitative (Q): Numbers (enrollment, CPA, revenue)
- Ordinal (O): Ordered categories (Low/Medium/High, stages in funnel)
- Nominal (N): Unordered categories (brand names, cities, channels)
- Temporal (T): Dates and times

**Common Pitfalls:**
- Don't use 3D charts (harder to read)
- Avoid pie charts for >5 categories (use bar charts)
- Don't truncate y-axis to exaggerate differences (start at 0 for counts)
- Include data labels when chart has <10 data points

**When to use Altair vs Excel:**
- **Altair**: Multi-dimensional data, interactive exploration, programmatic generation, reproducibility
- **Excel**: Quick one-off charts, stakeholder familiarity, embedded in spreadsheets

**When to use Altair vs Tableau:**
- **Altair**: Code-based reproducibility, version control, free, integrates with Python workflows
- **Tableau**: Drag-and-drop for non-coders, enterprise BI features, real-time data connections

---

## 9. EXCEL/SHEETS POWER FUNCTIONS

**When to use:** Building dashboards, multi-criteria analysis, dynamic reports, automating repetitive calculations.

### Multi-Criteria Analysis
```
=SUMIFS(Revenue, Brand, "GIIS", City, "Bangalore", Month, "January")
=COUNTIFS(Leads, ">100", CPA, "<2%", Channel, "Google Ads")
=AVERAGEIFS(ConversionRate, Brand, "OWIS", FeeRange, "Premium")
```
**Use case**: Calculate total revenue for a brand in a city in a given month, count campaigns meeting KPI targets

### Dynamic Lookups (INDEX-MATCH)
```
=INDEX(Fee_Range, MATCH(Brand_Name, Brand_List, 0))
=INDEX(CPA_Data, MATCH(1, (Brand=B2)*(City=C2), 0))  // Array formula for multi-criteria
```
**Why better than VLOOKUP:**
- Works left/right (VLOOKUP only right)
- Faster for large datasets
- Column insertions don't break formulas

### Google Sheets QUERY (SQL-like)
```
=QUERY(A1:F100, "SELECT A, SUM(E) WHERE B='GIIS' GROUP BY A ORDER BY SUM(E) DESC")
=QUERY(LeadData, "SELECT Source, COUNT(LeadID) WHERE Status='Enrolled' GROUP BY Source")
=QUERY(Campaigns, "SELECT Brand, City, AVG(CPA) WHERE Date >= date '2025-01-01' GROUP BY Brand, City PIVOT Channel")
```
**Use case**: Create pivot-table-like summaries without manual pivots

### Array Formulas
```
=ARRAYFORMULA(IF(B2:B="", "", B2:B * C2:C))  // Multiply entire columns
=ARRAYFORMULA(VLOOKUP(A2:A, LookupRange, 2, FALSE))  // Bulk lookups
```
**Use case**: Apply formulas to thousands of rows instantly without dragging

### Conditional Formatting with Formulas
```
// Highlight CPA >2% in red
=AND($E2>0.02, $D2="GIIS")

// Color scale for conversion rates
=IF($F2<0.05, "Red", IF($F2<0.08, "Yellow", "Green"))
```
**Use case**: Traffic light dashboards, automatic alerts for KPI violations

---

## 10. REGEX FOR DATA CLEANING

**When to use:** Cleaning messy CRM exports, extracting structured data from text, validating data quality, parsing campaign names.

### Common Patterns
```regex
Phone (India): (\+91)?[6-9]\d{9}
Email: [a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
Fee Amount: \s?(\d{1,2}[,.]?\d{0,2})\s?(L|Lakh|Cr|Crore)
```

### Use in Google Sheets
```
=REGEXEXTRACT(A2, "(GIIS|OWIS|Witty)_([A-Za-z]+)_(\d{4})")  // Extract brand from campaign name
=REGEXREPLACE(B2, "[^0-9]", "")  // Strip non-numeric characters from phone numbers
=REGEXMATCH(C2, "^\+?[6-9]\d{9}$")  // Validate Indian phone number format
```

### Use in Apps Script
```javascript
function cleanCampaignNames() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var data = sheet.getDataRange().getValues();
  var campaignPattern = /^(GIIS|OWIS|Witty)_([A-Z][a-z]+)_(\d{4})$/;

  for (var i = 1; i < data.length; i++) {
    var campaignName = data[i][2];  // Column C
    var match = campaignName.match(campaignPattern);

    if (match) {
      data[i][10] = match[1];  // Brand
      data[i][11] = match[2];  // City
      data[i][12] = match[3];  // Year
    } else {
      data[i][13] = "INVALID_FORMAT";  // Flag for manual review
    }
  }

  sheet.getRange(1, 1, data.length, data[0].length).setValues(data);
}
```

### Common Data Cleaning Tasks
```regex
// Remove extra whitespace
\s+  ->  (space)

// Standardize city names
/bangalore|blr|bengaluru/i  ->  Bangalore
/mumbai|bombay/i  ->  Mumbai

// Extract domain from email
([^@]+)@(.+)  ->  Capture group 2 for domain

// Parse dates in multiple formats
(\d{1,2})[-/](\d{1,2})[-/](\d{2,4})
```

**Use case**: Clean CRM exports with inconsistent formatting, validate campaign naming conventions, extract structured data from free-text form fields

---

## 11. INTEGRATED WORKFLOW: DATA MINING -> VISUALIZATION -> ACTION

### Example: Lead Scoring + Optimization

**Step 1: RapidMiner - Build Lead Scoring Model**
- **Input**: Historical leads CSV (10K rows) with features: source, city, engagement_score, campus_visit, form_completion_time, outcome (enrolled Y/N)
- **Process**:
  - 70/30 train/test split
  - Test Decision Tree, Random Forest, Gradient Boosting
  - Cross-validate (k=5)
  - Select Random Forest (highest AUC-ROC: 0.84)
- **Output**: Trained model + probability scores for new leads
- **Export**: Scored_Leads.csv with enrollment_probability (0-100%)

**Step 2: Altair - Visualize Lead Quality Distribution**
```python
import pandas as pd
import altair as alt

scored_leads = pd.read_csv('Scored_Leads.csv')

scatter = alt.Chart(scored_leads).mark_circle(size=60).encode(
    x='engagement_score:Q',
    y='enrollment_probability:Q',
    color=alt.Color('source:N', legend=alt.Legend(title='Source')),
    size=alt.Size('fee_tier:Q', legend=alt.Legend(title='Fee Tier (Lakhs)')),
    tooltip=['lead_id', 'source', 'engagement_score', 'enrollment_probability', 'city']
).interactive().properties(
    width=800,
    height=600,
    title='Lead Quality: High-probability leads in top-right quadrant'
)

scatter.save('lead_quality_dashboard.html')
```

**Step 3: Action - Marketing Prioritization**
- **Insight**: Google Ads leads have 65% enrollment probability vs. 35% for Facebook
- **Action**:
  - Reallocate budget from underperforming to high-performing channels
  - Train counselors to prioritize leads with enrollment_probability >60%
  - Create separate nurture campaign for 30-60% probability leads
- **Expected Impact**:
  - Conversion rate improvement: +55%
  - CPA reduction: -35%
  - Counselor efficiency improvement: +40%

**Step 4: Monitor - Track Results with Dashboard**
- Weekly Altair chart: conversion rate by probability tier
- Alert if high-probability leads convert at <50% (model drift)
- Monthly RapidMiner retraining with new data

---

## 12. ANALYTICAL DECISION PROTOCOLS

### Market Sizing Protocol
1. Define geographic boundary (city, zone, micro-market)
2. Fermi estimate: Population -> Households -> School-age children -> Income-qualifying -> Private school preference -> Premium segment
3. Cross-reference with available market data
4. Sanity check: compare to known enrollment numbers for existing operations
5. Calculate addressable market share needed for target enrollment

### CPA Optimization Protocol
1. Calculate current CPA per location: Total marketing spend / New enrollments
2. Calculate CPA as % of Year 1 fee (target: <2%)
3. Break down CPA by channel: Google Ads, Meta, Events, Referrals, Walk-ins
4. Apply 80/20: which 20% of channels drive 80% of enrollments?
5. Calculate marginal CPA for each channel (next unit spent)
6. Reallocate budget to lowest marginal CPA channels
7. Model impact: if referral share increases, what happens to blended CPA?

### Enrollment Forecasting Protocol
1. Gather 3-5 years historical enrollment by location
2. Apply exponential smoothing (a=0.5 for established, a=0.7 for new)
3. Adjust for known demand shifters (new competitor, fee change, capacity expansion)
4. Calculate required lead volume: Target enrollment / Conversion rate
5. Back-calculate required marketing spend: Required leads x Cost per lead
6. Compare to budget -> gap analysis

### Campus ROI Protocol
1. Revenue: Enrolled students x Average fee
2. Variable costs: Teacher salaries, materials, per-student costs
3. Contribution margin per student
4. Fixed costs: Rent, admin, infrastructure
5. Break-even enrollment
6. Current utilization vs. break-even
7. Marketing ROI: (Incremental revenue from marketing - Marketing spend) / Marketing spend
8. Target: 5:1 marketing ROI

### Pricing Decision Protocol
1. Assess price elasticity for this segment (premium vs. budget)
2. Check competitive positioning: where is this brand on the fee spectrum?
3. Test: is the brand clearly premium OR clearly value? (avoid the middle)
4. Would a 5-10% increase reduce enrollment by less than 5-10%?
5. If yes -> raise prices (inelastic demand)
6. If no -> improve value proposition before pricing
7. Calculate revenue impact: (New Price x Expected Volume) vs. (Old Price x Current Volume)

---

## 13. REFERENCE ANCHORS FOR ESTIMATION

### Marketing Quick References
| Metric | Benchmark | Notes |
|--------|-----------|-------|
| Google Ads CPC (Education, India) | 15-50 per click | Varies by keyword |
| Meta CPM (Education) | 80-200 | Varies by audience |
| Landing page conversion rate | 3-8% | Visitor to lead |
| Lead-to-visit conversion | 15-25% | Industry average |
| Visit-to-enrollment conversion | 20-40% | Industry average |
| Overall lead-to-enrollment | 3-5% (current) to 6-8% (target) | End-to-end |
| Premium school CAC | 8,000-20,000 | Per enrollment |
| LTV:CAC target | 5:1+ | Minimum viable |
| Marketing ROI target | 5:1 | Investment return |

### Useful Conversion Factors
- 1 Lakh = 100,000
- 1 Crore = 10,000,000 = 100 Lakhs
- Academic year: ~10 months instruction
