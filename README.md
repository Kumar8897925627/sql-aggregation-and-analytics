# sql-aggregation-and-analytics
"Statistical data analysis using PostgreSQL. Implementing Aggregate functions (SUM, AVG, COUNT), grouping data with GROUP BY, and mastering the HAVING clause for filtered analytics."
-- ==========================================================
-- PROJECT: Aggregate Functions & Grouping (Task 5)
-- CONCEPTS: COUNT, SUM, AVG, MIN, MAX, GROUP BY, HAVING
-- ==========================================================

-- 1. BASIC AGGREGATES
-- Get a snapshot of the entire student body
SELECT 
    COUNT(*) AS total_students,
    AVG(grade_point_average) AS average_gpa,
    MIN(grade_point_average) AS lowest_gpa,
    MAX(grade_point_average) AS highest_gpa
FROM students;

-- 2. GROUPING DATA (GROUP BY)
-- Count how many students were born in each year
SELECT 
    EXTRACT(YEAR FROM date_of_birth) AS birth_year, 
    COUNT(*) AS student_count
FROM students
GROUP BY birth_year
ORDER BY birth_year;

-- 3. FILTERING GROUPS (HAVING)
-- Find birth years that have more than 1 student
-- (Note: WHERE filters rows, HAVING filters groups)
SELECT 
    EXTRACT(YEAR FROM date_of_birth) AS birth_year, 
    COUNT(*) AS student_count
FROM students
GROUP BY birth_year
HAVING COUNT(*) > 1;

-- 4. AGGREGATES WITH NULLS
-- COUNT(*) counts all rows; COUNT(column) ignores NULLs
SELECT 
    COUNT(*) AS total_rows, 
    COUNT(grade_point_average) AS records_with_gpa
FROM students;

-- 5. REAL-WORLD SCENARIO: Performance Categories
-- Grouping students by a custom range (Logic practice)
SELECT 
    CASE 
        WHEN grade_point_average >= 3.5 THEN 'Honor Roll'
        WHEN grade_point_average >= 3.0 THEN 'Satisfactory'
        ELSE 'Needs Improvement'
    END AS academic_standing,
    COUNT(*) AS student_count,
    ROUND(AVG(grade_point_average), 2) AS category_avg_gpa
FROM students
GROUP BY academic_standing;

# 📈 SQL Aggregations & Grouping

This module focuses on statistical analysis. Instead of looking at individual students, we look at the patterns across the entire dataset.

## 🧮 Core Aggregate Functions
- **`COUNT()`**: Returns the number of rows.
- **`SUM()`**: Adds up numeric values.
- **`AVG()`**: Calculates the mean.
- **`MIN()` / `MAX()`**: Finds the smallest and largest values.



## 🧩 The GROUP BY Logic
Grouping allows us to summarize data into categories. For example, instead of seeing every student's birthday, we can see how many students belong to each birth year.

## ⚖️ WHERE vs. HAVING (The Big Difference)
This is a critical distinction in SQL:
- **`WHERE`**: Filters individual **rows** *before* any grouping happens.
- **`HAVING`**: Filters **groups** *after* the `GROUP BY` has been processed.



## 🛠️ Performance Tip: NULLs
Aggregate functions like `AVG` and `SUM` automatically ignore `NULL` values. However, `COUNT(*)` counts the row regardless of nulls, while `COUNT(column_name)` only counts non-null entries.
