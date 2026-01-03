# MySQL Tutorial

## 📦 Create Database

Show all databases:
```sql
SHOW DATABASES;
````

Create a database (if it does not exist):

```sql
CREATE DATABASE IF NOT EXISTS nodejs;
```

Select the database:

```sql
USE nodejs;
```

Drop a database (if it exists):

```sql
DROP DATABASE IF EXISTS nodejs;
```

Create a database with a specific charset and collation:

```sql
CREATE DATABASE IF NOT EXISTS nodejs
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

---

## 🧱 Create Tables

### Product Table

Drop table if it exists:

```sql
DROP TABLE IF EXISTS product;
```

Create `product` table:

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

Drop table if it exists:

```sql
DROP TABLE IF EXISTS city;
```

Create `city` table:

```sql
CREATE TABLE IF NOT EXISTS city (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) DEFAULT 'city_name'
);
```

---

### Student Table

Drop table if it exists:

```sql
DROP TABLE IF EXISTS student;
```

Create `student` table:

```sql
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

Add a new column:

```sql
ALTER TABLE product
ADD details VARCHAR(85);
```

Drop a column:

```sql
ALTER TABLE product
DROP COLUMN details;
```

Modify a column type:

```sql
ALTER TABLE product
MODIFY COLUMN description VARCHAR(200);
```

---

## 🧩 Data Manipulation (CRUD)

### ➕ Insert Data

Insert records into `student` table:

```sql
INSERT INTO student (firstname, lastname, national_code, bio)
VALUES
  ('Barbod', 'Masoudi', '0123456789', 'MERN Developer'),
  ('Elyas', 'Moludi', '12345678', 'Junior Developer');
```

---

### 🔍 Select Data

View all students:

```sql
SELECT * FROM student;
```

---

### ✏️ Update Data

Update student age:

```sql
UPDATE student
SET age = 19
WHERE lastname = 'Moludi';
```

---

### ❌ Delete Data

Delete by range condition:

```sql
DELETE FROM student
WHERE id > 2 AND id < 4;
```

Delete by specific id:

```sql
DELETE FROM student
WHERE id = 2;
```

---

## ✅ Notes

* Use `snake_case` for table and column names (recommended standard)
* `AUTO_INCREMENT` values are not guaranteed to be sequential
* Always use `WHERE` with `UPDATE` and `DELETE`
* `utf8mb4` is recommended for full Unicode support
* `CHECK` constraints are supported in MySQL 8.0+



