# MySQL Tutorial

1. Database & Tables
2. Constraints & Data Types (ENUM, SET)
3. CRUD
4. Filtering & Sorting
5. Subqueries
6. Aggregate
7. NULL handling
8. Table data reset (DELETE vs TRUNCATE)
9. Indexes

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

## 🧱 Special Data Types

### ENUM Example (Ticket Table)

```sql
CREATE TABLE IF NOT EXISTS ticket (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(20) NOT NULL,
  description VARCHAR(50) NOT NULL,
  status ENUM('pending', 'open', 'close') NOT NULL DEFAULT 'pending'
);
```

---

### SET Example (Student Courses)

```sql
CREATE TABLE student_course (
  id INT PRIMARY KEY AUTO_INCREMENT,
  course_list SET(
    'NodeJS', 'NestJS', 'JS', 'NextJS',
    'ReactJS', 'MongoDB'
  )
);
```

#### Insert SET values

```sql
INSERT INTO student_course (course_list)
VALUES
('NodeJS,NodeJS,NodeJS,MongoDB,JS,NestJS'),
('NodeJS,NestJS,NestJS,MongoDB,JS,NestJS'),
('NodeJS,ReactJS,ReactJS,MongoDB,JS,NestJS'),
('JS,JS,JS,MongoDB,JS,JS');
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

### Insert Data

```sql
INSERT INTO student (firstname, lastname, national_code, bio)
VALUES
('Barbod', 'Masoudi', '0123456789', 'MERN Developer'),
('Elyas', 'Moludi', '12345678', 'Junior Developer');
```

---

### Select Data

```sql
SELECT * FROM student;
```

---

### Update Data

```sql
UPDATE student
SET age = 19
WHERE lastname = 'Moludi';
```

---

### Delete Data

```sql
DELETE FROM student
WHERE id = 2;
```

---

## 🗑️ DELETE vs TRUNCATE

```sql
DELETE FROM student_course;
```

```sql
TRUNCATE TABLE student_course;
```

---

## 🔎 Filtering Data (WHERE)

### Comparison

```sql
SELECT * FROM student
WHERE age >= 25 AND age <= 30;
```

---

### LIKE

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
SELECT * FROM student ORDER BY age ASC;
```

```sql
SELECT * FROM student ORDER BY age DESC;
```

```sql
SELECT * FROM student ORDER BY firstname ASC;
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
SELECT MAX(age) FROM student;
SELECT SUM(age) FROM student;
SELECT AVG(age) FROM student;
SELECT COUNT(id) FROM student;
```

---

## 🧩 NULL Handling

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

## 🚀 Indexes (Performance)

### Create table with index

```sql
CREATE TABLE contact (
  id INT PRIMARY KEY AUTO_INCREMENT,
  fullname VARCHAR(50) NOT NULL,
  mobile VARCHAR(20),
  INDEX (fullname)
);
```

### Create index

```sql
CREATE INDEX idx_mobile ON contact(mobile);
```

### Drop index

```sql
DROP INDEX idx_mobile ON contact;
```

---

## ✅ Notes

* Use `snake_case` naming convention
* `ENUM` → single value
* `SET` → multiple values
* `TRUNCATE` is faster than `DELETE` but irreversible
* Indexes improve read performance
* `IFNULL` and `COALESCE` handle `NULL` values
* `ANY` / `ALL` are used with subqueries


