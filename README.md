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

#### Drop table if exists

```sql
DROP TABLE IF EXISTS product;
```

#### Create table

```sql
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

#### Delete by range

```sql
DELETE FROM student
WHERE id > 2 AND id < 4;
```

#### Delete by specific ID

```sql
DELETE FROM student
WHERE id = 2;
```

---

## 🔎 Query with Conditions (WHERE)

### Filter by range

```sql
SELECT * FROM student
WHERE age >= 25 AND age <= 30;
```

### Filter by NOT condition

```sql
SELECT * FROM student
WHERE NOT city = 'Mashhad' AND NOT city = 'Tehran';
```

---

## 📊 Sorting Data (ORDER BY)

### Sort by age descending

```sql
SELECT * FROM student
ORDER BY age DESC;
```

### Sort by firstname ascending

```sql
SELECT * FROM student
ORDER BY firstname ASC;
```

---

## 🧮 Aggregate Functions

### Minimum age

```sql
SELECT MIN(age) FROM student;
```

### Maximum age

```sql
SELECT MAX(age) FROM student;
```

### Sum of ages

```sql
SELECT SUM(age) FROM student;
```

### Average age

```sql
SELECT AVG(age) FROM student;
```

### Count of students

```sql
SELECT COUNT(id) FROM student;
```

---

## ✅ Notes

* Use `snake_case` for table and column names (recommended standard)
* `AUTO_INCREMENT` values are not guaranteed to be sequential
* Always use `WHERE` in `UPDATE` and `DELETE` statements
* `utf8mb4` is recommended for full Unicode support
* `CHECK` constraints are supported in MySQL 8.0+
* Aggregate functions (`MIN`, `MAX`, `SUM`, `AVG`, `COUNT`) are used to calculate on columns
* `ORDER BY` is used to sort query results

