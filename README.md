اوکی، اینو طوری برات می‌نویسم که **تمیز، خوانا و استانداردِ README گیتهاب** باشه و مستقیم بتونی کپی کنی 👌
(با تیتر، کدبلاک و توضیح کوتاه)

---

````md
# MySQL Tutorial

## 📦 Create Database

نمایش دیتابیس‌ها:
```sql
SHOW DATABASES;
````

ساخت دیتابیس (در صورت عدم وجود):

```sql
CREATE DATABASE IF NOT EXISTS nodejs;
```

انتخاب دیتابیس:

```sql
USE nodejs;
```

حذف دیتابیس (در صورت وجود):

```sql
DROP DATABASE IF EXISTS nodejs;
```

ساخت دیتابیس با charset و collation مشخص:

```sql
CREATE DATABASE IF NOT EXISTS nodejs
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

---

## 🧱 Create Table

حذف جدول (در صورت وجود):

```sql
DROP TABLE IF EXISTS product;
```

ساخت جدول `product`:

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

اضافه کردن ستون جدید:

```sql
ALTER TABLE product
ADD details VARCHAR(85);
```

حذف ستون:

```sql
ALTER TABLE product
DROP COLUMN details;
```

تغییر نوع ستون:

```sql
ALTER TABLE product
MODIFY COLUMN description VARCHAR(200);
```

```

---

### نکات کوچیک ولی حرفه‌ای ✨
- اسم دیتابیس و جدول بدون `' '` نوشته می‌شه
- `utf8mb4` بهترین انتخاب برای پشتیبانی کامل یونیکد (ایموجی 😄)
- این ساختار برای README خیلی تمیز و قابل فهمه

اگه بخوای:
- نسخه **انگلیسی رسمی‌تر**
- یا اضافه کردن **Index / Foreign Key / Example Insert**
- یا مخصوص پروژه **Node.js + MySQL**

بگو تا همونو برات آماده کنم 🔥
```
