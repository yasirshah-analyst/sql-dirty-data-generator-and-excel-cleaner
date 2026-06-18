# SQL Dirty Data Generation & Excel Data Cleaning Project

## 📌 Project Overview

This project demonstrates an end-to-end data quality workflow using SQL and Excel.

First, a dirty customer dataset containing common real-world data quality issues was generated in PostgreSQL using SQL scripts. The dataset was then exported to Excel, where a complete data cleaning process was performed to identify, correct, and validate data quality problems.

The project showcases both data generation and data cleaning skills commonly used by data analysts when preparing datasets for reporting, dashboard creation, and business analysis.

---

## 🎯 Project Objectives

- Generate a realistic dirty customer dataset using SQL.
- Simulate common data quality issues found in business datasets.
- Identify and clean inconsistent, incomplete, and invalid records.
- Standardize text, dates, and numeric fields.
- Handle missing values, duplicates, and formatting issues.
- Produce a clean dataset suitable for analysis and reporting.
- Demonstrate an end-to-end data cleaning workflow for a data analytics portfolio.

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

A total of **500 records** were generated with intentional data quality issues for cleaning practice.

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

# 1️⃣ SQL Dirty Data Generation

## Create Table

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

## 📥 Insert Dirty Data

The dataset was intentionally populated with common data quality issues using SQL `CASE WHEN` statements and `generate_series()`.

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

### Final Preview

```sql
SELECT *
FROM customers_dirty;
```

### Raw Dataset

[View Raw Dataset](Data/Raw/customers_dirty.csv)

---

### Simulated Data Quality Issues
The dataset was intentionally populated using SQL `CASE WHEN` statements and `generate_series()` to simulate common real-world data quality issues.

| Column | Issues Introduced |
|----------|----------|
| Name | Extra spaces, repeated names |
| Email | Invalid and duplicate emails |
| Phone | Missing values and invalid formats |
| City | Inconsistent capitalization and blank values |
| Country | Incorrect country codes |
| Signup Date | Mixed date formats |
| Amount | Non-numeric values and outliers |

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

### Issues Found

- Leading spaces

### Actions Taken

- Removed extra spaces using `TRIM()`

### Formula Used

```excel
=TRIM(B2)
```

### Cleaning Evidence

![Name Cleaning](cleaning/trim(b2).png)

---

## Step 3: Clean Email Column

### Issues Found

- Invalid email addresses
- Test/Fake Emails

### Examples

- invalidemail.com
- test@test.com

### Actions Taken

- Flagged invalid and Test/Fake email formats

### Formula Used

```excel
=if(C2="invalidemail.com","Invalid_Format",if(C2="test@test.com","Test/Fake_Email","Valid"))
```
  
### Cleaning Evidence

![Email Cleaning](cleaning/clean_email.png)

---

## Step 4: Clean Phone Number Column

### Issues Found

- Invalid phone formats
- Missing values

### Examples

- 123-456-789
- NULL

### Actions Taken

- Standardized phone number formats
- Converted NULL values to blanks
- Flagged invalid phone records

```excel
=IF(E2="NULL","Missing",IF((LEN(SUBSTITUTE(E2="-",""))<>11,"Invalid",E2))
```

### Cleaning Evidence
![Phone Cleaning](cleaning/raw_phone.png)

![Phone Cleaning](cleaning/raw_phone_2.png)

![Phone Cleaning](cleaning/raw_phone3.png)

![Phone Cleaning](cleaning/clean_phone_1.png)

![Phone Cleaning](cleaning/clean_phone_f.png)

---

## Step 5: Clean City Column

### Formula Used

```excel
=PROPER(IF(F2="","Missing",F2))
```

### Cleaning Evidence

![City Cleaning](cleaning/clean_city.png)

---

## Step 6: Clean Country Column

```excel
=PROPER(IF(G2="IND","India",G2))
```

### Cleaning Evidence

![Country Cleaning](cleaning/clean_country.png)

---

## Step 7: Handle Missing City Values

### Actions Taken

- Identified missing city values
- Replaced with "Unknown" where appropriate

```excel
=PROPER(IF(F2="","Missing",F2))
```

### Cleaning Evidence
![Country City](cleaning/clean_city.png)

---

## Step 8: Clean Amount Column

### Cleaning Evidence

![Amount Cleaning](cleaning/clean_amount_1.png)

![Amount Cleaning](cleaning/clean_amount_2.png)

![Amount Cleaning](cleaning/clean_amount_3.png)

---

## Step 9: Validate Date Column

### Final Format

```text
YYYY-MM-DD
```

### Cleaning Evidence

![Date Cleaning](cleaning/clean_date.png)

---


# 3️⃣ Cleaned Dataset

[View Clean Dataset](Data/clean/customers_clean.csv)

### Final Output

![Final Clean Dataset](cleaning/final_clean.png)

---

## ✅ Key Outcomes

- Generated a realistic dirty customer dataset using SQL.
- Identified and corrected data quality issues in Excel.
- Removed leading and trailing spaces using `TRIM()`.
- Flagged invalid and duplicate email records.
- Standardized phone number, city, and country formats.
- Handled missing values and inconsistent text entries.
- Converted dates into a consistent format.
- Validated numeric fields and investigated outliers.
- Verified record integrity using ID and Email fields.
- Produced a clean, analysis-ready customer dataset.

---

## 🛠️ Tools Used

- PostgreSQL
- WPS Excel

---

## 🧠 SQL Concepts Used

- CREATE TABLE
- INSERT INTO
- CASE WHEN
- generate_series()
- Data Quality Simulation

---

## 👤 Author

**Yasir Shah**

Data Analyst | SQL | Excel | Data Cleaning | Data Analytics
