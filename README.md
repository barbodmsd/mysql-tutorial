# 📘 MySQL Tutorial

A complete, clean, and step-by-step MySQL tutorial for learning database fundamentals.

---

## 📑 Table of Contents

* [Database Management](#-database-management)
* [Tables](#-tables)
* [Special Data Types (ENUM, SET)](#-special-data-types-enum-set)
* [Alter Table](#️-alter-table)
* [CRUD Operations](#-crud-operations)
* [DELETE vs TRUNCATE](#️-delete-vs-truncate)
* [Filtering Data (WHERE)](#-filtering-data-where)
* [Sorting Data (ORDER BY)](#-sorting-data-order-by)
* [Subqueries (IN, ANY, ALL)](#-subqueries-in-any-all)
* [Aggregate Functions](#-aggregate-functions)
* [NULL Handling](#-null-handling)
* [Foreign Keys & Relationships](#-foreign-keys--relationships)
* [JOINs (INNER, LEFT, RIGHT, OUTER)](#-joins-inner-left-right-outer)
* [Triggers](#-triggers)
* [Indexes & Performance](#-indexes--performance)
* [Notes](#-notes)

---

## 📦 Database Management

Used to create, select, and delete databases.

```sql
SHOW DATABASES;
```

```sql
CREATE DATABASE IF NOT EXISTS nodejs;
```

```sql
USE nodejs;
```

```sql
DROP DATABASE IF EXISTS nodejs;
```

```sql
CREATE DATABASE IF NOT EXISTS nodejs
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

---

## 🧱 Tables

Define database structure.

### User

```sql
CREATE TABLE user (
  id INT PRIMARY KEY AUTO_INCREMENT,
  fullname VARCHAR(20) NOT NULL,
  username VARCHAR(20) NOT NULL,
  password VARCHAR(20) NOT NULL
);
```

---

### Profile (One-to-One)

```sql
CREATE TABLE profile (
  id INT PRIMARY KEY AUTO_INCREMENT,
  age INT,
  bio TEXT,
  city VARCHAR(30),
  image VARCHAR(150) DEFAULT 'default.png',
  bg_image VARCHAR(150),
  user_id INT NOT NULL UNIQUE,
  FOREIGN KEY (user_id) REFERENCES user(id)
);
```

---

### Product

```sql
CREATE TABLE product (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(100) NOT NULL,
  description TEXT,
  price DOUBLE NOT NULL,
  count INT DEFAULT 0
);
```

---

### Order (One-to-Many)

```sql
CREATE TABLE `order` (
  id INT PRIMARY KEY AUTO_INCREMENT,
  amount DOUBLE NOT NULL,
  status ENUM('pending','cancel','finished') DEFAULT 'pending',
  user_id INT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES user(id)
);
```

---

### Payment

```sql
CREATE TABLE payment (
  id INT PRIMARY KEY AUTO_INCREMENT,
  amount DOUBLE NOT NULL,
  invoice_number VARCHAR(20),
  status TINYINT DEFAULT 0,
  description VARCHAR(150),
  user_id INT NOT NULL,
  order_id INT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES user(id),
  FOREIGN KEY (order_id) REFERENCES `order`(id)
);
```

---

### Order Items (Many-to-Many)

```sql
CREATE TABLE order_items (
  id INT PRIMARY KEY AUTO_INCREMENT,
  product_id INT NOT NULL,
  order_id INT NOT NULL,
  FOREIGN KEY (product_id) REFERENCES product(id),
  FOREIGN KEY (order_id) REFERENCES `order`(id)
);
```

---

## 🧱 Special Data Types (ENUM, SET)

### ENUM

Stores **only one value** from a predefined list.

```sql
CREATE TABLE ticket (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(20),
  status ENUM('pending','open','close') DEFAULT 'pending'
);
```

---

### SET

Stores **multiple values** from a predefined list.

```sql
CREATE TABLE student_course (
  id INT PRIMARY KEY AUTO_INCREMENT,
  course_list SET(
    'NodeJS','NestJS','JS','NextJS','ReactJS','MongoDB'
  )
);
```

#### Insert data into SET

```sql
INSERT INTO student_course (course_list)
VALUES
('NodeJS,JS,MongoDB'),
('ReactJS,NextJS'),
('JS,NestJS,MongoDB');
```

---

## ✏️ Alter Table

Used to modify table structure.

### Add column

```sql
ALTER TABLE user
ADD details VARCHAR(50);
```

### Drop column

```sql
ALTER TABLE user
DROP COLUMN details;
```

### Modify column

```sql
ALTER TABLE product
MODIFY description VARCHAR(200);
```

---

## 🧩 CRUD Operations

### Insert

```sql
INSERT INTO user (fullname, username, password)
VALUES ('Ali', 'ali_dev', '1234');
```

### Select

```sql
SELECT *
FROM user;
```

### Update

```sql
UPDATE user
SET fullname = 'Ali Dev'
WHERE id = 1;
```

### Delete

```sql
DELETE
FROM user
WHERE id = 1;
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

```sql
SELECT *
FROM user
WHERE fullname LIKE '%a%';
```

```sql
SELECT *
FROM user
WHERE id BETWEEN 1 AND 10;
```

```sql
SELECT *
FROM user
WHERE id IN (1, 2, 3);
```

---

## 📊 Sorting Data (ORDER BY)

```sql
SELECT *
FROM user
ORDER BY fullname ASC;
```

```sql
SELECT *
FROM user
ORDER BY id DESC;
```

---

## 🧠 Subqueries (IN, ANY, ALL)

```sql
SELECT *
FROM user
WHERE id IN (
  SELECT user_id
  FROM `order`
);
```

```sql
SELECT *
FROM user
WHERE id < ANY (
  SELECT user_id
  FROM `order`
);
```

```sql
SELECT *
FROM user
WHERE id < ALL (
  SELECT user_id
  FROM `order`
);
```

---

## 🧮 Aggregate Functions

```sql
SELECT COUNT(id)
FROM user;
```

```sql
SELECT AVG(amount)
FROM `order`;
```

```sql
SELECT SUM(amount)
FROM payment;
```

---

## 🧩 NULL Handling

```sql
SELECT IFNULL(bio, 'No bio')
FROM profile;
```

```sql
SELECT COALESCE(city, 'Unknown')
FROM profile;
```

---

## 🔗 Foreign Keys & Relationships

* Prevent orphan records
* Enforce data integrity

### Relationship Types

| Type         | Example         |
| ------------ | --------------- |
| One-to-One   | user ↔ profile  |
| One-to-Many  | user → order    |
| Many-to-Many | order ↔ product |

---

## 🔀 JOINs (INNER, LEFT, RIGHT, OUTER)

### LEFT JOIN

Returns all rows from the left table.

```sql
SELECT
  user.id,
  user.fullname,
  `order`.amount,
  payment.invoice_number
FROM user
LEFT JOIN `order`
  ON user.id = `order`.user_id
LEFT JOIN payment
  ON `order`.id = payment.order_id;
```

---

### INNER JOIN

Returns only matching rows.

```sql
SELECT *
FROM user
INNER JOIN profile
  ON user.id = profile.user_id;
```

---

### Implicit JOIN (Not Recommended)

```sql
SELECT *
FROM user, profile
WHERE user.id = profile.user_id;
```

---

## 🔥 Triggers

Triggers execute automatically on table events.

```sql
CREATE TRIGGER after_update_user
AFTER UPDATE ON user
FOR EACH ROW
BEGIN
  IF NEW.id < OLD.id THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Invalid update';
  END IF;
END;
```

---

## 🚀 Indexes & Performance

```sql
CREATE INDEX idx_username
ON user(username);
```

```sql
DROP INDEX idx_username
ON user;
```

---

## ✅ Notes

* Prefer **explicit JOIN**
* `LEFT JOIN` is best for reporting
* `ENUM` → single value
* `SET` → multiple values
* `FOREIGN KEY` ensures integrity
* Indexes speed up SELECT but slow down writes


