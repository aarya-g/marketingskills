---
name: python-marketing-analytics
description: >
  Python for marketing analytics using pandas (data manipulation), numpy (numerical computing),
  matplotlib/seaborn (visualization), scikit-learn (machine learning), statsmodels (statistical
  modeling), requests/BeautifulSoup (web scraping), prophet (time series forecasting). Use when
  Excel/Apps Script hit limits, building custom ML models, automating competitor intelligence,
  processing large datasets (>100K rows), or creating advanced visualizations. Complements SQL
  for end-to-end analytics pipelines.
---

# Python Marketing Analytics Skill

Use this skill when the task requires: large-scale data manipulation (>100K rows), custom machine learning models, web scraping for competitor intelligence, time series forecasting, API automation, or advanced statistical analysis beyond Excel/RapidMiner capabilities.

**Critical for GSG:** Automate competitor monitoring, build custom enrollment forecasting models, process multi-year CRM exports, create production-grade analytics pipelines.

---

## 1. ENVIRONMENT SETUP

### Installing Core Libraries
```bash
# Create virtual environment (recommended)
python -m venv marketing_analytics
source marketing_analytics/bin/activate  # Mac/Linux
# marketing_analytics\Scripts\activate  # Windows

# Install essentials
pip install pandas numpy matplotlib seaborn scikit-learn statsmodels requests beautifulsoup4 prophet openpyxl
```

### Antigravity Project Setup
1. **New Project** -> Select existing interpreter (venv above)
2. **Project Structure:**
```
marketing_analytics/
├─ data/
│  ├─ raw/           # HubSpot exports, CRM dumps
│  ├─ processed/     # Cleaned data
│  └─ outputs/       # Reports, charts
├─ scripts/
│  ├─ data_cleaning.py
│  ├─ analysis.py
│  └─ scraper.py
├─ models/
│  └─ lead_scoring_model.pkl
└─ config.py         # API keys, database credentials
```

---

## 2. DATA MANIPULATION WITH PANDAS

### Reading Data
```python
import pandas as pd

# CSV (HubSpot exports)
df_leads = pd.read_csv('data/raw/hubspot_leads.csv')

# Excel (multiple sheets)
df_campaigns = pd.read_excel('data/raw/campaigns.xlsx', sheet_name='Q4_2024')

# SQL database
import sqlite3
conn = sqlite3.connect('marketing.db')
df_enrollments = pd.read_sql('SELECT * FROM enrollments WHERE year = 2024', conn)

# Google Sheets (via API)
import gspread
from oauth2client.service_account import ServiceAccountCredentials
gc = gspread.authorize(creds)
sheet = gc.open('Marketing Dashboard').sheet1
df_sheet = pd.DataFrame(sheet.get_all_records())
```

### Basic Data Inspection
```python
# First 10 rows
df_leads.head(10)

# Summary statistics
df_leads.describe()

# Data types and missing values
df_leads.info()

# Column names
df_leads.columns.tolist()

# Shape (rows, columns)
df_leads.shape

# Unique values in column
df_leads['source'].unique()
df_leads['source'].value_counts()
```

### Filtering (Like SQL WHERE)
```python
# Single condition
df_giis = df_leads[df_leads['brand'] == 'GIIS']

# Multiple conditions (AND)
df_bangalore_giis = df_leads[
    (df_leads['brand'] == 'GIIS') &
    (df_leads['city'] == 'Bangalore')
]

# Multiple conditions (OR)
df_premium_cities = df_leads[
    (df_leads['city'] == 'Bangalore') |
    (df_leads['city'] == 'Mumbai')
]

# Date filtering
df_leads['created_date'] = pd.to_datetime(df_leads['created_date'])
df_2024 = df_leads[df_leads['created_date'] >= '2024-01-01']

# Substring matching
df_google_campaigns = df_campaigns[df_campaigns['campaign_name'].str.contains('Google')]

# NOT condition
df_not_enrolled = df_leads[df_leads['status'] != 'Enrolled']

# NULL handling
df_with_email = df_leads[df_leads['email'].notna()]
```

### Data Cleaning
```python
# Remove duplicates
df_leads_clean = df_leads.drop_duplicates(subset=['email'])

# Fill missing values
df_leads['phone'].fillna('Not Provided', inplace=True)
df_leads['engagement_score'].fillna(df_leads['engagement_score'].mean(), inplace=True)

# Drop rows with missing critical fields
df_leads_clean = df_leads.dropna(subset=['email', 'source'])

# Rename columns
df_leads.rename(columns={'old_name': 'new_name'}, inplace=True)

# Convert data types
df_leads['annual_fee'] = pd.to_numeric(df_leads['annual_fee'], errors='coerce')
df_leads['created_date'] = pd.to_datetime(df_leads['created_date'])

# Remove outliers (IQR method)
Q1 = df_leads['cpa'].quantile(0.25)
Q3 = df_leads['cpa'].quantile(0.75)
IQR = Q3 - Q1
df_leads_no_outliers = df_leads[
    (df_leads['cpa'] >= Q1 - 1.5*IQR) &
    (df_leads['cpa'] <= Q3 + 1.5*IQR)
]
```

### Aggregation (Like SQL GROUP BY)
```python
# Count by brand
brand_counts = df_leads.groupby('brand')['lead_id'].count()

# Multiple aggregations
summary = df_leads.groupby('brand').agg({
    'lead_id': 'count',
    'annual_fee': ['mean', 'sum'],
    'cpa': 'mean'
}).round(2)

# Rename aggregated columns
summary.columns = ['num_leads', 'avg_fee', 'total_revenue', 'avg_cpa']

# Multi-level grouping
city_brand_summary = df_leads.groupby(['city', 'brand']).agg({
    'lead_id': 'count',
    'cpa': 'mean'
}).reset_index()
```

### Joins (Like SQL)
```python
# Inner join (only matching rows)
df_merged = pd.merge(
    df_leads,
    df_enrollments,
    on='lead_id',
    how='inner'
)

# Left join (all leads, enrollment info if exists)
df_full = pd.merge(
    df_leads,
    df_enrollments,
    on='lead_id',
    how='left'
)

# Multiple joins
df_complete = df_leads \
    .merge(df_campuses, on='campus_id', how='left') \
    .merge(df_enrollments, on='lead_id', how='left')
```

### Creating New Columns
```python
# Simple calculation
df_leads['cpa_percent'] = (df_leads['cpa'] / df_leads['annual_fee']) * 100

# Conditional logic (like SQL CASE)
df_leads['fee_tier'] = df_leads['annual_fee'].apply(
    lambda x: 'Premium' if x >= 400000 else 'Mid-Tier' if x >= 200000 else 'Budget'
)

# Using np.where for vectorized conditions
import numpy as np
df_leads['high_priority'] = np.where(
    (df_leads['engagement_score'] > 70) & (df_leads['campus_visit'] == True),
    'Yes',
    'No'
)

# Date-based features
df_leads['enrollment_month'] = df_leads['enrollment_date'].dt.month
df_leads['enrollment_year'] = df_leads['enrollment_date'].dt.year
df_leads['days_to_enrollment'] = (df_leads['enrollment_date'] - df_leads['created_date']).dt.days
```

---

## 3. VISUALIZATION WITH MATPLOTLIB & SEABORN

### Basic Matplotlib Charts
```python
import matplotlib.pyplot as plt

# Line chart (enrollment trends)
df_monthly = df_enrollments.groupby('month')['enrollment_id'].count()
plt.figure(figsize=(10, 6))
plt.plot(df_monthly.index, df_monthly.values, marker='o')
plt.title('Monthly Enrollment Trends')
plt.xlabel('Month')
plt.ylabel('Enrollments')
plt.grid(True)
plt.savefig('data/outputs/enrollment_trends.png', dpi=300, bbox_inches='tight')
plt.show()

# Bar chart (CPA by channel)
df_channel = df_campaigns.groupby('channel')['cpa'].mean().sort_values()
plt.figure(figsize=(10, 6))
plt.barh(df_channel.index, df_channel.values)
plt.title('Average CPA by Channel')
plt.xlabel('CPA')
plt.ylabel('Channel')
for i, v in enumerate(df_channel.values):
    plt.text(v, i, f' {v:,.0f}', va='center')
plt.tight_layout()
plt.savefig('data/outputs/cpa_by_channel.png', dpi=300)
plt.show()
```

### Seaborn for Statistical Plots
```python
import seaborn as sns

# Set style
sns.set_style('whitegrid')
sns.set_palette('Set2')

# Distribution plot (CPA distribution)
plt.figure(figsize=(10, 6))
sns.histplot(df_campaigns['cpa'], bins=30, kde=True)
plt.axvline(df_campaigns['cpa'].mean(), color='red', linestyle='--', label='Mean')
plt.title('CPA Distribution Across Campaigns')
plt.xlabel('CPA')
plt.legend()
plt.savefig('data/outputs/cpa_distribution.png', dpi=300)

# Box plot (fee by brand)
plt.figure(figsize=(12, 6))
sns.boxplot(data=df_enrollments, x='brand', y='annual_fee')
plt.title('Fee Distribution by Brand')
plt.ylabel('Annual Fee')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('data/outputs/fee_by_brand.png', dpi=300)

# Heatmap (correlation matrix)
numeric_cols = df_leads.select_dtypes(include=[np.number]).columns
corr_matrix = df_leads[numeric_cols].corr()
plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, fmt='.2f', cmap='coolwarm', center=0)
plt.title('Feature Correlation Matrix')
plt.tight_layout()
plt.savefig('data/outputs/correlation_heatmap.png', dpi=300)

# Scatter plot with regression line
plt.figure(figsize=(10, 6))
sns.regplot(data=df_campaigns, x='spend', y='enrollments', scatter_kws={'alpha':0.5})
plt.title('Marketing Spend vs Enrollments')
plt.xlabel('Spend')
plt.ylabel('Enrollments')
plt.savefig('data/outputs/spend_vs_enrollments.png', dpi=300)
```

---

## 4. MACHINE LEARNING WITH SCIKIT-LEARN

### Lead Scoring Model (Classification)
```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, roc_auc_score, confusion_matrix
import pickle

# Prepare data
features = ['engagement_score', 'campus_visit_binary', 'email_opens',
            'form_completion_time', 'source_encoded', 'fee_tier_encoded']
X = df_leads[features]
y = df_leads['enrolled']  # 1 = enrolled, 0 = not enrolled

# Train/test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Train model
model = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
model.fit(X_train, y_train)

# Predictions
y_pred = model.predict(X_test)
y_pred_proba = model.predict_proba(X_test)[:, 1]  # Probability of enrollment

# Evaluate
print(classification_report(y_test, y_pred))
print(f"ROC-AUC Score: {roc_auc_score(y_test, y_pred_proba):.3f}")

# Feature importance
feature_importance = pd.DataFrame({
    'feature': features,
    'importance': model.feature_importances_
}).sort_values('importance', ascending=False)
print(feature_importance)

# Save model
with open('models/lead_scoring_model.pkl', 'wb') as f:
    pickle.dump(model, f)

# Score new leads
new_leads = pd.read_csv('data/raw/new_leads.csv')
new_leads['enrollment_probability'] = model.predict_proba(new_leads[features])[:, 1]
new_leads['priority'] = new_leads['enrollment_probability'].apply(
    lambda x: 'High' if x > 0.6 else 'Medium' if x > 0.3 else 'Low'
)
new_leads.to_csv('data/outputs/scored_leads.csv', index=False)
```

### Enrollment Forecasting (Regression)
```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, r2_score

# Prepare time series data
df_monthly = df_enrollments.groupby('month').agg({
    'enrollment_id': 'count',
    'marketing_spend': 'sum'
}).reset_index()
df_monthly['month_num'] = range(len(df_monthly))

# Features: month number, marketing spend, seasonality
X = df_monthly[['month_num', 'marketing_spend']]
y = df_monthly['enrollment_id']

# Train/test split (chronological)
train_size = int(0.8 * len(df_monthly))
X_train, X_test = X[:train_size], X[train_size:]
y_train, y_test = y[:train_size], y[train_size:]

# Train model
model = LinearRegression()
model.fit(X_train, y_train)

# Predictions
y_pred = model.predict(X_test)

# Evaluate
print(f"MAE: {mean_absolute_error(y_test, y_pred):.1f}")
print(f"R2: {r2_score(y_test, y_pred):.3f}")

# Forecast next 3 months
next_months = pd.DataFrame({
    'month_num': [len(df_monthly), len(df_monthly)+1, len(df_monthly)+2],
    'marketing_spend': [2500000, 2500000, 2500000]  # Planned spend
})
forecast = model.predict(next_months)
print(f"Forecasted enrollments: {forecast}")
```

### Customer Segmentation (Clustering)
```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

# Select features for segmentation
features = ['engagement_score', 'website_visits', 'email_opens', 'annual_fee']
X = df_leads[features].dropna()

# Standardize (important for K-means)
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Determine optimal k (elbow method)
inertias = []
for k in range(2, 11):
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(X_scaled)
    inertias.append(kmeans.inertia_)

plt.plot(range(2, 11), inertias, marker='o')
plt.xlabel('Number of Clusters')
plt.ylabel('Inertia')
plt.title('Elbow Method for Optimal k')
plt.savefig('data/outputs/kmeans_elbow.png')

# Train with k=4 (based on elbow)
kmeans = KMeans(n_clusters=4, random_state=42)
df_leads['segment'] = kmeans.fit_predict(X_scaled)

# Analyze segments
segment_summary = df_leads.groupby('segment')[features + ['enrolled']].agg(['mean', 'count'])
print(segment_summary)

# Label segments
segment_labels = {
    0: 'Low Engagement',
    1: 'High Value Prospects',
    2: 'Budget Shoppers',
    3: 'Premium Seekers'
}
df_leads['segment_label'] = df_leads['segment'].map(segment_labels)
```

---

## 5. TIME SERIES FORECASTING WITH PROPHET

### Enrollment Forecasting
```python
from prophet import Prophet

# Prepare data (Prophet requires 'ds' and 'y' columns)
df_prophet = df_enrollments.groupby('enrollment_date')['enrollment_id'].count().reset_index()
df_prophet.columns = ['ds', 'y']

# Add seasonality (admission cycle: Nov-Mar peak)
model = Prophet(
    yearly_seasonality=True,
    weekly_seasonality=False,
    daily_seasonality=False
)
model.add_seasonality(name='admission_cycle', period=365.25, fourier_order=5)

# Train
model.fit(df_prophet)

# Forecast 12 months ahead
future = model.make_future_dataframe(periods=365)
forecast = model.predict(future)

# Plot
fig1 = model.plot(forecast)
plt.title('Enrollment Forecast (Next 12 Months)')
plt.savefig('data/outputs/enrollment_forecast.png', dpi=300)

fig2 = model.plot_components(forecast)
plt.savefig('data/outputs/forecast_components.png', dpi=300)

# Extract forecast summary
forecast_summary = forecast[['ds', 'yhat', 'yhat_lower', 'yhat_upper']].tail(365)
forecast_summary.to_csv('data/outputs/forecast_12months.csv', index=False)

print(f"Avg forecasted enrollments (next 12m): {forecast_summary['yhat'].mean():.0f}")
```

---

## 6. WEB SCRAPING FOR COMPETITOR INTELLIGENCE

### Scrape Competitor Website
```python
import requests
from bs4 import BeautifulSoup
import time

# Scrape fee information
def scrape_competitor_fees(url):
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
    }

    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.content, 'html.parser')

    # Extract fee table (adjust selectors based on actual website)
    fee_table = soup.find('table', class_='fee-structure')

    fees = []
    for row in fee_table.find_all('tr')[1:]:  # Skip header
        cols = row.find_all('td')
        grade = cols[0].text.strip()
        annual_fee = cols[1].text.strip()
        fees.append({'grade': grade, 'annual_fee': annual_fee})

    return pd.DataFrame(fees)

# Scrape campus locations
def scrape_competitor_campuses(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    campuses = []
    for campus_div in soup.find_all('div', class_='campus-card'):
        name = campus_div.find('h3').text.strip()
        city = campus_div.find('span', class_='city').text.strip()
        campuses.append({'campus_name': name, 'city': city})

    return pd.DataFrame(campuses)
```

### Automated Daily Competitor Monitoring
```python
import schedule
from datetime import datetime

def daily_competitor_check():
    # Scrape competitor data
    current_campuses = scrape_competitor_campuses('https://competitor.com/campuses')

    # Load yesterday's data
    try:
        previous_campuses = pd.read_csv('data/raw/competitor_campuses_previous.csv')
    except FileNotFoundError:
        previous_campuses = pd.DataFrame()

    # Detect changes
    if not previous_campuses.empty:
        new_campuses = current_campuses[
            ~current_campuses['campus_name'].isin(previous_campuses['campus_name'])
        ]

        if len(new_campuses) > 0:
            alert_msg = f"ALERT: Competitor opened {len(new_campuses)} new campus(es):\n"
            alert_msg += new_campuses.to_string()
            send_slack_alert(alert_msg)

    # Save current data as previous for tomorrow
    current_campuses.to_csv('data/raw/competitor_campuses_previous.csv', index=False)

# Schedule daily at 9 AM
schedule.every().day.at("09:00").do(daily_competitor_check)

while True:
    schedule.run_pending()
    time.sleep(60)
```

### Alternative: Apify (No-Code Scraping)
**When to use Apify instead of Python:**
- Pre-built scrapers available (Google Maps, LinkedIn, social media)
- Need cloud scheduling (daily runs)
- Want maintenance-free (Apify updates scrapers when sites change)
- Don't want to handle proxies/CAPTCHAs yourself

**When to use Python:**
- Custom scraping logic not available in Apify
- Very simple scrapes (single page, no anti-bot)
- Want full control and zero cost

---

## 7. API AUTOMATION

### HubSpot API (Pull CRM Data)
```python
import requests

HUBSPOT_API_KEY = 'your_api_key_here'
BASE_URL = 'https://api.hubapi.com'

def get_hubspot_contacts(limit=100):
    url = f'{BASE_URL}/crm/v3/objects/contacts'
    headers = {'Authorization': f'Bearer {HUBSPOT_API_KEY}'}
    params = {'limit': limit}

    response = requests.get(url, headers=headers, params=params)
    contacts = response.json()['results']

    df_contacts = pd.DataFrame([
        {
            'contact_id': c['id'],
            'email': c['properties'].get('email'),
            'firstname': c['properties'].get('firstname'),
            'lastname': c['properties'].get('lastname'),
            'lead_status': c['properties'].get('hs_lead_status')
        }
        for c in contacts
    ])

    return df_contacts

# Pull contacts
df_contacts = get_hubspot_contacts(limit=1000)
df_contacts.to_csv('data/raw/hubspot_contacts.csv', index=False)
```

### Google Ads API (Pull Campaign Performance)
```python
from google.ads.googleads.client import GoogleAdsClient

# Initialize client (requires oauth2 credentials)
client = GoogleAdsClient.load_from_storage('config/google-ads.yaml')

def get_campaign_performance(customer_id, start_date, end_date):
    ga_service = client.get_service("GoogleAdsService")

    query = f"""
        SELECT
            campaign.id,
            campaign.name,
            metrics.impressions,
            metrics.clicks,
            metrics.cost_micros,
            metrics.conversions
        FROM campaign
        WHERE segments.date BETWEEN '{start_date}' AND '{end_date}'
    """

    response = ga_service.search(customer_id=customer_id, query=query)

    campaigns = []
    for row in response:
        campaigns.append({
            'campaign_id': row.campaign.id,
            'campaign_name': row.campaign.name,
            'impressions': row.metrics.impressions,
            'clicks': row.metrics.clicks,
            'spend': row.metrics.cost_micros / 1_000_000,
            'conversions': row.metrics.conversions
        })

    return pd.DataFrame(campaigns)
```

---

## 8. STATISTICAL ANALYSIS WITH STATSMODELS

### A/B Test Analysis
```python
from statsmodels.stats.proportion import proportions_ztest

# Data from A/B test
control_visitors = 5000
control_conversions = 200
variant_visitors = 5000
variant_conversions = 250

# Z-test for proportions
count = np.array([control_conversions, variant_conversions])
nobs = np.array([control_visitors, variant_visitors])

z_stat, p_value = proportions_ztest(count, nobs)

print(f"Control conversion: {control_conversions/control_visitors:.2%}")
print(f"Variant conversion: {variant_conversions/variant_visitors:.2%}")
print(f"Z-statistic: {z_stat:.3f}")
print(f"P-value: {p_value:.4f}")

if p_value < 0.05:
    print("SIGNIFICANT: Variant is statistically better")
else:
    print("NOT SIGNIFICANT: Need more data or no real difference")
```

### Time Series Decomposition
```python
from statsmodels.tsa.seasonal import seasonal_decompose

# Prepare monthly enrollment time series
df_monthly = df_enrollments.set_index('month').sort_index()

# Decompose into trend, seasonal, residual
decomposition = seasonal_decompose(df_monthly['enrollments'], model='additive', period=12)

# Plot components
fig, axes = plt.subplots(4, 1, figsize=(12, 10))
decomposition.observed.plot(ax=axes[0], title='Observed')
decomposition.trend.plot(ax=axes[1], title='Trend')
decomposition.seasonal.plot(ax=axes[2], title='Seasonal')
decomposition.resid.plot(ax=axes[3], title='Residual')
plt.tight_layout()
plt.savefig('data/outputs/time_series_decomposition.png', dpi=300)
```

---

## 9. PRODUCTION PIPELINES & AUTOMATION

### Daily Automated Report Pipeline
```python
# daily_report.py

import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime, timedelta
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.image import MIMEImage

def generate_daily_report():
    # 1. Pull data from database
    conn = sqlite3.connect('marketing.db')
    yesterday = (datetime.now() - timedelta(days=1)).strftime('%Y-%m-%d')

    df_yesterday = pd.read_sql(f"""
        SELECT brand, city, COUNT(*) as enrollments, SUM(annual_fee) as revenue
        FROM enrollments
        WHERE enrollment_date = '{yesterday}'
        GROUP BY brand, city
    """, conn)

    # 2. Create visualizations
    plt.figure(figsize=(10, 6))
    df_yesterday.plot(kind='bar', x='brand', y='enrollments')
    plt.title(f'Enrollments by Brand - {yesterday}')
    plt.tight_layout()
    plt.savefig('/tmp/daily_enrollments.png', dpi=300)

    # 3. Calculate KPIs
    total_enrollments = df_yesterday['enrollments'].sum()
    total_revenue = df_yesterday['revenue'].sum()
    avg_fee = total_revenue / total_enrollments if total_enrollments > 0 else 0

    # 4. Email report
    msg = MIMEMultipart()
    msg['Subject'] = f'Daily Enrollment Report - {yesterday}'
    msg['From'] = 'analytics@company.com'
    msg['To'] = 'marketing-team@company.com'

    body = f"""
    Daily Enrollment Summary ({yesterday})

    Total Enrollments: {total_enrollments}
    Total Revenue: {total_revenue:,.0f}
    Average Fee: {avg_fee:,.0f}

    See attached chart for breakdown by brand.
    """

    msg.attach(MIMEText(body, 'plain'))

    with open('/tmp/daily_enrollments.png', 'rb') as f:
        img = MIMEImage(f.read())
        msg.attach(img)

    # Send email
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login('analytics@company.com', 'password')
    server.send_message(msg)
    server.quit()

    print(f"Daily report sent for {yesterday}")

# Run manually or schedule with cron
if __name__ == '__main__':
    generate_daily_report()
```

### Schedule with Python (No External Scheduler)
```python
# scheduler.py
import schedule
import time

# Import your report functions
from daily_report import generate_daily_report
from competitor_scraper import daily_competitor_check
from model_retraining import retrain_lead_scoring_model

# Schedule tasks
schedule.every().day.at("08:00").do(generate_daily_report)
schedule.every().day.at("09:00").do(daily_competitor_check)
schedule.every().monday.at("06:00").do(retrain_lead_scoring_model)

print("Scheduler started. Press Ctrl+C to stop.")
while True:
    schedule.run_pending()
    time.sleep(60)
```

---

## 10. GSG-SPECIFIC USE CASES

### Use Case 1: Automated Competitor Fee Tracking
```python
def track_competitor_fees():
    competitors = [
        ('Competitor A', 'https://competitora.com/fees'),
        ('Competitor B', 'https://competitorb.com/admissions/fees'),
        ('Competitor C', 'https://competitorc.com/schools/india/fees')
    ]

    all_fees = []
    for name, url in competitors:
        try:
            fees = scrape_fee_structure(url)
            fees['competitor'] = name
            fees['scraped_date'] = datetime.now()
            all_fees.append(fees)
        except Exception as e:
            print(f"Error scraping {name}: {e}")

    df_all_fees = pd.concat(all_fees, ignore_index=True)

    # Compare to internal fees
    df_internal_fees = pd.read_csv('data/internal/fee_structure.csv')

    # Analysis: Where are we cheaper/more expensive?
    comparison = df_all_fees.merge(
        df_internal_fees[['grade', 'brand', 'annual_fee']],
        on='grade',
        suffixes=('_competitor', '_internal')
    )

    comparison['price_difference'] = comparison['annual_fee_internal'] - comparison['annual_fee_competitor']
    comparison['price_difference_pct'] = (comparison['price_difference'] / comparison['annual_fee_competitor']) * 100

    # Flag opportunities
    underpriced = comparison[comparison['price_difference_pct'] < -10]

    if len(underpriced) > 0:
        alert = f"PRICING ALERT: Underpriced in {len(underpriced)} grade/brand combinations:\n"
        alert += underpriced[['brand', 'grade', 'competitor', 'price_difference_pct']].to_string()
        send_slack_alert(alert)

    # Save for historical tracking
    comparison.to_csv(f'data/outputs/competitor_fee_comparison_{datetime.now().strftime("%Y%m%d")}.csv', index=False)
```

### Use Case 2: Lead Scoring with Real-Time Predictions
```python
# Load trained model
with open('models/lead_scoring_model.pkl', 'rb') as f:
    lead_scoring_model = pickle.load(f)

def score_new_lead(lead_data):
    """
    Scores a single lead in real-time (e.g., called from webhook when form submitted)

    lead_data = {
        'engagement_score': 75,
        'campus_visit_binary': 1,
        'email_opens': 5,
        'form_completion_time': 120,  # seconds
        'source_encoded': 2,  # Google Ads
        'fee_tier_encoded': 1  # Premium
    }
    """
    features = pd.DataFrame([lead_data])
    probability = lead_scoring_model.predict_proba(features)[0][1]

    priority = 'High' if probability > 0.6 else 'Medium' if probability > 0.3 else 'Low'

    return {
        'enrollment_probability': round(probability, 3),
        'priority': priority,
        'recommended_action': get_recommended_action(priority)
    }

def get_recommended_action(priority):
    actions = {
        'High': 'Immediate counselor call within 2 hours',
        'Medium': 'Email sequence + schedule call within 24 hours',
        'Low': 'Add to nurture campaign, no immediate action'
    }
    return actions[priority]
```

### Use Case 3: Cohort LTV Analysis
```python
def calculate_cohort_ltv():
    # Pull 5-year enrollment data
    conn = sqlite3.connect('marketing.db')
    df_students = pd.read_sql("""
        SELECT
            s.student_id,
            s.enrollment_date,
            s.brand,
            s.initial_fee,
            EXTRACT(YEAR FROM s.enrollment_date) as cohort_year,
            COUNT(DISTINCT p.payment_id) as years_retained,
            SUM(p.fee_amount) as total_fees_paid
        FROM students s
        LEFT JOIN fee_payments p ON s.student_id = p.student_id
        WHERE s.enrollment_date >= '2019-01-01'
        GROUP BY s.student_id, s.enrollment_date, s.brand, s.initial_fee
    """, conn)

    # Calculate cohort-level metrics
    cohort_summary = df_students.groupby(['cohort_year', 'brand']).agg({
        'student_id': 'count',
        'years_retained': 'mean',
        'total_fees_paid': 'mean',
        'initial_fee': 'mean'
    }).reset_index()

    cohort_summary.columns = ['cohort_year', 'brand', 'cohort_size',
                               'avg_retention_years', 'avg_ltv', 'avg_initial_fee']

    cohort_summary['ltv_to_initial_ratio'] = cohort_summary['avg_ltv'] / cohort_summary['avg_initial_fee']

    # Visualize
    pivot = cohort_summary.pivot(index='cohort_year', columns='brand', values='avg_ltv')
    pivot.plot(kind='bar', figsize=(12, 6))
    plt.title('Average LTV by Cohort Year and Brand')
    plt.ylabel('Average LTV')
    plt.xlabel('Enrollment Year')
    plt.legend(title='Brand')
    plt.tight_layout()
    plt.savefig('data/outputs/cohort_ltv_analysis.png', dpi=300)

    return cohort_summary
```

---

## 11. BEST PRACTICES & TIPS

### Code Organization
```python
# Good: Modular functions
def clean_hubspot_data(df):
    df = df.drop_duplicates(subset=['email'])
    df['created_date'] = pd.to_datetime(df['created_date'])
    df = df[df['email'].notna()]
    return df

def calculate_cpa(df):
    df['cpa'] = df['spend'] / df['enrollments']
    df['cpa_percent'] = (df['cpa'] / df['avg_fee']) * 100
    return df
```

### Error Handling
```python
# Always wrap risky operations in try/except
def scrape_with_retry(url, max_retries=3):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            return response
        except requests.exceptions.RequestException as e:
            if attempt == max_retries - 1:
                print(f"Failed after {max_retries} attempts: {e}")
                return None
            time.sleep(2 ** attempt)  # Exponential backoff
```

### Performance Optimization
```python
# Use vectorized operations (fast)
df['cpa'] = df['spend'] / df['enrollments']  # Vectorized

# Avoid loops (slow)
cpa_list = []
for i, row in df.iterrows():  # DON'T DO THIS
    cpa_list.append(row['spend'] / row['enrollments'])
```

### Version Control for Models
```python
from datetime import datetime
import json

# Save model with timestamp
model_version = datetime.now().strftime('%Y%m%d_%H%M%S')
model_path = f'models/lead_scoring_{model_version}.pkl'

with open(model_path, 'wb') as f:
    pickle.dump(model, f)

# Save metadata
metadata = {
    'model_version': model_version,
    'training_date': datetime.now().isoformat(),
    'features': features,
    'accuracy': accuracy_score(y_test, y_pred),
    'roc_auc': roc_auc_score(y_test, y_pred_proba)
}

with open(f'models/metadata_{model_version}.json', 'w') as f:
    json.dump(metadata, f, indent=2)
```

---

## 12. TROUBLESHOOTING

### Common Errors
```python
# KeyError: Column doesn't exist
# Fix: Check column names
print(df.columns.tolist())

# ValueError: Cannot convert string to float
# Fix: Clean data first
df['annual_fee'] = pd.to_numeric(df['annual_fee'], errors='coerce')

# MemoryError: Dataset too large
# Fix: Process in chunks
for chunk in pd.read_csv('large_file.csv', chunksize=10000):
    process(chunk)

# ModuleNotFoundError: No module named 'X'
# Fix: Install missing library
# pip install X
```

---

## 13. REFERENCE: LIBRARY CHEAT SHEET

### Pandas
```python
pd.read_csv()           # Load CSV
pd.read_excel()         # Load Excel
df.head(n)              # First n rows
df.describe()           # Summary stats
df.groupby()            # Aggregate
df.merge()              # Join tables
df.apply()              # Apply function to column
df.to_csv()             # Export to CSV
```

### NumPy
```python
np.mean()               # Average
np.median()             # Median
np.std()                # Standard deviation
np.where()              # Conditional selection
np.random.choice()      # Random sampling
```

### Matplotlib
```python
plt.plot()              # Line chart
plt.bar()               # Bar chart
plt.scatter()           # Scatter plot
plt.savefig()           # Save image
```

### Scikit-learn
```python
train_test_split()      # Split data
RandomForestClassifier()  # ML model
model.fit()             # Train
model.predict()         # Predictions
classification_report() # Evaluation
```
