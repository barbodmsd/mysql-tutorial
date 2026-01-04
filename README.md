# MySQL Tutorial

## 📦 Database Management

### Show all databases

```sql
SHOW DATABASES;
```

### Create a database if it doesn't exist

```sql
CREATE DATABASE IF NOT EXISTS nodejs;
```

### Select a database

```sql
USE nodejs;
```

### Drop a database if it exists

```sql
DROP DATABASE IF EXISTS nodejs;
```

### Create a database with specific charset and collation

```sql
CREATE DATABASE IF NOT EXISTS nodejs
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

---

## 🧱 Tables

### Product Table

```sql
DROP TABLE IF EXISTS product;

CREATE TABLE IF NOT EXISTS product (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(100) NOT NULL,
  description VARCHAR(200) NOT NULL,
  summary TEXT
);
```

---

### City Table

```sql
DROP TABLE IF EXISTS city;

CREATE TABLE IF NOT EXISTS city (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) DEFAULT 'city_name'
);
```

---

### Student Table

```sql
DROP TABLE IF EXISTS student;

CREATE TABLE IF NOT EXISTS student (
  id INT PRIMARY KEY AUTO_INCREMENT,
  firstname VARCHAR(50),
  lastname VARCHAR(50),
  national_code VARCHAR(10) UNIQUE NOT NULL,
  bio TEXT,
  age INT NOT NULL DEFAULT 18,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  CHECK (age >= 18)
);
```

---

## ✏️ Alter Table

### Add a column

```sql
ALTER TABLE product
ADD details VARCHAR(85);
```

### Drop a column

```sql
ALTER TABLE product
DROP COLUMN details;
```

### Modify a column

```sql
ALTER TABLE product
MODIFY COLUMN description VARCHAR(200);
```

---

## 🧩 CRUD Operations

### ➕ Insert Data

```sql
INSERT INTO student (firstname, lastname, national_code, bio)
VALUES
  ('Barbod', 'Masoudi', '0123456789', 'MERN Developer'),
  ('Elyas', 'Moludi', '12345678', 'Junior Developer');
```

---

### 🔍 Select Data

```sql
SELECT * FROM student;
```

---

### ✏️ Update Data

```sql
UPDATE student
SET age = 19
WHERE lastname = 'Moludi';
```

---

### ❌ Delete Data

```sql
DELETE FROM student
WHERE id > 2 AND id < 4;

DELETE FROM student
WHERE id = 2;
```

---

## 🔎 Filtering Data (WHERE)

### Comparison operators

```sql
SELECT * FROM student
WHERE age >= 25 AND age <= 30;
```

---

### LIKE (Pattern matching)

```sql
SELECT * FROM student
WHERE firstname LIKE '%b%';
```

```sql
SELECT * FROM student
WHERE city LIKE 'mashh_%';
```

---

### BETWEEN

```sql
SELECT * FROM student
WHERE age BETWEEN 25 AND 30;
```

---

### IN

```sql
SELECT * FROM student
WHERE city IN ('Mashhad', 'Tehran', 'Qom');
```

---

### Subquery with IN

```sql
SELECT * FROM student
WHERE age IN (
  SELECT age FROM student
  WHERE age BETWEEN 25 AND 30
);
```

---

## 📊 Sorting Data (ORDER BY)

```sql
SELECT * FROM student
ORDER BY age ASC;
```

```sql
SELECT * FROM student
ORDER BY age DESC;
```

```sql
SELECT * FROM student
ORDER BY firstname ASC;
```

---

## 🧠 Subqueries with ANY & ALL

### ANY

```sql
SELECT * FROM student
WHERE age < ANY (SELECT age FROM user)
ORDER BY age;
```

### ALL

```sql
SELECT * FROM student
WHERE age < ALL (SELECT age FROM user)
ORDER BY age;
```

---

## 🧮 Aggregate Functions

```sql
SELECT MIN(age) FROM student;
```

```sql
SELECT MAX(age) FROM student;
```

```sql
SELECT SUM(age) FROM student;
```

```sql
SELECT AVG(age) FROM student;
```

```sql
SELECT COUNT(id) FROM student;
```

---

## 🧩 NULL Handling Functions

### IFNULL

```sql
SELECT id, firstname, lastname, age + IFNULL(bio, 1000)
FROM student;
```

### COALESCE

```sql
SELECT id, firstname, lastname, age + COALESCE(bio, 1000)
FROM student;
```

---

## ✅ Notes

* Use `snake_case` for table and column names
* `AUTO_INCREMENT` values are not guaranteed to be sequential
* Always use `WHERE` with `UPDATE` and `DELETE`
* `utf8mb4` is recommended for full Unicode support
* `CHECK` constraints are supported in MySQL 8.0+
* `LIKE`, `BETWEEN`, `IN` are used for filtering data
* `ANY` and `ALL` are used with subqueries
* `IFNULL` and `COALESCE` help handle `NULL` values
* `ORDER BY` sorts query results
