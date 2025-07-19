# 📑 مستندات API

## 🌐 آدرس API
`https://netsheet.freehost.io/api/`

---

## 🧭 نحوه استفاده از API

- ارسال پارامترهای GET:
  - `token`: توکن کاربر برای احراز هویت
  - `mode`: متد مورد نظر

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

توکن در پایگاه داده بررسی شده و اطلاعات GitHub شما واکشی می‌شود.

---

## 🛠 پارامترهای GET عمومی

| پارامتر | نوع | اجباری | توضیح |
|--------|-----|--------|-------|
| token  | string | ✅ | توکن GitHub |
| mode   | string | ✅ | نام متد مورد نظر |

---

## 📄 CREATE_NEW_FILE

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| fileName | string | ✅ | نام فایل جدید |
| in       | string | ❌ | مسیر پوشه دلخواه |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "Missing POST parameter: fileName"
}
```

---

## ✏️ EDIT_DATA_FILE

| پارامتر     | نوع | اجباری | توضیح         |
|-------------|-----|--------|----------------|
| fileName    | string | ✅ | نام فایل |
| newContent  | string | ✅ | محتوای جدید |
| in          | string | ❌ | مسیر فایل |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## ❌ DELETE_FILE

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| fileName | string | ✅ | نام فایل |
| in       | string | ❌ | مسیر فایل |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔁 RENAME_FILE

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| oldName | string | ✅ | نام فعلی فایل |
| newName | string | ✅ | نام جدید فایل |
| in      | string | ❌ | مسیر فایل |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔍 GET_INFO_FILE

| پارامتر  | نوع | اجباری | توضیح |
|----------|-----|--------|--------|
| fileName | string | ✅ | نام فایل |
| in       | string | ❌ | مسیر فایل |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "File does not exist in the repository."
}
```

---

## 📂 GET_ALL_FILES

| پارامتر | نوع | اجباری | توضیح |
|--------|-----|--------|--------|
| in     | string | ❌ | مسیر پوشه |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "No files found in the repository."
}
```

---

## 📁 CREATE_NEW_FOLDER

| پارامتر    | نوع | اجباری | توضیح |
|------------|-----|--------|--------|
| folderName | string | ✅ | نام پوشه |
| in         | string | ❌ | مسیر والد |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "Missing POST parameter: folderName"
}
```

---

## ⬆️ UPLOAD_FILES

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| path    | string | ✅ | مسیر روی سرور |
| in      | string | ❌ | مسیر در GitHub |

### ✅ خروجی موفق
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

### ❌ خروجی ناموفق
```json
{
  "status": false,
  "message": "Path does not exist."
}
```
