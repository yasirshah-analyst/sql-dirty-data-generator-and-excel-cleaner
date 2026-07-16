# SQL Dirty Data Generation & Excel Data Cleaning Project

## 💼 Business Problem

Organizations rely heavily on customer data for critical decision-making, including:

- **Customer Analysis** & Segmentation
- **Revenue Tracking** & Financial Forecasting
- **Marketing Campaigns** & Retention Strategies
- **Performance Dashboards** & Executive Reporting

### The Challenge

In real-world business environments, raw customer data is rarely pristine. It is
frequently plagued by:

| Data Quality Issue | Business Impact |
|---|---|
| ❌ **Inconsistent Entries** | Fragmented metrics and skewed totals |
| ❌ **Incomplete Records** | Missing critical demographic or transactional context |
| ❌ **Incorrect Formatting** | Broken data types (e.g., text in date fields) preventing automated pipelines |
| ❌ **Invalid or Duplicate Values** | Artificial inflation of customer counts and wasted marketing spend |

These issues lead to inaccurate reporting, flawed analytics, and unreliable
business insights.

---

## 🎯 Project Goal

The goal of this project is to simulate a real-world, "dirty" customer dataset
and execute an end-to-end transformation. By leveraging data cleaning and
engineering best practices, this pipeline turns chaotic raw data into a clean,
optimized, and analysis-ready source of truth suitable for reliable business
reporting.

> **Scope note:** This project currently covers data generation and cleaning
> (SQL + Excel). A Power BI dashboard for visualizing the cleaned dataset is a
> planned next phase.

---

## ❓ Business Questions

1. What proportion of customer records contain unusable contact information (invalid/missing email or phone)?
2. How many records have inconsistent or missing city/country values that would break geographic reporting?
3. How many transaction amounts are invalid or statistical outliers?
4. What percentage of the raw dataset is fully clean and analysis-ready after processing?
5. Which data quality issue affects the largest share of records, and should be prioritized in the source system?

---

## 📌 Project Overview

This project demonstrates an end-to-end data quality workflow using SQL and Excel.

First, a dirty customer dataset containing common real-world data quality
issues was generated in PostgreSQL using SQL scripts. The dataset was then
exported to Excel, where a complete data cleaning process was performed to
identify, correct, and validate data quality problems.

The project showcases both data generation and data cleaning skills commonly
used by data analysts when preparing datasets for reporting, dashboard
creation, and business analysis.

---

## 🗂️ Dataset Description

The dataset contains customer information including:

- Customer ID
- Name
- Email
- Phone Number
- City
- Country
- Signup Date
- Amount

A total of **500 records** were generated with intentional data quality issues
for cleaning practice.

---

## 📁 Project Structure

```text
sql-customer-dirty-data-generator/
│
├── Data/
│   ├── Raw/
│   │   └── customers_dirty.csv
│   │
│   └── clean/
│       └── customers_clean.csv
│
├── SQL/
│   └── customers_dirty_creation.sql
│
├── cleaning/
│   ├── clean_amount_1.png
│   ├── clean_amount_2.png
│   ├── clean_amount_3.png
│   ├── clean_city.png
│   ├── clean_country.png
│   ├── clean_date.png
│   ├── clean_email.png
│   ├── clean_phone_1.png
│   ├── clean_phone_f.png
│   ├── final_clean.png
│   ├── raw_phone.png
│   ├── raw_phone3.png
│   ├── raw_phone_2.png
│   └── trim(b2).png
│
└── README.md
```

---

# Phase 1 — SQL: Dirty Data Generation

## Step 1: Create Table

### Purpose

The first step was to create a table to store customer information.

All fields were intentionally created as `TEXT` data types to allow insertion
of invalid values, inconsistent formats, and other data quality issues without
database restrictions.


```sql
CREATE TABLE customers_dirty (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT,
    phone TEXT,
    city TEXT,
    country TEXT,
    signup_date TEXT,
    amount TEXT
);
```

---

## Step 2: Insert Dirty Data

To replicate real-world scenarios, a PostgreSQL script was used to generate
500 customer records. The dataset was intentionally designed with common data
quality issues found in enterprise systems:

- inconsistent text formatting
- invalid email addresses
- missing phone numbers
- incorrect country codes
- mixed date formats
- unrealistic financial values and outliers

This simulates real challenges faced in CRM, sales, and customer analytics
systems.

### SQL Concept Used

- generate_series()
- INSERT INTO
- CASE WHEN

```sql
INSERT INTO customers_dirty (
    name,
    email,
    phone,
    city,
    country,
    signup_date,
    amount
)
SELECT

    -- Messy Names
    CASE
        WHEN i % 10 = 0 THEN '  Ali Khan  '
        WHEN i % 7 = 0 THEN 'Sara Ahmed'
        ELSE 'User ' || i
    END,

    -- Invalid Emails
    CASE
        WHEN i % 15 = 0 THEN 'invalidemail.com'
        WHEN i % 9 = 0 THEN 'test@test.com'
        ELSE 'user' || i || '@gmail.com'
    END,

    -- Invalid Phone Numbers
    CASE
        WHEN i % 12 = 0 THEN NULL
        WHEN i % 5 = 0 THEN '123-456-789'
        ELSE '03' || (100000000 + i)::TEXT
    END,

    -- Inconsistent Cities
    CASE
        WHEN i % 6 = 0 THEN 'karachi'
        WHEN i % 8 = 0 THEN 'LAHORE'
        WHEN i % 11 = 0 THEN ''
        ELSE 'Islamabad'
    END,

    -- Wrong Countries
    CASE
        WHEN i % 10 = 0 THEN 'IND'
        ELSE 'Pakistan'
    END,

    -- Mixed Date Formats
    CASE
        WHEN i % 13 = 0 THEN '12-05-2023'
        ELSE '2023-01-' || ((i % 28)+1)
    END,

    -- Outliers & Invalid Amounts
    CASE
        WHEN i % 20 = 0 THEN '999999'
        WHEN i % 17 = 0 THEN 'abc'
        ELSE (100 + i)::TEXT
    END

FROM generate_series(1,500) i;
```

## Step 3: Simulate Data Quality Issues

### Purpose

Real business data is rarely perfect.

The dataset was intentionally populated with common data quality problems that analysts frequently encounter.

### Issues Introduced

| Column | Data Quality Issue |
|----------|----------------|
| Name | Leading and trailing spaces |
| Email | Invalid and duplicate emails |
| Phone | Missing values and invalid formats |
| City | Inconsistent capitalization |
| Country | Incorrect country abbreviations |
| Signup Date | Mixed date formats |
| Amount | Invalid values and outliers |

---

### Final Preview

```sql
SELECT *
FROM customers_dirty;
```

### Raw Dataset

[View Raw Dataset](Data/Raw/customers_dirty.csv)

---

# 2️⃣ Excel Data Cleaning Process

## Step 1: Inspect the Dataset

### Checked all columns for:

- Missing values
- Duplicate records
- Invalid formats
- Inconsistent text
- Outliers
- Incorrect data types

---

## Step 2: Clean Name Column

### Issue Identified

Some customer names contained unnecessary leading and trailing spaces.

### Example

Before:

```text
"  Ali Khan  "
```

After:

```text
"Ali Khan"
```

### Formula Used

```excel
=TRIM(B2)
```

### Why?

TRIM removes extra spaces and improves consistency.

### Cleaning Evidence

![Name Cleaning](cleaning/trim(b2).png)

---

## Step 3: Clean Email Column

### Issues Identified

- Invalid email addresses
- Test email accounts

### Examples

```text
invalidemail.com
test@test.com
```

### Formula Used

```excel
=IF(C2="invalidemail.com","Invalid_Format",
IF(C2="test@test.com","Test/Fake_Email","Valid"))
```

### Why?

Email validation helps identify records requiring correction before communication or analysis.

### Cleaning Evidence

![Email Cleaning](cleaning/clean_email.png)

---

## Step 4: Review Phone Number Column

### Issues Identified

- Missing values
- Invalid phone formats
- Inconsistent formatting
- Excel "Number Stored as Text" warnings (green triangles)

### Why Were Green Triangles Appearing?

The phone number column was imported as text, causing Excel to display green error indicators with the message:

```text
Number Stored as Text
```

Phone numbers are identifiers rather than values used for mathematical calculations. Therefore, storing them as text is the recommended approach because it preserves leading zeros and prevents unwanted formatting changes.

### Remove Green Triangle Warnings

To improve worksheet readability:

1. File
2. Options
3. Formulas
4. Error Checking Rules
5. Uncheck:

```text
Numbers Formatted as Text or Preceded by an Apostrophe
```

6. Click OK

### Examples

```text
NULL
123-456-789
03123456789
```

### Actions Taken

- Removed Excel warning indicators
- Identified missing phone numbers
- Validated phone number length
- Flagged invalid phone formats

### Formula Used

```excel
=IF(E2="NULL",
"Missing",
IF(LEN(SUBSTITUTE(E2,"-",""))<>11,
"Invalid",
E2))
```

### Why?

The formula:

- Identifies missing phone numbers
- Removes dashes for validation
- Checks whether the phone number contains 11 digits
- Flags invalid records for further review

### Cleaning Evidence

![Phone Cleaning](cleaning/raw_phone.png)

![Phone Cleaning](cleaning/raw_phone_2.png)

![Phone Cleaning](cleaning/raw_phone3.png)

![Phone Cleaning](cleaning/clean_phone_1.png)

![Phone Cleaning](cleaning/clean_phone_f.png)

---

## Step 5: Clean and Standardize City Names

### Issues Identified

- Inconsistent capitalization
- Missing city values (blank cells)

### Examples

Before Cleaning:

```text
karachi
LAHORE
Islamabad
(blank)
```

### Formula Used

```excel
=PROPER(IF(F2="","Missing",F2))
```

### Actions Taken

- Replaced blank city values with "Missing"
- Standardized city names into Proper Case format

### Why?

The formula performs two cleaning tasks:

**1. Handle Missing Values**

```excel
IF(F2="","Missing",F2)
```

Checks whether the city cell is blank.

If blank, it replaces the value with:

```text
Missing
```

This makes missing records easier to identify during analysis instead of leaving empty cells.

**2. Standardize Text Format**

```excel
PROPER(...)
```

Converts city names into Proper Case.

Examples:

```text
karachi   → Karachi
LAHORE    → Lahore
islamabad → Islamabad
```

Standardized city names improve:

- Data consistency
- Filtering and sorting
- Pivot Tables
- Grouping and aggregation
- Dashboard reporting accuracy

### Cleaning Evidence

![City Cleaning](cleaning/clean_city.png)

---

## Step 6: Standardize Country Names

### Issue Identified

Country abbreviations:

```text
IND
```

### Formula Used

```excel
=PROPER(IF(G2="IND","India",G2))
```

### Why?

Ensures consistent country naming for analysis.

### Cleaning Evidence

![Country Cleaning](cleaning/clean_country.png)

---

## Step 8: Clean and Validate Amount Column

### Issues Identified

- Non-numeric values
- Potential outliers
- Incorrect data types

### Examples

Invalid Values:

```text
abc
```

Potential Outliers:

```text
999999
```

### Formula Used

```excel
=IF(AND(ISNUMBER(I2),I2<10000),I2,"")
```

### Actions Taken

- Identified valid numeric values
- Removed non-numeric entries
- Flagged extreme outliers
- Converted text values to numeric format
- Standardized decimal formatting

### Why?

The formula:

```excel
=IF(AND(ISNUMBER(I2),I2<10000),I2,"")
```

performs two validation checks:

**1. Numeric Validation**

```excel
ISNUMBER(I2)
```

Checks whether the value is numeric.

Examples:

```text
150   → Valid
abc   → Invalid
```

**2. Outlier Validation**

```excel
I2 < 10000
```

Checks whether the amount falls within a reasonable range.

Examples:

```text
450    → Valid
999999 → Outlier
```

Only values that pass both conditions are retained.

If either condition fails, the formula returns a blank value.

This helps ensure that only valid transaction amounts are kept for analysis.

### Data Type Conversion

After validating the values, the Amount column was converted from **Text** to **Number** format.

### Why?

Numeric data types are required for:

- SUM calculations
- Averages
- Revenue analysis
- Pivot Tables
- Charts and dashboards

Without converting the data type, Excel may treat numbers as text and calculations may produce incorrect results.

### Decimal Standardization

After converting the column to a numeric data type, unnecessary decimal places were reduced.

Example:

```text
125.000000 → 125
250.500000 → 250.5
```

### Why?

Reducing excessive decimal places:

- Improves readability
- Makes reports cleaner
- Produces more professional dashboards
- Prevents visual clutter

### Cleaning Evidence

**Validation of Numeric Values and Outliers**

![Amount Cleaning](cleaning/clean_amount_1.png)

**Conversion from Text to Number Data Type**

![Amount Cleaning](cleaning/clean_amount_2.png)

**Standardized Decimal Formatting**

![Amount Cleaning](cleaning/clean_amount_3.png)

---

## Step 9: Standardize Date Formats

### Issue Identified

Mixed date formats:

```text
2023-01-15
12-05-2023
```

### Final Format

```text
YYYY-MM-DD
```

### Why?

Consistent dates are required for trend analysis and reporting.

### Cleaning Evidence

![Date Cleaning](cleaning/clean_date.png)

---

# 3️⃣ Final Clean Dataset

After all cleaning steps were completed, the dataset became:

✅ Consistent

✅ Standardized

✅ Analysis-ready

### Final Output

![Final Dataset](cleaning/final_clean.png)

---

# 📊 Data Quality Issues Resolved

| Issue | Status |
|---------|---------|
| Leading Spaces | ✅ Fixed |
| Invalid Emails | ✅ Flagged |
| Missing Phones | ✅ Identified |
| Invalid Phones | ✅ Flagged |
| Inconsistent Cities | ✅ Standardized |
| Incorrect Countries | ✅ Standardized |
| Mixed Dates | ✅ Standardized |
| Invalid Amounts | ✅ Investigated |
| Missing Values | ✅ Handled |

---

## 📈 Business Impact

After cleaning the dataset:
* **Data accuracy was significantly improved** for reporting purposes
* **Invalid, missing, and inconsistent records** were identified and handled
* **Dataset became suitable** for business intelligence dashboards and KPI reporting
👉 **This ensures that business stakeholders can make decisions based on clean and trustworthy data.**

# 🧠 SQL Concepts Used

- CREATE TABLE
- INSERT INTO
- CASE WHEN
- generate_series()
- Data Quality Simulation

---

# 📈 Skills Demonstrated

- Data Cleaning
- Data Validation
- Data Quality Assessment
- Excel Functions
- SQL Data Generation
- Data Preparation
- Business Data Standardization

---

# 🛠️ Tools Used

- PostgreSQL
- WPS Excel

---

## 🔄 Project Update: Power BI Extension (In Progress)

This project is being extended beyond SQL + Excel cleaning into a full **Power BI analysis**, covering the complete process from problem definition to business insights and recommendations.

### Status: 🚧 In Progress

Currently building two report pages:
- **Page 1 — Data Quality Impact:** quantifies how much the raw (dirty) data distorted customer counts and revenue reporting, and documents every category of issue found (invalid amounts, invalid/test emails, missing/invalid phone formats, blank cities).
- **Page 2 — Customer & Revenue Insights:** analyzes the *cleaned* data to answer real business questions — which cities generate the most revenue, which have the highest average transaction value, and where the biggest customer base is concentrated.

### 🐞 Data Issue Found & Fix In Progress

While building Page 2, a new data quality issue was discovered that wasn't caught during the original SQL/Excel cleaning stage:

**Issue:** Every record where `country = India` had a **Pakistani city** (Islamabad, Karachi, or Lahore) instead of an Indian one.

**Root cause:** The original SQL script's random data generator selected `city` and `country` independently of each other, rather than pairing valid city–country combinations. As a result, any row randomly assigned "India" as its country still pulled from the same fixed city list, which only ever contained Pakistani cities.

**Fix:** Since every "India" row was consistently paired with a real Pakistani city, this confirms the **country** field — not the city — was the incorrect value. The fix is being applied in **Power Query** (Power BI) using a `Replace Values` step: `India → Pakistan`.

This decision was made deliberately at the Power Query stage (rather than re-editing the original CSV/Excel source) to keep the transformation logic visible, documented, and reproducible as part of the Power BI data model — consistent with this project's goal of showing the *full* analytics process, including issues found and corrected along the way, not just a polished final result.

### Why this is documented here
This project isn't meant to show only clean, pre-planned steps — it's meant to reflect a genuine, real-world analytics workflow, where new issues can surface even after earlier cleaning stages, and part of the job is catching them, diagnosing the root cause, and deciding where in the pipeline to fix them.

---
*This section will be updated as the Power BI portion of the project progresses.*

---

# 👤 Author

**Yasir Shah**

Data Analyst | SQL | Excel
