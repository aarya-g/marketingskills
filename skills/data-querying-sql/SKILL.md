---
name: data-querying-sql
description: >
  SQL querying for marketing analytics: SELECT, JOIN, WHERE, GROUP BY, HAVING, window functions,
  CTEs, subqueries. Use when analyzing large datasets from CRM/databases, performing multi-table
  joins (contacts, companies, deals), calculating cohort metrics, funnel analysis, or when
  Excel/Sheets hit row limits (>100K rows). Essential for 5-year historical analysis and
  proprietary data modeling.
---

# Data Querying (SQL) Skill

Use this skill when the task requires: querying CRM databases (HubSpot, Salesforce), analyzing large datasets (>100K rows), multi-table joins, time-series analysis, cohort analysis, funnel metrics, or building proprietary data models that Excel cannot handle.

**Critical for GSG:** Process 5-year enrollment history, build LTV cohort models, analyze 500K+ lead records, create automated reporting pipelines.

---

## 1. SQL FUNDAMENTALS FOR MARKETING

### Core SELECT Statement
```sql
SELECT
  column1,
  column2,
  column3
FROM table_name
WHERE condition
ORDER BY column1 DESC
LIMIT 100;
```

### Essential Clauses
- **SELECT**: Columns to retrieve
- **FROM**: Table(s) to query
- **WHERE**: Filter rows (before aggregation)
- **GROUP BY**: Aggregate data by category
- **HAVING**: Filter groups (after aggregation)
- **ORDER BY**: Sort results
- **LIMIT**: Restrict number of rows returned

---

## 2. FILTERING DATA (WHERE CLAUSE)

### Basic Filters
```sql
-- Enrollments in 2024
SELECT * FROM enrollments
WHERE enrollment_date >= '2024-01-01'
  AND enrollment_date < '2025-01-01';

-- GIIS brand only
SELECT * FROM campuses
WHERE brand = 'GIIS';

-- Premium fee tier (₹4L+)
SELECT * FROM students
WHERE annual_fee >= 400000;

-- Multiple cities
SELECT * FROM leads
WHERE city IN ('Bangalore', 'Mumbai', 'Pune');

-- Pattern matching
SELECT * FROM campaigns
WHERE campaign_name LIKE 'GIIS_Bangalore_%';
```

### Advanced Filters
```sql
-- Combination conditions
SELECT * FROM leads
WHERE (city = 'Bangalore' OR city = 'Mumbai')
  AND status = 'Enrolled'
  AND enrollment_date >= '2024-01-01';

-- NULL handling
SELECT * FROM leads
WHERE email IS NOT NULL
  AND phone_number IS NOT NULL;

-- NOT conditions
SELECT * FROM students
WHERE grade NOT IN ('Pre-K', 'Nursery')
  AND status != 'Withdrawn';

-- Date ranges
SELECT * FROM marketing_spend
WHERE spend_date BETWEEN '2024-01-01' AND '2024-12-31';
```

---

## 3. AGGREGATION (GROUP BY)

### Basic Aggregations
```sql
-- Count enrollments by brand
SELECT
  brand,
  COUNT(*) as total_enrollments
FROM enrollments
GROUP BY brand;

-- Total revenue by city
SELECT
  city,
  SUM(annual_fee) as total_revenue
FROM enrollments
GROUP BY city
ORDER BY total_revenue DESC;

-- Average CPA by channel
SELECT
  channel,
  AVG(cost_per_acquisition) as avg_cpa,
  COUNT(*) as num_campaigns
FROM marketing_campaigns
GROUP BY channel;
```

### Multi-Level Aggregations
```sql
-- Enrollments by brand and city
SELECT
  brand,
  city,
  COUNT(*) as enrollments,
  SUM(annual_fee) as revenue,
  AVG(annual_fee) as avg_fee
FROM enrollments
WHERE enrollment_date >= '2024-01-01'
GROUP BY brand, city
ORDER BY brand, enrollments DESC;
```

### HAVING (Filter After Aggregation)
```sql
-- Cities with >100 enrollments
SELECT
  city,
  COUNT(*) as enrollments
FROM enrollments
GROUP BY city
HAVING COUNT(*) > 100;

-- Brands with avg CPA <2%
SELECT
  brand,
  AVG(cpa_percent) as avg_cpa
FROM campaigns
GROUP BY brand
HAVING AVG(cpa_percent) < 2.0;
```

---

## 4. JOINS (COMBINING TABLES)

### INNER JOIN (Only Matching Rows)
```sql
-- Leads with their enrollment status
SELECT
  l.lead_id,
  l.name,
  l.email,
  l.source,
  e.enrollment_date,
  e.annual_fee
FROM leads l
INNER JOIN enrollments e ON l.lead_id = e.lead_id;
```

### LEFT JOIN (All Rows from Left Table)
```sql
-- All leads, with enrollment info if exists
SELECT
  l.lead_id,
  l.name,
  l.source,
  l.created_date,
  e.enrollment_date,
  CASE
    WHEN e.enrollment_date IS NOT NULL THEN 'Enrolled'
    ELSE 'Not Enrolled'
  END as status
FROM leads l
LEFT JOIN enrollments e ON l.lead_id = e.lead_id;
```

### Multiple Joins
```sql
-- Leads -> Campuses -> Enrollment with full context
SELECT
  l.lead_id,
  l.name,
  c.campus_name,
  c.brand,
  c.city,
  e.enrollment_date,
  e.annual_fee,
  m.campaign_name,
  m.channel
FROM leads l
LEFT JOIN campuses c ON l.campus_id = c.campus_id
LEFT JOIN enrollments e ON l.lead_id = e.lead_id
LEFT JOIN marketing_attribution m ON l.lead_id = m.lead_id
WHERE l.created_date >= '2024-01-01';
```

---

## 5. MARKETING ANALYTICS QUERIES

### CPA by Channel (Last 90 Days)
```sql
SELECT
  channel,
  COUNT(DISTINCT lead_id) as total_leads,
  SUM(spend) as total_spend,
  COUNT(DISTINCT CASE WHEN status = 'Enrolled' THEN lead_id END) as enrollments,
  SUM(spend) / NULLIF(COUNT(DISTINCT CASE WHEN status = 'Enrolled' THEN lead_id END), 0) as cpa,
  ROUND(
    100.0 * SUM(spend) / NULLIF(COUNT(DISTINCT CASE WHEN status = 'Enrolled' THEN lead_id END), 0) /
    AVG(CASE WHEN status = 'Enrolled' THEN annual_fee END), 2
  ) as cpa_percent
FROM marketing_data
WHERE created_date >= CURRENT_DATE - INTERVAL '90 days'
GROUP BY channel
ORDER BY cpa ASC;
```

### Funnel Conversion Rates
```sql
SELECT
  brand,
  city,
  COUNT(DISTINCT CASE WHEN stage >= 'Lead' THEN lead_id END) as leads,
  COUNT(DISTINCT CASE WHEN stage >= 'MQL' THEN lead_id END) as mqls,
  COUNT(DISTINCT CASE WHEN stage >= 'SQL' THEN lead_id END) as sqls,
  COUNT(DISTINCT CASE WHEN stage >= 'Visit' THEN lead_id END) as visits,
  COUNT(DISTINCT CASE WHEN stage >= 'Application' THEN lead_id END) as applications,
  COUNT(DISTINCT CASE WHEN stage = 'Enrolled' THEN lead_id END) as enrollments,

  -- Conversion rates
  ROUND(100.0 * COUNT(DISTINCT CASE WHEN stage >= 'MQL' THEN lead_id END) /
        NULLIF(COUNT(DISTINCT CASE WHEN stage >= 'Lead' THEN lead_id END), 0), 2) as lead_to_mql,
  ROUND(100.0 * COUNT(DISTINCT CASE WHEN stage = 'Enrolled' THEN lead_id END) /
        NULLIF(COUNT(DISTINCT CASE WHEN stage >= 'Lead' THEN lead_id END), 0), 2) as lead_to_enrollment
FROM funnel_data
WHERE created_date >= '2024-01-01'
GROUP BY brand, city
ORDER BY enrollments DESC;
```

### Monthly Enrollment Trends
```sql
SELECT
  DATE_TRUNC('month', enrollment_date) as month,
  brand,
  COUNT(*) as enrollments,
  SUM(annual_fee) as revenue,
  AVG(annual_fee) as avg_fee
FROM enrollments
WHERE enrollment_date >= '2022-01-01'
GROUP BY DATE_TRUNC('month', enrollment_date), brand
ORDER BY month, brand;
```

### Source Performance (ROI Analysis)
```sql
SELECT
  source,
  COUNT(DISTINCT lead_id) as total_leads,
  COUNT(DISTINCT CASE WHEN enrolled = TRUE THEN lead_id END) as enrollments,
  ROUND(100.0 * COUNT(DISTINCT CASE WHEN enrolled = TRUE THEN lead_id END) /
        NULLIF(COUNT(DISTINCT lead_id), 0), 2) as conversion_rate,
  SUM(marketing_spend) as total_spend,
  SUM(CASE WHEN enrolled = TRUE THEN annual_fee ELSE 0 END) as revenue_generated,
  ROUND((SUM(CASE WHEN enrolled = TRUE THEN annual_fee ELSE 0 END) - SUM(marketing_spend)) /
        NULLIF(SUM(marketing_spend), 0), 2) as roi
FROM leads_with_spend
WHERE created_date >= '2024-01-01'
GROUP BY source
HAVING COUNT(DISTINCT lead_id) >= 50  -- Minimum sample size
ORDER BY roi DESC;
```

---

## 6. COHORT ANALYSIS

### Enrollment Cohorts (By Year)
```sql
SELECT
  EXTRACT(YEAR FROM enrollment_date) as cohort_year,
  brand,
  COUNT(*) as initial_enrollment,
  COUNT(CASE WHEN years_retained >= 1 THEN 1 END) as retained_1yr,
  COUNT(CASE WHEN years_retained >= 2 THEN 1 END) as retained_2yr,
  COUNT(CASE WHEN years_retained >= 3 THEN 1 END) as retained_3yr,
  ROUND(100.0 * COUNT(CASE WHEN years_retained >= 3 THEN 1 END) / COUNT(*), 2) as retention_rate_3yr,
  SUM(lifetime_fees_paid) as total_ltv,
  AVG(lifetime_fees_paid) as avg_ltv_per_student
FROM student_cohorts
GROUP BY EXTRACT(YEAR FROM enrollment_date), brand
ORDER BY cohort_year, brand;
```

### Lead Source Cohorts (Conversion Over Time)
```sql
WITH cohort_base AS (
  SELECT
    DATE_TRUNC('month', created_date) as cohort_month,
    source,
    lead_id,
    created_date,
    enrollment_date
  FROM leads
  WHERE created_date >= '2023-01-01'
)
SELECT
  cohort_month,
  source,
  COUNT(*) as total_leads,
  COUNT(CASE WHEN enrollment_date <= cohort_month + INTERVAL '30 days' THEN 1 END) as enrolled_30d,
  COUNT(CASE WHEN enrollment_date <= cohort_month + INTERVAL '60 days' THEN 1 END) as enrolled_60d,
  COUNT(CASE WHEN enrollment_date <= cohort_month + INTERVAL '90 days' THEN 1 END) as enrolled_90d,
  ROUND(100.0 * COUNT(CASE WHEN enrollment_date <= cohort_month + INTERVAL '90 days' THEN 1 END) / COUNT(*), 2) as conv_rate_90d
FROM cohort_base
GROUP BY cohort_month, source
ORDER BY cohort_month, source;
```

---

## 7. WINDOW FUNCTIONS (ADVANCED)

### Running Totals
```sql
-- Cumulative enrollments by month
SELECT
  enrollment_month,
  brand,
  monthly_enrollments,
  SUM(monthly_enrollments) OVER (
    PARTITION BY brand
    ORDER BY enrollment_month
  ) as cumulative_enrollments
FROM monthly_enrollment_summary
ORDER BY brand, enrollment_month;
```

### Ranking
```sql
-- Top 5 performing campaigns by CPA per brand
SELECT
  brand,
  campaign_name,
  cpa,
  ROW_NUMBER() OVER (PARTITION BY brand ORDER BY cpa ASC) as rank
FROM campaigns
WHERE enrollments > 0
QUALIFY ROW_NUMBER() OVER (PARTITION BY brand ORDER BY cpa ASC) <= 5;
```

### Moving Averages
```sql
-- 3-month moving average enrollment
SELECT
  month,
  brand,
  enrollments,
  AVG(enrollments) OVER (
    PARTITION BY brand
    ORDER BY month
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
  ) as moving_avg_3m
FROM monthly_enrollments
ORDER BY brand, month;
```

### Lag/Lead (Previous/Next Values)
```sql
-- Month-over-month enrollment change
SELECT
  month,
  brand,
  enrollments,
  LAG(enrollments) OVER (PARTITION BY brand ORDER BY month) as prev_month_enrollments,
  enrollments - LAG(enrollments) OVER (PARTITION BY brand ORDER BY month) as mom_change,
  ROUND(100.0 * (enrollments - LAG(enrollments) OVER (PARTITION BY brand ORDER BY month)) /
        NULLIF(LAG(enrollments) OVER (PARTITION BY brand ORDER BY month), 0), 2) as mom_change_pct
FROM monthly_enrollments
ORDER BY brand, month;
```

---

## 8. COMMON TABLE EXPRESSIONS (CTEs)

### Basic CTE
```sql
-- Break complex query into readable steps
WITH high_value_leads AS (
  SELECT
    lead_id,
    source,
    engagement_score
  FROM leads
  WHERE engagement_score > 70
)
SELECT
  source,
  COUNT(*) as num_high_value_leads,
  AVG(engagement_score) as avg_engagement
FROM high_value_leads
GROUP BY source;
```

### Multiple CTEs
```sql
-- Multi-step funnel analysis
WITH stage_1_leads AS (
  SELECT lead_id, source, created_date
  FROM leads
  WHERE created_date >= '2024-01-01'
),
stage_2_mqls AS (
  SELECT lead_id, mql_date
  FROM lead_stages
  WHERE stage = 'MQL'
),
stage_3_enrollments AS (
  SELECT lead_id, enrollment_date, annual_fee
  FROM enrollments
)
SELECT
  l.source,
  COUNT(DISTINCT l.lead_id) as total_leads,
  COUNT(DISTINCT m.lead_id) as mqls,
  COUNT(DISTINCT e.lead_id) as enrollments,
  ROUND(100.0 * COUNT(DISTINCT e.lead_id) / COUNT(DISTINCT l.lead_id), 2) as conversion_rate,
  SUM(e.annual_fee) as total_revenue
FROM stage_1_leads l
LEFT JOIN stage_2_mqls m ON l.lead_id = m.lead_id
LEFT JOIN stage_3_enrollments e ON l.lead_id = e.lead_id
GROUP BY l.source
ORDER BY enrollments DESC;
```

---

## 9. TIME-SERIES ANALYSIS

### Year-Over-Year Comparison
```sql
SELECT
  brand,
  city,
  EXTRACT(YEAR FROM enrollment_date) as year,
  EXTRACT(MONTH FROM enrollment_date) as month,
  COUNT(*) as enrollments,
  LAG(COUNT(*)) OVER (
    PARTITION BY brand, city, EXTRACT(MONTH FROM enrollment_date)
    ORDER BY EXTRACT(YEAR FROM enrollment_date)
  ) as prev_year_enrollments,
  COUNT(*) - LAG(COUNT(*)) OVER (
    PARTITION BY brand, city, EXTRACT(MONTH FROM enrollment_date)
    ORDER BY EXTRACT(YEAR FROM enrollment_date)
  ) as yoy_change
FROM enrollments
WHERE enrollment_date >= '2022-01-01'
GROUP BY brand, city, EXTRACT(YEAR FROM enrollment_date), EXTRACT(MONTH FROM enrollment_date)
ORDER BY brand, city, year, month;
```

### Seasonality Detection
```sql
-- Average enrollments by month (across all years)
SELECT
  EXTRACT(MONTH FROM enrollment_date) as month,
  brand,
  AVG(monthly_count) as avg_enrollments,
  STDDEV(monthly_count) as stddev_enrollments
FROM (
  SELECT
    DATE_TRUNC('month', enrollment_date) as month_date,
    brand,
    COUNT(*) as monthly_count
  FROM enrollments
  WHERE enrollment_date >= '2020-01-01'
  GROUP BY DATE_TRUNC('month', enrollment_date), brand
) monthly_data
GROUP BY EXTRACT(MONTH FROM month_date), brand
ORDER BY month, brand;
```

---

## 10. DATA QUALITY CHECKS

### Duplicate Detection
```sql
-- Find duplicate leads (same email)
SELECT
  email,
  COUNT(*) as num_duplicates,
  STRING_AGG(lead_id::TEXT, ', ') as lead_ids
FROM leads
WHERE email IS NOT NULL
GROUP BY email
HAVING COUNT(*) > 1
ORDER BY num_duplicates DESC;
```

### Missing Data Analysis
```sql
-- Identify records with missing critical fields
SELECT
  'Missing Email' as issue,
  COUNT(*) as num_records
FROM leads
WHERE email IS NULL OR email = ''
UNION ALL
SELECT
  'Missing Phone',
  COUNT(*)
FROM leads
WHERE phone_number IS NULL OR phone_number = ''
UNION ALL
SELECT
  'Missing Source',
  COUNT(*)
FROM leads
WHERE source IS NULL OR source = '';
```

### Data Consistency Checks
```sql
-- Enrollments without corresponding leads (orphaned records)
SELECT
  e.enrollment_id,
  e.student_name,
  e.enrollment_date
FROM enrollments e
LEFT JOIN leads l ON e.lead_id = l.lead_id
WHERE l.lead_id IS NULL;

-- Future-dated enrollments (data entry errors)
SELECT
  enrollment_id,
  student_name,
  enrollment_date
FROM enrollments
WHERE enrollment_date > CURRENT_DATE;
```

---

## 11. OPTIMIZATION & PERFORMANCE

### Use EXPLAIN to Check Query Performance
```sql
EXPLAIN ANALYZE
SELECT
  brand,
  COUNT(*) as enrollments
FROM enrollments
WHERE enrollment_date >= '2024-01-01'
GROUP BY brand;
```

### Indexing Strategy
```sql
-- Create index on frequently filtered columns
CREATE INDEX idx_enrollments_date ON enrollments(enrollment_date);
CREATE INDEX idx_leads_source_date ON leads(source, created_date);
CREATE INDEX idx_campaigns_brand_channel ON campaigns(brand, channel);
```

### Query Optimization Tips
1. **Filter early:** Use WHERE before JOIN when possible
2. **Avoid SELECT *:** Only select needed columns
3. **Use LIMIT:** For exploratory queries, limit results
4. **Index foreign keys:** Speed up JOIN operations
5. **Avoid functions in WHERE:** `WHERE YEAR(date) = 2024` -> `WHERE date >= '2024-01-01' AND date < '2025-01-01'`

---

## 12. GSG-SPECIFIC QUERY TEMPLATES

### Template 1: Brand Performance Dashboard
```sql
SELECT
  b.brand,
  COUNT(DISTINCT c.campus_id) as num_campuses,
  COUNT(DISTINCT e.student_id) as total_students,
  SUM(e.annual_fee) as total_revenue,
  AVG(e.annual_fee) as avg_fee,
  COUNT(DISTINCT CASE WHEN e.enrollment_date >= CURRENT_DATE - INTERVAL '1 year' THEN e.student_id END) as enrollments_last_12m,
  SUM(m.spend) as marketing_spend_last_12m,
  ROUND(100.0 * SUM(m.spend) / NULLIF(SUM(CASE WHEN e.enrollment_date >= CURRENT_DATE - INTERVAL '1 year' THEN e.annual_fee END), 0), 2) as cpa_percent
FROM brands b
LEFT JOIN campuses c ON b.brand = c.brand
LEFT JOIN enrollments e ON c.campus_id = e.campus_id
LEFT JOIN marketing_spend m ON b.brand = m.brand AND m.spend_date >= CURRENT_DATE - INTERVAL '1 year'
GROUP BY b.brand
ORDER BY total_revenue DESC;
```

### Template 2: City Expansion Analysis
```sql
WITH city_metrics AS (
  SELECT
    city,
    COUNT(DISTINCT campus_id) as num_campuses,
    COUNT(DISTINCT CASE WHEN enrollment_date >= '2024-01-01' THEN student_id END) as enrollments_2024,
    AVG(CASE WHEN enrollment_date >= '2024-01-01' THEN annual_fee END) as avg_fee_2024,
    SUM(CASE WHEN enrollment_date >= '2024-01-01' THEN annual_fee END) as revenue_2024
  FROM enrollments e
  JOIN campuses c ON e.campus_id = c.campus_id
  GROUP BY city
)
SELECT
  city,
  num_campuses,
  enrollments_2024,
  avg_fee_2024,
  revenue_2024,
  ROUND(revenue_2024 / num_campuses, 2) as revenue_per_campus,
  CASE
    WHEN num_campuses >= 5 THEN 'Mature Market'
    WHEN num_campuses >= 2 THEN 'Growth Market'
    ELSE 'New Market'
  END as market_stage
FROM city_metrics
ORDER BY revenue_2024 DESC;
```

### Template 3: LTV Cohort Model
```sql
WITH student_cohorts AS (
  SELECT
    s.student_id,
    s.enrollment_date,
    EXTRACT(YEAR FROM s.enrollment_date) as cohort_year,
    s.brand,
    s.initial_fee,
    COUNT(DISTINCT f.payment_id) as num_years_retained,
    SUM(f.fee_amount) as total_fees_paid
  FROM students s
  LEFT JOIN fee_payments f ON s.student_id = f.student_id
  GROUP BY s.student_id, s.enrollment_date, s.brand, s.initial_fee
)
SELECT
  cohort_year,
  brand,
  COUNT(*) as initial_cohort_size,
  AVG(num_years_retained) as avg_retention_years,
  AVG(total_fees_paid) as avg_ltv,
  AVG(initial_fee) as avg_initial_fee,
  ROUND(AVG(total_fees_paid) / AVG(initial_fee), 2) as ltv_to_initial_fee_ratio,
  SUM(total_fees_paid) as cohort_total_ltv
FROM student_cohorts
WHERE cohort_year >= 2019
GROUP BY cohort_year, brand
ORDER BY cohort_year, brand;
```

### Template 4: Channel Attribution (Last Touch)
```sql
SELECT
  m.channel,
  m.campaign_name,
  COUNT(DISTINCT l.lead_id) as attributed_leads,
  COUNT(DISTINCT CASE WHEN e.enrollment_date IS NOT NULL THEN l.lead_id END) as attributed_enrollments,
  ROUND(100.0 * COUNT(DISTINCT CASE WHEN e.enrollment_date IS NOT NULL THEN l.lead_id END) /
        NULLIF(COUNT(DISTINCT l.lead_id), 0), 2) as conversion_rate,
  SUM(m.spend) as total_spend,
  SUM(CASE WHEN e.enrollment_date IS NOT NULL THEN e.annual_fee ELSE 0 END) as attributed_revenue,
  ROUND(SUM(m.spend) / NULLIF(COUNT(DISTINCT CASE WHEN e.enrollment_date IS NOT NULL THEN l.lead_id END), 0), 2) as cpa
FROM marketing_touchpoints m
JOIN leads l ON m.lead_id = l.lead_id
  AND m.touchpoint_date = (
    SELECT MAX(touchpoint_date)
    FROM marketing_touchpoints
    WHERE lead_id = l.lead_id
      AND touchpoint_date <= COALESCE(l.enrollment_date, CURRENT_DATE)
  )
LEFT JOIN enrollments e ON l.lead_id = e.lead_id
WHERE m.touchpoint_date >= '2024-01-01'
GROUP BY m.channel, m.campaign_name
ORDER BY attributed_enrollments DESC;
```

---

## 13. CONNECTING TO DATABASES

### Common Database Types for Marketing
- **PostgreSQL**: HubSpot data warehouse, internal databases
- **MySQL**: WordPress, older CRM systems
- **BigQuery**: Google Analytics 360, Google Ads exports
- **Snowflake**: Enterprise data warehouses
- **SQLite**: Local analytics, prototyping

### Connection Examples

**Python (PostgreSQL):**
```python
import psycopg2
import pandas as pd

conn = psycopg2.connect(
    host="database.hostname.com",
    database="marketing_db",
    user="username",
    password="password"
)

query = """
SELECT brand, COUNT(*) as enrollments
FROM enrollments
WHERE enrollment_date >= '2024-01-01'
GROUP BY brand;
"""

df = pd.read_sql(query, conn)
conn.close()
```

**Google BigQuery:**
```python
from google.cloud import bigquery

client = bigquery.Client(project='gsg-analytics')

query = """
SELECT
  date,
  campaign_name,
  SUM(cost) as spend,
  SUM(conversions) as enrollments
FROM `gsg-analytics.google_ads.campaigns`
WHERE date >= '2024-01-01'
GROUP BY date, campaign_name
ORDER BY date;
"""

df = client.query(query).to_dataframe()
```

---

## 14. SQL BEST PRACTICES FOR GSG

### Naming Conventions
- **Tables**: Plural nouns (`leads`, `enrollments`, `campaigns`)
- **Columns**: Snake_case (`enrollment_date`, `annual_fee`, `cpa_percent`)
- **CTEs**: Descriptive names (`high_value_leads`, `monthly_aggregates`)

### Code Formatting
```sql
-- GOOD: Readable, organized
SELECT
  brand,
  city,
  COUNT(*) as enrollments,
  SUM(annual_fee) as revenue
FROM enrollments
WHERE enrollment_date >= '2024-01-01'
  AND status = 'Active'
GROUP BY brand, city
ORDER BY revenue DESC;

-- BAD: Hard to read
SELECT brand,city,COUNT(*) as enrollments,SUM(annual_fee) as revenue FROM enrollments WHERE enrollment_date>='2024-01-01' AND status='Active' GROUP BY brand,city ORDER BY revenue DESC;
```

### Comment Complex Logic
```sql
-- Calculate CPA as percentage of Year 1 fee
-- Target: <2% for all brands
SELECT
  brand,
  campaign_name,
  SUM(spend) as total_spend,
  COUNT(DISTINCT enrollment_id) as enrollments,
  AVG(annual_fee) as avg_fee,
  -- CPA in absolute terms
  SUM(spend) / NULLIF(COUNT(DISTINCT enrollment_id), 0) as cpa_absolute,
  -- CPA as percentage (GSG KPI)
  ROUND(
    100.0 * SUM(spend) / NULLIF(COUNT(DISTINCT enrollment_id), 0) / AVG(annual_fee),
    2
  ) as cpa_percent
FROM campaign_performance
WHERE campaign_date >= '2024-01-01'
GROUP BY brand, campaign_name
HAVING COUNT(DISTINCT enrollment_id) > 0  -- Exclude campaigns with zero enrollments
ORDER BY cpa_percent ASC;
```

### Test with LIMIT First
```sql
-- Always test on small sample before running full query
SELECT *
FROM large_table
WHERE complex_condition
LIMIT 100;  -- Remove after verifying results
```

---

## 15. TROUBLESHOOTING COMMON ERRORS

### Error: Division by Zero
```sql
-- BAD: Will error if denominator is 0
SELECT spend / enrollments as cpa
FROM campaigns;

-- GOOD: Use NULLIF to handle zero denominators
SELECT spend / NULLIF(enrollments, 0) as cpa
FROM campaigns;
```

### Error: Ambiguous Column Names
```sql
-- BAD: Multiple tables have 'id' column
SELECT id, name
FROM leads l
JOIN enrollments e ON l.lead_id = e.lead_id;

-- GOOD: Prefix with table alias
SELECT l.id as lead_id, l.name
FROM leads l
JOIN enrollments e ON l.lead_id = e.lead_id;
```

### Error: Aggregate Function Misuse
```sql
-- BAD: Can't mix aggregated and non-aggregated columns without GROUP BY
SELECT brand, COUNT(*) as enrollments, city
FROM enrollments;

-- GOOD: Include all non-aggregated columns in GROUP BY
SELECT brand, city, COUNT(*) as enrollments
FROM enrollments
GROUP BY brand, city;
```

### Error: String vs Number Comparison
```sql
-- BAD: Comparing string to number
SELECT * FROM leads
WHERE lead_score > '70';  -- lead_score is numeric

-- GOOD: Use correct data type
SELECT * FROM leads
WHERE lead_score > 70;
```

---

## 16. REFERENCE: SQL FUNCTION CHEAT SHEET

### Aggregate Functions
```sql
COUNT(*)              -- Count all rows
COUNT(DISTINCT col)   -- Count unique values
SUM(col)              -- Sum values
AVG(col)              -- Average
MIN(col)              -- Minimum
MAX(col)              -- Maximum
STDDEV(col)           -- Standard deviation
```

### String Functions
```sql
CONCAT(str1, str2)              -- Combine strings
UPPER(str)                      -- Uppercase
LOWER(str)                      -- Lowercase
SUBSTRING(str, start, length)   -- Extract substring
LENGTH(str)                     -- String length
TRIM(str)                       -- Remove whitespace
```

### Date Functions
```sql
CURRENT_DATE                    -- Today's date
CURRENT_TIMESTAMP               -- Current date/time
DATE_TRUNC('month', date_col)   -- Truncate to month
EXTRACT(YEAR FROM date_col)     -- Extract year
date_col + INTERVAL '1 year'    -- Add time interval
AGE(date1, date2)               -- Calculate difference
```

### Conditional Logic
```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE default_result
END

COALESCE(val1, val2, default)   -- Return first non-null
NULLIF(val1, val2)              -- Return NULL if values equal
```

---

## 17. NEXT STEPS: FROM SQL TO DATA PIPELINES

### Automate Queries with Scheduled Jobs
```sql
-- Example: Daily enrollment report
-- Schedule via cron job or database scheduler
SELECT
  CURRENT_DATE as report_date,
  brand,
  COUNT(*) as enrollments_today,
  SUM(annual_fee) as revenue_today
FROM enrollments
WHERE enrollment_date = CURRENT_DATE
GROUP BY brand;
```

### Export to Tools
- **Google Sheets**: Use Apps Script with JDBC connector
- **Python**: Use pandas.read_sql() for analysis
- **BI Tools**: Connect Tableau/Looker/Metabase to database
- **Automated Reports**: Schedule queries -> email CSV

### Build Data Warehouse
1. **Extract**: Pull data from HubSpot, Google Ads, Meta Ads
2. **Transform**: Clean, deduplicate, enrich with SQL
3. **Load**: Write to central database
4. **Analyze**: Run analytics queries on unified data

**Tools for ETL:**
- Fivetran, Stitch (managed ETL)
- Apache Airflow (custom pipelines)
- dbt (data transformation)
