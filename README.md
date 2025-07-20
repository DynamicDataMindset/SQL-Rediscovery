# SQL Rediscovery – University Of Mpumalanga Sessions

I studied SQL during my Diploma and Advanced Diploma in Applications Development at the University of Mpumalanga. This project is part of my personal revision—to see if I still remember SQL queries and core concepts.

To test myself, I asked ChatGPT to generate a sample table in CSV format. I then uploaded it into Copilot, which generated 20 SQL practice questions for me to work through, ranging from easy to advanced.

All queries were executed using **SQLiteonline** (https://sqliteonline.com), and this README documents the final queries, my debugging steps, and learning insights for each. I’m currently branching into **Data Analytics and Data Science**, where SQL is essential—so this is the first batch of revision queries. More are coming as I level up.

---

## 📁 Table Schema (from CSV import) 📄 [Download the dataset (CSV)](https://drive.google.com/file/d/1cmU3Gq4SIsGLyn5jCRk3w7LLqkk8NwdX/view?usp=sharing)

- `first_name`
- `last_name`
- `salary`
- `hire_date` (`YYYY-MM-DD`)
- `department`
- `location`
- `job_title`

---

## 🎓 SQL Practice Questions and Solutions

### 1️⃣ Show all employees

```sql
SELECT * FROM employees;
```
Basic full-table exploration.

### 2️⃣ Show first and last names of employees
```sql
SELECT first_name, last_name FROM employees; 
```
Focuses on identification columns only

### 3️⃣ Show employees hired before 2020
```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE hire_date < '2020-01-01';
```
🛠 Debug Insight:
Originally tried  `'2020/01/01' `, which didn’t match SQLite’s expected  `YYYY-MM-DD ` format. I corrected this by inspecting the data with:
```sql
SELECT DISTINCT hire_date FROM employees;
```
### 4️⃣ Show employees with salary greater than 80,000
```sql
SELECT first_name, last_name, salary
FROM employees
WHERE salary > 80000;
```
### 5️⃣ List employees sorted by salary (ascending)
```sql
SELECT first_name, last_name, salary
FROM employees
ORDER BY salary ASC;
```
### 6️⃣ Top 5 highest-paid employee
```sql
SELECT first_name, last_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 5;
```
### 7️⃣ Most recently hired employee
```sql
SELECT first_name, last_name, hire_date
FROM employees
ORDER BY hire_date DESC;
```
### 8️⃣ Marketing employees sorted by last name
```sql
SELECT first_name || ' ' || last_name AS full_name, department
FROM employees
WHERE department = 'Marketing'
ORDER BY last_name ASC;
```
✅ Used SQLite's concatenation operator `||` to format full name.

### 9️⃣ Employees with "Engineer" in job title
```sql
SELECT first_name || ' ' || last_name AS full_name, job_title
FROM employees
WHERE job_title LIKE '%Engineer%';
```
✅ Used LIKE `'%Engineer%'` for pattern matching across job titles.

### 🔟 First 3 employees from New York
```sql
SELECT first_name || ' ' || last_name AS full_name, location
FROM employees
WHERE location LIKE '%New York%'
LIMIT 3;
```
📍 Could’ve used WHERE location = 'New York' if exact matches were guaranteed

### 1️⃣1️⃣ Employees earning between 60k and 90k
```sql
SELECT first_name || ' ' || last_name AS full_name, salary
FROM employees
WHERE salary BETWEEN 60000 AND 90000;
```
🔍 Explanation:
- Filters employees whose salary is between `60,000` and `90,000` (inclusive).
- Uses BETWEEN for clear range logic. Equivalent to:
```sql
WHERE salary >= 60000 AND salary <= 90000;
```
- Combines `first_name` and `last_name` with `||` for full name formatting.
- Output is ideal for mid-tier compensation analysis or banding reports.
  
### 12️⃣ Employees with the lowest salary (all ties)
```sql
SELECT first_name || ' ' || last_name AS full_name, salary
FROM employees
WHERE salary = (SELECT MIN(salary) FROM employees);
```
✅ Used a subquery for dynamic retrieval, not hardcoding lowest salary.

### 13️⃣ Engineering employees earning over 90,000
```sql
SELECT first_name || ' ' || last_name AS full_name, salary, department
FROM employees
WHERE department = 'Engineering' AND salary > 90000;
```
🔄 Combined multiple conditions with `AND`

### 14️⃣ Last names of employees hired after 2020
```sql
SELECT last_name, hire_date
FROM employees
WHERE hire_date >= '2020-01-01'
ORDER BY hire_date ASC;
```
🛠 Date Format Reminder:
Using `'2020-01-01'` (not `'2020/01/01'`) for compatibility with SQLite sorting.

### 15️⃣ Show all columns but skip first 5 rows
```sql
SELECT * 
FROM employees 
LIMIT 5 OFFSET 5;
```
🧭 Pagination Tip:
Shows rows 6–10 in a dataset. `OFFSET` skips first 5; `LIMIT` fetches next 5

### 1️⃣6️⃣ Find employees whose job titles start with 'Senior'
```sql
SELECT first_name || ' ' || last_name AS full_name, job_title
FROM employees
WHERE job_title LIKE 'Senior%';
```
🔍 Explanation:
- `LIKE 'Senior%'` matches any job titles that begin with “Senior” (e.g., Senior Developer, Senior Analyst).
- Uses SQLite string matching and formatting with `||` to combine first and last name

### 1️⃣7️⃣ Show the number of employees hired in each year
```sql
SELECT strftime('%Y', hire_date) AS hire_year, COUNT(*) AS hires
FROM employees
GROUP BY hire_year
ORDER BY hire_year ASC;
```
🔍 Explanation:
- Uses SQLite’s `strftime('%Y', hire_date)` to extract the `year` from each hire date.
- Groups results by year and counts how many employees joined in each.
- Sorted chronologically for time-based hiring trends.

### 1️⃣8️⃣ Count how many employees work in each department
```sql
SELECT department, COUNT(*) AS number_of_employees
FROM employees
GROUP BY department
ORDER BY department ASC;
```
🔍 Explanation:
- `GROUP BY` department clusters records based on their department label.
- `COUNT(*)` calculates the number of employees in each group.
- `AS` number_of_employees renames the result column for clarity.
- `ORDER BY` department ASC sorts the output alphabetically by department name.
🛠️ Debugging Insight:
At first, I tried mixing individual employee names with aggregation like this
```sql
SELECT first_name || ' ' || last_name AS full_name, department
COUNT department
ORDER BY department ASC;
```
But this:
- Placed `COUNT` outside the `SELECT` clause,
- Didn't include a proper `GROUP BY`,
- Mixed row-level details with aggregation logic—causing errors.
I corrected it by focusing solely on `department` and using `GROUP BY` correctly with `COUNT(*)`.
📘 Real-World Application:
This query is perfect for:
- Headcount tracking
- Departmental growth audits
- Visual dashboards showing team sizes

### 1️⃣9️⃣ Total salary paid to employees in the Engineering department
```sql
SELECT department, SUM(salary) AS total_salary
FROM employees
WHERE department = 'Engineering'
GROUP BY department;
```
🔍 Explanation:
- Filters employees in the Engineering department.
- Calculates their total salary using `SUM(salary)`.
- Groups the result by department for consistent formatting.
🛠️ Fixes I made:
- Originally wrote `ADD(*)` (invalid in SQL).
- Grouped by `salary`, which split totals incorrectly.
- Switched to `SUM(salary)` and grouped by `department` to fix both

### 2️⃣0️⃣ Calculate the average salary of employees in the Marketing department
```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE department = 'Marketing'
GROUP BY department;
```
🔍 Explanation:
- Filters employees from Marketing.
- Uses `AVG(salary)` to get the department’s average pay.
- Grouped for consistent results.
📘 Use Case:
Helps assess typical pay scales within specific teams. Can be adapted to use `MAX()`, `MIN()`, or other aggregates for advanced analytics.

## 🧩 Personal Reflections & SQL Clauses Explored

Looking back on this first revision batch, I didn't just test whether I still understood SQL—I actively challenged my memory, re-learned concepts, and explored better ways to write clean, structured queries. Each solution pushed me to revisit key clauses and understand how they behave in **SQLite**, especially when layered together.

### ✅ Clauses and Enhancements I Practiced

- **Alias usage:**  
  I used `AS` consistently to make outputs more readable—like `AS full_name`, `AS total_salary`, and `AS avg_salary`. This helped me format cleaner results for reporting.

- **Pattern matching with LIKE:**  
  I explored matching logic with `LIKE '%Engineer%'` for job titles, and `LIKE '%New York%'` for location filtering. These taught me how flexible `LIKE` can be when working with partial strings.

- **Subqueries:**  
  I used `(SELECT MIN(salary) FROM employees)` to dynamically retrieve employees earning the lowest salary—without manually scanning the table or hardcoding values.

- **Pagination using LIMIT and OFFSET:**  
  I applied `LIMIT 5 OFFSET 5` to skip rows and control query output size. This is perfect for scenarios where you'd build page-based views or feed data to a frontend.

### 💡 SQLite-Specific Notes I Discovered

## 🛠️ Tools Used

During this revision journey, I wrote and tested all SQL queries using the **SQLite Online** editor (https://sqliteonline.com). It’s lightweight, accessible, and perfect for revisiting syntax and logic without needing a full local setup.

I specifically used **SQLite** as my query engine, which comes with its own syntax rules and quirks:
- Date filtering using `'YYYY-MM-DD'` formats
- Easy support for `LIMIT`, `OFFSET`, `BETWEEN`, and subqueries

As I continue building future sets, I may switch to other engines (e.g., PostgreSQL, MySQL) to compare behaviors—but for this revision batch, SQLite Online was ideal for fast, clean testing.

---

## 🧠 What This Set Meant

This revision batch wasn’t just about solving 20 queries—it was a personal rediscovery of the SQL foundation I learned at university, now reframed with a practical lens for analytics. Every mistake I debugged taught me something I’ll carry into future datasets and dashboards.

I'm now even more motivated to build real-world analytical systems and share what I learn with others.

---

## 📅 Roadmap: What’s Coming Next

This is just the beginning. The next README sets will explore deeper SQL topics like:

- Advanced filtering: `IN`, `NOT IN`, `EXISTS`
- Joins: `INNER`, `LEFT`, `RIGHT`
- Common Table Expressions (CTEs)
- Subqueries inside `SELECT` and `FROM`
- Window functions: `ROW_NUMBER`, `RANK`, `OVER`
- Performance tuning with indexing
- Real-world logic for dashboards, attendance summaries, and churn prediction

Each set will include detailed documentation, explanations, and growth notes—just like this one.

---

## 🙏 Final Note

This project reflects both my past training and my future direction. SQL is the backbone of data analytics, and this repo is my open learning archive as I advance toward **data science**, **business intelligence**, and **AI-enhanced systems**.

Thanks for walking with me through this first chapter. Feedback, forks, and collaboration are all welcome as the journey continues!

*― Boniface Ramushu*

---





