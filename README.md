# 📑 مستندات API

## 🌐 آدرس API
`https://netsheet.freehost.io/api/`

---

## 🧭 نحوه استفاده از API

- ارسال پارامترهای GET:
  - `token`: توکن کاربر برای احراز هویت
  - `mode`: نام متد مورد نظر

- سایر پارامترها بسته به نوع متد به صورت POST ارسال می‌شوند.

---

## ✅ مثال CURL

```
curl -X POST "https://netsheet.freehost.io/api/?token=123456&mode=CREATE_NEW_FILE" \
  -d "fileName=test.txt" \
  -d "in=docs/"
```

---

## 🔒 احراز هویت

توکن در دیتابیس بررسی شده و در صورت معتبر بودن، اطلاعات GitHub کاربر واکشی می‌شود و عملیات روی مخزن انجام می‌گیرد.

---

## 🛠 پارامترهای GET عمومی

| پارامتر | نوع داده | اجباری | توضیح |
|--------|-----------|--------|--------|
| token  | string    | ✅     | توکن کاربر جهت دسترسی به GitHub |
| mode   | string    | ✅     | نام متد مورد نظر |

---

## 📄 CREATE_NEW_FILE

ساخت یک فایل جدید در مسیر مشخص‌شده در ریپوی GitHub با محتوای ابتدایی خالی.  
در صورت اعتبارسنجی موفق، فایل ایجاد و لینک مستقیم آن بازگردانده می‌شود.

### پارامترهای POST

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| fileName | string | ✅ | نام فایل جدید |
| in       | string | ❌ | مسیر پوشه دلخواه |

### خروجی موفق
```json
{
  "status": true,
  "message": "File successfully created.",
  "result": {
    "path": "docs/readme.txt",
    "url": "https://username.github.io/docs/readme.txt"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "Missing POST parameter: fileName"
}
```

---

## ✏️ EDIT_DATA_FILE

جایگزینی محتوای موجود یک فایل با متن جدید از طریق پارامتر `newContent`.  
در صورت وجود فایل، محتوای آن به‌روزرسانی و لینک نهایی بازگردانده می‌شود.

### پارامترهای POST

| پارامتر     | نوع     | اجباری | توضیح         |
|-------------|----------|--------|----------------|
| fileName    | string   | ✅     | نام فایل مورد ویرایش |
| newContent  | string   | ✅     | متن جدید فایل |
| in          | string   | ❌     | مسیر فایل     |

### خروجی موفق
```json
{
  "status": true,
  "message": "File successfully updated.",
  "result": {
    "path": "file.txt",
    "url": "https://username.github.io/file.txt"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## ❌ DELETE_FILE

حذف فایل مشخص‌شده از مخزن GitHub در صورت وجود و احراز توکن.  
اگر فایل قابل حذف باشد، عملیات انجام و لینک آن برگشت داده می‌شود.

### پارامترهای POST

| پارامتر  | نوع     | اجباری | توضیح     |
|----------|----------|--------|------------|
| fileName | string   | ✅     | نام فایل برای حذف |
| in       | string   | ❌     | مسیر فایل |

### خروجی موفق
```json
{
  "status": true,
  "message": "File successfully deleted.",
  "result": {
    "path": "file.txt",
    "url": "https://username.github.io/file.txt"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔁 RENAME_FILE

تغییر نام یک فایل با ساخت نسخه جدید و حذف نسخه قبلی.  
نام‌های جدید و قبلی بررسی شده و فایل منتقل می‌شود.

### پارامترهای POST

| پارامتر  | نوع     | اجباری | توضیح     |
|----------|----------|--------|------------|
| oldName  | string   | ✅     | نام فایل فعلی |
| newName  | string   | ✅     | نام فایل جدید |
| in       | string   | ❌     | مسیر فایل |

### خروجی موفق
```json
{
  "status": true,
  "message": "File successfully renamed.",
  "result": {
    "path": "newname.txt",
    "url": "https://username.github.io/newname.txt"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔍 GET_INFO_FILE

واکشی اطلاعات کامل فایل شامل مسیر، سایز، فرمت، محتوا و نام‌ها.  
در صورت موفقیت، جزئیات کامل فایل به صورت JSON برگشت داده می‌شود.

### پارامترهای POST

| پارامتر  | نوع     | اجباری | توضیح     |
|----------|----------|--------|------------|
| fileName | string   | ✅     | نام فایل جهت بررسی |
| in       | string   | ❌     | مسیر فایل |

### خروجی موفق
```json
{
  "status": true,
  "message": "File successfully retrieved.",
  "result": {
    "path": "folder/file.txt",
    "url": "https://username.github.io/folder/file.txt",
    "size": 102,
    "content": "hello world",
    "format": "txt",
    "name": "file.txt",
    "namenoext": "file",
    "folder": "folder"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "File does not exist in the repository."
}
```

---

## 📂 GET_ALL_FILES

دریافت لیست فایل‌ها داخل مسیر مشخص‌شده یا کل مخزن.  
خروجی شامل جزئیات فایل‌ها، محتوا و مسیرها خواهد بود.

### پارامترهای POST

| پارامتر | نوع   | اجباری | توضیح |
|--------|--------|--------|--------|
| in     | string | ❌     | مسیر پوشه دلخواه |

### خروجی موفق
```json
{
  "status": true,
  "message": "All files retrieved.",
  "result": [
    {
      "name": "file1.txt",
      "url": "https://username.github.io/docs/file1.txt",
      "size": 54,
      "content": "hello world",
      "format": "txt",
      "namenoext": "file1",
      "folder": "docs"
    }
  ]
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "No files found in the repository."
}
```

---

## 📁 CREATE_NEW_FOLDER

ساخت پوشه جدید در ریپوی کاربر در مسیر دلخواه.  
در صورت موفقیت، فایل ابتدایی README.md نیز ایجاد می‌شود.

### پارامترهای POST

| پارامتر     | نوع     | اجباری | توضیح     |
|-------------|----------|--------|------------|
| folderName  | string   | ✅     | نام پوشه برای ساخت |
| in          | string   | ❌     | مسیر والد |

### خروجی موفق
```json
{
  "status": true,
  "message": "Folder successfully created.",
  "result": {
    "path": "newfolder/README.md",
    "url": "https://username.github.io/newfolder/README.md"
  }
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "Missing POST parameter: folderName"
}
```

---

## ⬆️ UPLOAD_FILES

انتقال فایل یا پوشه از سرور میزبان به مسیر مشخص‌شده در GitHub.  
عملیات فقط در صورت وجود مسیر سرور و اعتبار موفق اجرا می‌شود.

### پارامترهای POST

| پارامتر | نوع     | اجباری | توضیح     |
|--------|----------|--------|------------|
| path   | string   | ✅     | مسیر کامل فایل یا پوشه روی سرور |
| in     | string   | ❌     | مسیر پوشه داخل GitHub برای آپلود فایل |

### خروجی موفق
```json
{
  "status": true,
  "message": "Upload(s) completed.",
  "result": [
    {
      "path": "img1.png",
      "url": "https://username.github.io/images/img1.png"
    },
    {
      "path": "img2.png",
      "url": "https://username.github.io/images/img2.png"
    }
  ]
}
```

### خروجی ناموفق
```json
{
  "status": false,
  "message": "Path does not exist."
}
```
``
