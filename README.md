# Advanced SQL Analysis, Ranking, and Automation

This project demonstrates advanced SQL techniques for analyzing employee data, performing ranking, rolling aggregates, and database automation using **MySQL**.

---

## 🚀 Features

- **Employee Ranking & Analysis:**  
  Rank employees by salary using `RANK()` and `ROW_NUMBER()` window functions.  
- **Rolling Aggregates:**  
  Calculate running totals of salaries partitioned by gender.  
- **Modular Queries:**  
  Use **Common Table Expressions (CTEs)** for clean, reusable queries.  
- **Database Automation:**  
  Implement **Stored Procedures**, **Triggers**, and **Events** for automated database management.  
- **Department-Based Analysis:**  
  Compare employee salaries against department averages.  
- **Age & High Salary Insights:**  
  Analyze employees over 30 or high earners and join with department data.

---

## 🛠 Built With

- **MySQL**
- **Window Functions:** `RANK()`, `ROW_NUMBER()`
- **CTEs (Common Table Expressions)**
- **Stored Procedures**
- **Triggers & Events**

---

## 📂 Project Structure


---

## 💻 SQL Examples

### 1. Employee Ranking by Gender

```sql
SELECT 
    ed.employee_id, 
    ed.first_name, 
    ed.last_name, 
    ed.gender,
    es.salary,
    ROW_NUMBER() OVER(PARTITION BY ed.gender ORDER BY es.salary DESC) AS row_num,
    RANK() OVER(PARTITION BY ed.gender ORDER BY es.salary DESC) AS rank_num
FROM employee_demographics ed
JOIN employee_salary es 
    ON ed.employee_id = es.employee_id;
```

### 2. Rolling Salary Total by Gender

```sql
SELECT gender, SUM(salary) OVER(PARTITION BY gender ORDER BY ed.employee_id) AS Rolling_Total 
FROM employee_demographics ed
JOIN employee_salary es ON ed.employee_id = es.employee_id;
```

### 3. Employees Above Average Salary

```sql
WITH average_salary AS (
    SELECT AVG(salary) AS avg_sal FROM employee_salary
)
SELECT 
    es.employee_id, 
    es.first_name, 
    es.last_name, 
    es.salary 
FROM employee_salary es 
JOIN average_salary a 
ON es.salary > a.avg_sal;
```

### 4. Department Salary Analysis

```sql
WITH dept_avg AS (
    SELECT dept_id, AVG(salary) AS avg_dept_salary FROM employee_salary GROUP BY dept_id
)
SELECT 
    es.employee_id,
    es.first_name,
    pd.department_name,
    es.salary,
    da.avg_dept_salary,
    (es.salary - da.avg_dept_salary) AS difference_from_avg
FROM employee_salary es 
JOIN parks_departments pd ON es.dept_id = pd.department_id
JOIN dept_avg da ON es.dept_id = da.dept_id;
```

### 5. Stored Procedure Example

```sql
CREATE PROCEDURE large_salaries()
BEGIN
    SELECT * FROM employee_salary WHERE salary >= 50000;
    SELECT * FROM employee_salary WHERE salary >= 10000;  
END;

CALL large_salaries();
```

### 6. Trigger Example

```sql
CREATE TRIGGER employee_insert
AFTER INSERT ON employee_salary
FOR EACH ROW 
BEGIN
    INSERT INTO employee_demographics (employee_id, first_name, last_name) 
    VALUES (NEW.employee_id, NEW.first_name, NEW.last_name);
END;
```

### 7. Event Example

```sql
CREATE EVENT delete_retirees
ON SCHEDULE EVERY 30 SECOND
DO 
BEGIN
    DELETE FROM employee_demographics WHERE age >= 60;
END;
```

## 📥 How to Use

### 1. Clone this repository

git clone https://github.com/yourusername/advanced-sql-analysis.git


### 2. Import SQL files into MySQL

mysql -u your_user -p < sql/employee_demographics.sql
mysql -u your_user -p < sql/employee_salary.sql

## Run analysis queries in analysis_queries.sql to explore insights.


⚡ Insights

Ranking and rolling totals help identify top performers and trends.

CTEs make queries modular and reusable.

Triggers, events, and stored procedures automate repetitive database tasks.

Department-based salary comparison highlights high earners and salary gaps.

📸 Optional Screenshots

Add screenshots of query outputs here for better visualization.

🔖 License

This project is open-source and free to use for learning and demonstration purposes.

If you want, I can also **add a “Project Demo / Screenshots Section” with images of outputs** in README so it looks like a professional portfolio piece.  

Do you want me to do that?

