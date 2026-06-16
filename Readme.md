# SQL Customer Dirty Data Creation Project

## 📌 Project Overview
This project focuses on creating a messy customer dataset using SQL for data cleaning practice.

The dataset intentionally contains:

Duplicate values
Invalid emails
Missing phone numbers
Inconsistent city names
Incorrect country values
Mixed date formats
Numeric outliers
Text values inside numeric columns

---

## 🗂️ Dataset Description

This dataset simulates customer information including:

- Customer ID  
- Name  
- Email  
- Phone number  
- City  
- Country  
- Signup date  
- Amount  

The data is intentionally messy.

---

## 📁 Project Structure

```id="proj1"
sql-customers-dirty-data-creation/
│
├── SQL/
│   └── customers_dirty_creation.sql
│
├── Data/
│   └── Raw/
│       └── customers_dirty.csv
│           
└── README.md
```

## 🏗️ Create Table

```sql
-- =========================================================
-- CREATE DIRTY CUSTOMER TABLE
-- =========================================================

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

-- =========================================================
-- INSERT MESSY CUSTOMER DATA
-- =========================================================

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

    -- =========================
    -- MESSY NAMES
    -- =========================
    CASE 
        WHEN i % 10 = 0 THEN '  Ali Khan  '
        WHEN i % 7 = 0 THEN 'Sara Ahmed'
        ELSE 'User ' || i
    END,

    -- =========================
    -- INVALID EMAILS
    -- =========================
    CASE 
        WHEN i % 15 = 0 THEN 'invalidemail.com'
        WHEN i % 9 = 0 THEN 'test@test.com'
        ELSE 'user' || i || '@gmail.com'
    END,

    -- =========================
    -- MESSY PHONE NUMBERS
    -- =========================
    CASE 
        WHEN i % 12 = 0 THEN NULL
        WHEN i % 5 = 0 THEN '123-456-789'
        ELSE '03' || (100000000 + i)::TEXT
    END,

    -- =========================
    -- INCONSISTENT CITY NAMES
    -- =========================
    CASE 
        WHEN i % 6 = 0 THEN 'karachi'
        WHEN i % 8 = 0 THEN 'LAHORE'
        WHEN i % 11 = 0 THEN ''
        ELSE 'Islamabad'
    END,

    -- =========================
    -- WRONG COUNTRY VALUES
    -- =========================
    CASE 
        WHEN i % 10 = 0 THEN 'IND'
        ELSE 'Pakistan'
    END,

    -- =========================
    -- MESSY DATE FORMATS
    -- =========================
    CASE 
        WHEN i % 13 = 0 THEN '12-05-2023'
        ELSE '2023-01-' || ((i % 28)+1)
    END,

    -- =========================
    -- OUTLIERS + TEXT VALUES
    -- =========================
    CASE 
        WHEN i % 20 = 0 THEN '999999'
        WHEN i % 17 = 0 THEN 'abc'
        ELSE (100 + i)::TEXT
    END

FROM generate_series(1,500) i;

-- =========================================================
-- 13. FINAL CLEANED DATA PREVIEW
-- =========================================================

select * 
from customers_dirty;
```

## Raw Data

[View Raw Dataset](Data/Raw/customers_dirty.csv)

---

## ⚠️ Intentional Data Issues

| Column       | Issue                     |
|--------------|--------------------------|
| name         | duplicates, extra spaces |
| email        | invalid + duplicates     |
| phone        | NULL + inconsistent format |
| city         | casing issues + blanks   |
| country      | wrong values             |
| signup_date  | mixed formats            |
| amount       | text + outliers          |

---

## 🛠️ SQL Concepts Used

- CREATE TABLE  
- INSERT INTO  
- CASE WHEN  
- generate_series()

---

## 👤 Author

Yasir Shah  
SQL Data Analytics Portfolio Project



---

# Excel Data Cleaning Project

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
Extra leading and trailing spaces in names.

### Example

| Before |
|----------|
|   Ali Khan   |

### Actions
- Removed unnecessary leading and trailing spaces.
- Standardized name formatting.

### Using

```excel
=TRIM(A2)
```

---

## Step 3: Clean Email Column

### Issues Found

**Invalid Emails**
- invalidemail.com

**Duplicate Emails**
- test@test.com appeared multiple times

### Actions
- Flagged invalid email addresses.
- Identified duplicate email records.
- Excluded duplicate emails from analysis where necessary.

### Examples

| Invalid Email | Duplicate Email |
|--------------|----------------|
| invalidemail.com | test@test.com |

---

## Step 4: Clean Phone Number Column

### Issues Found

**Invalid Format**
- 123-456-789

**Missing Values**
- NULL

### Actions
- Standardized phone number formatting.
- Converted NULL values to blank cells.
- Flagged invalid phone numbers for review.

### Examples

| Invalid Phone |
|--------------|
| 123-456-789 |

---

## Step 5: Clean City Column

### Issues Found

Different spellings and capitalization:

- Islamabad
- karachi
- LAHORE

### Actions
- Standardized city names.
- Applied proper capitalization.

### Standardized Values

- Islamabad
- Karachi
- Lahore

### Using

```excel
=PROPER(A2)
```

---

## Step 6: Clean Country Column

### Issues Found

- Pakistan
- IND

### Actions
- Standardized country names.
- Replaced abbreviations with full country names.

### Example

| Before | After |
|---------|--------|
| IND | India |

---

## Step 7: Handle Missing City Values

### Issues Found

Blank city records.

### Actions
- Identified missing city values.
- Replaced with "Unknown" or left blank according to business requirements.

---

## Step 8: Clean Amount Column

### Issues Found

**Non-Numeric Values**
- abc

**Potential Outliers**
- 999999

### Actions
- Converted Amount column to numeric format.
- Flagged non-numeric values.
- Investigated extreme outliers.

### Examples

| Invalid Value | Outlier |
|--------------|---------|
| abc | 999999 |

---

## Step 9: Validate Date Column

### Issues Found

Dates that could be interpreted differently:

- 1/2/2023
- 12/5/2023

### Actions
- Converted all dates to a consistent format.
- Standardized dates using ISO format.

### Standardized Format

```text
YYYY-MM-DD
```

### Examples

| Original | Standardized |
|-----------|-------------|
| 1/2/2023 | 2023-01-02 |
| 12/5/2023 | 2023-12-05 |

---

## Step 10: Check Duplicate Names

### Issues Found

Repeated names:

- Sara Ahmed
- Ali Khan

### Actions
- Verified records using Email and ID.
- Did not remove records because names alone do not indicate duplicates.

---

## Step 11: Verify ID Column

### Actions
- Checked for missing IDs.
- Checked for duplicate IDs.
- Confirmed primary key integrity.

---

# Project Summary

### Data Cleaning in Excel

✔ Removed extra spaces from name fields using TRIM().

✔ Identified and flagged invalid email addresses and duplicate email records.

✔ Standardized city and country values for consistency.

✔ Cleaned phone number formats and handled missing values.

✔ Converted date fields into a consistent format.

✔ Validated transaction amounts and flagged non-numeric values and outliers.

✔ Verified data integrity through duplicate and missing-value checks.

✔ Prepared a clean, analysis-ready dataset for reporting and dashboard creation.