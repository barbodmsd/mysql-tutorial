# 📘 MySQL Tutorial

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

```sql
CREATE TABLE ticket (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(20),
  status ENUM('pending','open','close') DEFAULT 'pending'
);
```

### SET

```sql
CREATE TABLE student_course (
  id INT PRIMARY KEY AUTO_INCREMENT,
  course_list SET('NodeJS','NestJS','JS','NextJS','ReactJS','MongoDB')
);
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

```sql
INSERT INTO user (fullname,username,password)
VALUES ('Ali','ali_dev','1234');
```

```sql
SELECT * FROM user;
```

```sql
UPDATE user SET fullname='Ali Dev' WHERE id=1;
```

```sql
DELETE FROM user WHERE id=1;
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
SELECT * FROM user WHERE fullname LIKE '%a%';
```

```sql
SELECT * FROM user WHERE id BETWEEN 1 AND 10;
```

```sql
SELECT * FROM user WHERE id IN (1,2,3);
```

---

## 📊 Sorting Data (ORDER BY)

```sql
SELECT * FROM user ORDER BY fullname ASC;
SELECT * FROM user ORDER BY id DESC;
```

---

## 🧠 Subqueries (IN, ANY, ALL)

```sql
SELECT * FROM user
WHERE id IN (SELECT user_id FROM `order`);
```

```sql
SELECT * FROM user
WHERE id < ANY (SELECT user_id FROM `order`);
```

```sql
SELECT * FROM user
WHERE id < ALL (SELECT user_id FROM `order`);
```

---

## 🧮 Aggregate Functions

```sql
SELECT COUNT(id) FROM user;
SELECT AVG(amount) FROM `order`;
SELECT SUM(amount) FROM payment;
```

---

## 🧩 NULL Handling

```sql
SELECT IFNULL(bio,'No bio') FROM profile;
SELECT COALESCE(city,'Unknown') FROM profile;
```

---

## 🔗 Foreign Keys & Relationships

### What is FOREIGN KEY?

```text
FOREIGN KEY ensures referential integrity
```

* Prevents orphan records
* Enforces relationships between tables

### Common Naming Convention

```text
user_id → references user(id)
order_id → references order(id)
```

### Relationship Types

| Type         | Example         |
| ------------ | --------------- |
| One-to-One   | user ↔ profile  |
| One-to-Many  | user → order    |
| Many-to-Many | order ↔ product |

---

## 🔀 JOINs (INNER, LEFT, RIGHT, OUTER)

### LEFT JOIN (recommended)

```sql
SELECT
  user.id,
  user.fullname,
  `order`.id,
  `order`.amount,
  payment.invoice_number
FROM user
LEFT JOIN `order`
  ON user.id = `order`.user_id
LEFT JOIN payment
  ON `order`.id = payment.order_id;
```

✔ returns all users
✔ even if order or payment does not exist

---

### INNER JOIN

```sql
SELECT *
FROM user
INNER JOIN profile
ON user.id = profile.user_id;
```

✔ only matched records

---

### Implicit JOIN (OLD – not recommended)

```sql
SELECT *
FROM user, profile
WHERE user.id = profile.user_id;
```

❌ harder to read
❌ error-prone with multiple tables

---

## 🔥 Triggers

```sql
CREATE TRIGGER after_update_user
AFTER UPDATE ON user
FOR EACH ROW
BEGIN
  IF NEW.id < OLD.id THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT='Invalid update';
  END IF;
END;
```

---

## 🚀 Indexes & Performance

```sql
CREATE INDEX idx_username ON user(username);
DROP INDEX idx_username ON user;
```

---

## ✅ Notes

* Always prefer **explicit JOIN**
* `LEFT JOIN` is best for reports
* Foreign keys protect data integrity
* `ENUM` = single value
* `SET` = multiple values
* `TRIGGER` runs automatically
* Indexes speed up SELECT, slow down INSERT/UPDATE

