# 📘 MySQL Tutorial (Complete & Clean)

## 📑 Table of Contents

* [Database Management](#-database-management)
* [Tables & Relationships](#-tables--relationships)
* [Special Data Types (ENUM, SET)](#-special-data-types-enum-set)
* [ALTER TABLE](#-alter-table)
* [Indexes](#-indexes)
* [CRUD Operations](#-crud-operations)
* [DELETE vs TRUNCATE](#️-delete-vs-truncate)
* [Filtering Data (WHERE)](#-filtering-data-where)
* [Sorting Data (ORDER BY)](#-sorting-data-order-by)
* [Subqueries (IN, ANY, ALL)](#-subqueries-in-any-all)
* [Aggregate Functions](#-aggregate-functions)
* [NULL Handling](#-null-handling)
* [JOINs (INNER, LEFT, RIGHT, FULL)](#-joins-inner-left-right-full)
* [Triggers](#-triggers)
* [Notes](#-notes)

---

## 📦 Database Management

**Manage databases: create, select, drop, charset/collation.**

```sql
-- Show all databases
SHOW DATABASES;

-- Create a database if it does not exist
CREATE DATABASE IF NOT EXISTS test;

-- Select a database
USE test;

-- Drop a database if it exists
DROP DATABASE IF EXISTS test;

-- Create database with utf8mb4 charset and collation
CREATE DATABASE IF NOT EXISTS test
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

**Explanation:**

* `CREATE DATABASE` creates a new DB
* `USE` selects DB to work with
* `DROP DATABASE` removes the DB
* `utf8mb4` supports all Unicode characters

---

## 🧱 Tables & Relationships

### User Table

```sql
CREATE TABLE user (
  id INT PRIMARY KEY AUTO_INCREMENT,
  fullname VARCHAR(50) NOT NULL,
  username VARCHAR(50) NOT NULL,
  password VARCHAR(50) NOT NULL
);
```

**Explanation:** Basic user table, `AUTO_INCREMENT` for unique IDs.

---

### Profile Table (One-to-One)

```sql
CREATE TABLE profile (
  id INT PRIMARY KEY AUTO_INCREMENT,
  age INT,
  bio TEXT,
  city VARCHAR(50),
  image VARCHAR(150) DEFAULT 'default.png',
  bg_image VARCHAR(150),
  user_id INT NOT NULL UNIQUE,
  FOREIGN KEY (user_id) REFERENCES user(id)
);
```

**Explanation:** Each user has exactly one profile. `FOREIGN KEY` ensures data integrity.

---

### Product Table

```sql
CREATE TABLE product (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(100) NOT NULL,
  description TEXT,
  price DOUBLE NOT NULL,
  count INT DEFAULT 0
);
```

**Explanation:** Stores product info. `count` has default 0.

---

### Order Table (One-to-Many)

```sql
CREATE TABLE `order` (
  id INT PRIMARY KEY AUTO_INCREMENT,
  amount DOUBLE NOT NULL,
  status ENUM('pending','cancel','finished') DEFAULT 'pending',
  user_id INT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES user(id)
);
```

**Explanation:** One user can have multiple orders.

---

### Payment Table

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

**Explanation:** Payment is linked to a user and an order.

---

### Order Items Table (Many-to-Many)

```sql
CREATE TABLE order_items (
  id INT PRIMARY KEY AUTO_INCREMENT,
  product_id INT NOT NULL,
  order_id INT NOT NULL,
  FOREIGN KEY (product_id) REFERENCES product(id),
  FOREIGN KEY (order_id) REFERENCES `order`(id)
);
```

**Explanation:** One order can include multiple products.

---

## 🧱 Special Data Types

### ENUM

```sql
CREATE TABLE ticket (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(50),
  status ENUM('pending','open','close') DEFAULT 'pending'
);
```

**Explanation:** `ENUM` allows only one value from a predefined list.

### SET

```sql
CREATE TABLE student_course (
  id INT PRIMARY KEY AUTO_INCREMENT,
  course_list SET('NodeJS','NestJS','JS','NextJS','ReactJS','MongoDB')
);
```

**Explanation:** `SET` allows storing multiple values from predefined list.

**Insert data into SET**

```sql
INSERT INTO student_course (course_list)
VALUES 
('NodeJS,JS,MongoDB'),
('ReactJS,NextJS'),
('JS,NestJS');
```

---

## ✏️ ALTER TABLE

### Add column

```sql
ALTER TABLE user 
ADD details VARCHAR(50);
```

*Adds a new column `details` to `user`.*

### Drop column

```sql
ALTER TABLE user 
DROP COLUMN details;
```

*Removes the `details` column.*

### Modify column

```sql
ALTER TABLE user 
MODIFY username VARCHAR(50);
```

*Changes the type/size of `username`.*

---

## 🏷️ Indexes

### Add index while creating table

```sql
CREATE TABLE contact (
  id INT PRIMARY KEY AUTO_INCREMENT,
  fullname VARCHAR(50),
  mobile VARCHAR(20),
  INDEX idx_fullname (fullname)
);
```

**Explanation:** Indexes improve SELECT performance.

### Add index to existing table

```sql
CREATE INDEX idx_mobile 
ON contact(mobile);
```

### Drop index

```sql
DROP INDEX idx_mobile
ON contact;
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
SET fullname='Ali Dev'
WHERE id=1;
```

### Delete

```sql
DELETE FROM user
WHERE id=1;
```

---

## 🗑️ DELETE vs TRUNCATE

```sql
DELETE FROM student_course WHERE id=1;
TRUNCATE TABLE student_course;
```

*DELETE can use WHERE; TRUNCATE removes all rows and resets AUTO_INCREMENT.*

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
WHERE id IN (1,2,3);
```

---

## 📊 Sorting Data (ORDER BY)

```sql
SELECT * 
FROM user 
ORDER BY fullname ASC;

SELECT * 
FROM user 
ORDER BY id DESC;
```

---

## 🧠 Subqueries (IN, ANY, ALL)

```sql
SELECT * 
FROM user
WHERE id IN (SELECT user_id FROM `order`);
```

```sql
SELECT * 
FROM user
WHERE id < ANY (SELECT user_id FROM `order`);
```

```sql
SELECT * 
FROM user
WHERE id < ALL (SELECT user_id FROM `order`);
```

---

## 🧮 Aggregate Functions

```sql
SELECT COUNT(id) FROM user;
SELECT AVG(amount) FROM `order`;
SELECT SUM(amount) FROM payment;
SELECT MIN(amount) FROM payment;
SELECT MAX(amount) FROM payment;
```

---

## 🧩 NULL Handling

```sql
SELECT IFNULL(bio,'No bio') FROM profile;
SELECT COALESCE(city,'Unknown') FROM profile;
```

---

## 🔀 JOINs

### INNER JOIN

```sql
SELECT *
FROM user
INNER JOIN profile
ON user.id = profile.user_id;
```

*Returns only matching rows.*

### LEFT JOIN

```sql
SELECT user.id, user.fullname, `order`.amount, payment.invoice_number
FROM user
LEFT JOIN `order` ON user.id = `order`.user_id
LEFT JOIN payment ON `order`.id = payment.order_id;
```

*Returns all users even if no order/payment exists.*

### RIGHT JOIN

```sql
SELECT user.id, `order`.id
FROM user
RIGHT JOIN `order` ON user.id = `order`.user_id;
```

*Returns all orders even if user does not exist.*

### FULL OUTER JOIN (via UNION)

```sql
SELECT user.id, `order`.id
FROM user
LEFT JOIN `order` ON user.id = `order`.user_id
UNION
SELECT user.id, `order`.id
FROM user
RIGHT JOIN `order` ON user.id = `order`.user_id;
```

*Returns all records from both tables.*

---

## 🔥 Triggers

### BEFORE INSERT

```sql
CREATE TRIGGER before_insert_user
BEFORE INSERT ON user
FOR EACH ROW
BEGIN
  IF NEW.username IS NULL THEN
    SET NEW.username = 'anonymous';
  END IF;
END;
```

### AFTER INSERT

```sql
CREATE TRIGGER after_insert_user
AFTER INSERT ON user
FOR EACH ROW
BEGIN
  INSERT INTO profile(user_id) VALUES (NEW.id);
END;
```

### AFTER UPDATE

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

### AFTER DELETE

```sql
CREATE TRIGGER after_delete_user
AFTER DELETE ON user
FOR EACH ROW
BEGIN
  DELETE FROM profile WHERE user_id = OLD.id;
END;
```

---

## ✅ Notes

* Always prefer **explicit JOINs**
* `LEFT JOIN` for reporting & unmatched records
* `ENUM` = single value, `SET` = multiple values
* `FOREIGN KEY` ensures integrity
* Triggers run **BEFORE/AFTER INSERT/UPDATE/DELETE**
* Indexes speed up SELECT but slow INSERT/UPDATE

---

## 👑 Author

Built with ❤️ by **Barbod Masoudi**

