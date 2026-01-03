حتماً 👍
این نسخه **انگلیسی، تمیز و استاندارد برای README گیتهاب** هست و مستقیم می‌تونی کپی کنی
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

## 🧱 Create Table

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

