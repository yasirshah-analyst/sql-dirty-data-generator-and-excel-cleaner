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
│   └── Raw Data/
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

[View Raw Dataset](Data/Raw Data/customers_dirty.csv.csv)

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

## ⭐ Purpose

This project is part of my data analytics learning journey to practice:

- SQL Data Generation
- Portfolio building for analytics roles
