# SQL Rediscovery – University Of Mpumalanga Sessions

I studied SQL during my Diploma and Advanced Diploma in Applications Development at the University of Mpumalanga. This project is part of my personal revision—to see if I still remember SQL queries and core concepts.

To test myself, I asked ChatGPT to generate a sample table in CSV format. I then uploaded it into Copilot, which generated 20 SQL practice questions for me to work through, ranging from easy to advanced.

All queries were executed using **SQLite**, and this README documents the final queries, my debugging steps, and learning insights for each. I’m currently branching into **Data Analytics and Data Science**, where SQL is essential—so this is the first batch of revision queries. More are coming as I level up.

---

## 📁 Table Schema (from CSV import)

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






