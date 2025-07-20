# SQL Rediscovery – Mpumalanga Sessions

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

