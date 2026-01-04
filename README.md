
# MySQL Tutorial

## 📑 Table of Contents

* [Database Management](#-database-management)
* [Tables](#-tables)
* [Special Data Types (ENUM, SET)](#-special-data-types-enum-set)
* [Alter Table](#️-alter-table)
* [CRUD Operations](#-crud-operations)
* [DELETE vs TRUNCATE](#️-delete-vs-truncate)
* [Filtering Data (WHERE)](#-filtering-data-where)
* [Sorting Data (ORDER-by)](#-sorting-data-order-by)
* [Subqueries (IN, ANY, ALL)](#-subqueries-in-any-all)
* [Aggregate Functions](#-aggregate-functions)
* [NULL Handling](#-null-handling)
* [Triggers](#-triggers)
* [Indexes & Performance](#-indexes--performance)
* [Notes](#-notes)

---

## 📦 Database Management

```sql
SHOW DATABASES;
CREATE DATABASE IF NOT EXISTS nodejs;
USE nodejs;
DROP DATABASE IF EXISTS nodejs;
```

```sql
CREATE DATABASE IF NOT EXISTS nodejs
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

---

## 🧱 Tables

### Product

```sql
DROP TABLE IF EXISTS product;

CREATE TABLE product (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(100) NOT NULL,
  description VARCHAR(200) NOT NULL,
  summary TEXT
);
```

---

### City

```sql
DROP TABLE IF EXISTS city;

CREATE TABLE city (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) DEFAULT 'city_name'
);
```

---

### Student

```sql
DROP TABLE IF EXISTS student;

CREATE TABLE student (
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

## 🧱 Special Data Types (ENUM, SET)

### ENUM – Ticket Status

```sql
CREATE TABLE ticket (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(20) NOT NULL,
  description VARCHAR(50) NOT NULL,
  status ENUM('pending','open','close') DEFAULT 'pending'
);
```

---

### SET – Student Courses

```sql
CREATE TABLE student_course (
  id INT PRIMARY KEY AUTO_INCREMENT,
  course_list SET(
    'NodeJS','NestJS','JS','NextJS','ReactJS','MongoDB'
  )
);
```

```sql
INSERT INTO student_course (course_list)
VALUES
('NodeJS,NodeJS,MongoDB,JS'),
('NodeJS,NestJS,MongoDB'),
('ReactJS,JS,MongoDB'),
('JS,JS,JS');
```

---

## ✏️ Alter Table

```sql
ALTER TABLE product ADD details VARCHAR(85);
ALTER TABLE product DROP COLUMN details;
ALTER TABLE product MODIFY description VARCHAR(200);
```

---

## 🧩 CRUD Operations

### Insert

```sql
INSERT INTO student (firstname, lastname, national_code, bio)
VALUES
('Barbod','Masoudi','0123456789','MERN Developer'),
('Elyas','Moludi','12345678','Junior Developer');
```

### Select

```sql
SELECT * FROM student;
```

### Update

```sql
UPDATE student
SET age = 19
WHERE lastname = 'Moludi';
```

### Delete

```sql
DELETE FROM student WHERE id = 2;
```

---

## 🗑️ DELETE vs TRUNCATE

```sql
DELETE FROM student_course;
TRUNCATE TABLE student_course;
```

---

## 🔎 Filtering Data (WHERE)

```sql
SELECT * FROM student WHERE age BETWEEN 25 AND 30;
```

```sql
SELECT * FROM student WHERE firstname LIKE '%b%';
```

```sql
SELECT * FROM student WHERE city IN ('Mashhad','Tehran','Qom');
```

---

## 📊 Sorting Data (ORDER BY)

```sql
SELECT * FROM student ORDER BY age ASC;
SELECT * FROM student ORDER BY age DESC;
SELECT * FROM student ORDER BY firstname ASC;
```

---

## 🧠 Subqueries (IN, ANY, ALL)

```sql
SELECT * FROM student
WHERE age IN (SELECT age FROM student WHERE age BETWEEN 25 AND 30);
```

```sql
SELECT * FROM student
WHERE age < ANY (SELECT age FROM user);
```

```sql
SELECT * FROM student
WHERE age < ALL (SELECT age FROM user);
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

```sql
SELECT age + IFNULL(bio,1000) FROM student;
SELECT age + COALESCE(bio,1000) FROM student;
```

---

## 🔥 Triggers

### BEFORE INSERT Trigger

```sql
CREATE TRIGGER before_insert_user
BEFORE INSERT ON user
FOR EACH ROW
BEGIN
  IF NEW.city IS NULL THEN
    SET NEW.city = 'Tehran';
  END IF;
END;
```

---

### AFTER INSERT Trigger

```sql
CREATE TRIGGER after_insert_user
AFTER INSERT ON user
FOR EACH ROW
BEGIN
  INSERT INTO student(age)
  VALUES (NEW.age);
END;
```

---

### AFTER UPDATE Trigger (Validation)

```sql
CREATE TRIGGER after_update_user
AFTER UPDATE ON user
FOR EACH ROW
BEGIN
  IF (NEW.age > OLD.age) THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'new age is bigger than old one.';
  END IF;
END;
```

---

## 🚀 Indexes & Performance

```sql
CREATE TABLE contact (
  id INT PRIMARY KEY AUTO_INCREMENT,
  fullname VARCHAR(50) NOT NULL,
  mobile VARCHAR(20),
  INDEX (fullname)
);
```

```sql
CREATE INDEX idx_mobile ON contact(mobile);
DROP INDEX idx_mobile ON contact;
```

---

## ✅ Notes

* `ENUM` → one value
* `SET` → multiple values
* `TRIGGER` runs automatically on INSERT / UPDATE / DELETE
* `BEFORE` → data validation
* `AFTER` → logging / side effects
* `SIGNAL` is used to throw custom errors
* Indexes improve SELECT performance


