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

# 📑 API Documentation

## 🌐 API Endpoint
`https://netsheet.freehost.io/api/`

---

## 🧭 How to Use

- Send the following via GET:
  - `token`: user token for authentication
  - `mode`: desired method name

- Send additional parameters via POST depending on the chosen method.

---

## ✅ Example CURL

```
curl -X POST "https://netsheet.freehost.io/api/?token=123456&mode=CREATE_NEW_FILE" \
  -d "fileName=test.txt" \
  -d "in=docs/"
```

---

## 🔒 Authentication

The token is validated in the database. If valid, GitHub repository details are retrieved and actions are performed on your repo.

---

## 🛠 Common GET Parameters

| Parameter | Type   | Required | Description                    |
|-----------|--------|----------|--------------------------------|
| token     | string | ✅       | GitHub access token            |
| mode      | string | ✅       | Target API method              |

---

## 📄 CREATE_NEW_FILE

Creates a new file in your GitHub repository at a specified location.  
If authentication succeeds, the file is created and its URL is returned.

### POST Parameters

| Parameter | Type   | Required | Description                   |
|-----------|--------|----------|-------------------------------|
| fileName  | string | ✅       | Name of the new file          |
| in        | string | ❌       | Target folder path (optional) |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "Missing POST parameter: fileName"
}
```

---

## ✏️ EDIT_DATA_FILE

Replaces an existing file's content with new data via the `newContent` parameter.  
If the file exists, it is updated and the new URL is returned.

### POST Parameters

| Parameter   | Type   | Required | Description           |
|-------------|--------|----------|-----------------------|
| fileName    | string | ✅       | Name of the file      |
| newContent  | string | ✅       | New file content      |
| in          | string | ❌       | Path to the file      |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## ❌ DELETE_FILE

Deletes a specified file from the GitHub repository if it exists and the token is valid.  
Returns success or error message based on deletion outcome.

### POST Parameters

| Parameter | Type   | Required | Description           |
|-----------|--------|----------|-----------------------|
| fileName  | string | ✅       | Name of the file      |
| in        | string | ❌       | Path to the file      |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔁 RENAME_FILE

Renames an existing file by copying it to a new name and deleting the old one.  
Verifies both old and new filenames, then moves the content.

### POST Parameters

| Parameter | Type   | Required | Description           |
|-----------|--------|----------|-----------------------|
| oldName   | string | ✅       | Current file name     |
| newName   | string | ✅       | Desired new name      |
| in        | string | ❌       | File path             |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "File not found or SHA missing."
}
```

---

## 🔍 GET_INFO_FILE

Fetches full metadata about a file including size, format, content, and name.  
Returns complete details if the file is found.

### POST Parameters

| Parameter | Type   | Required | Description           |
|-----------|--------|----------|-----------------------|
| fileName  | string | ✅       | Name of the file      |
| in        | string | ❌       | Path to the file      |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "File does not exist in the repository."
}
```

---

## 📂 GET_ALL_FILES

Retrieves a list of all files in a given folder or the entire repo.  
Output includes file details, content, size, and URLs.

### POST Parameters

| Parameter | Type   | Required | Description           |
|-----------|--------|----------|-----------------------|
| in        | string | ❌       | Folder path (optional) |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "No files found in the repository."
}
```

---

## 📁 CREATE_NEW_FOLDER

Creates a new folder in the GitHub repository at the specified path.  
Automatically adds a `README.md` file inside the new folder.

### POST Parameters

| Parameter   | Type   | Required | Description           |
|-------------|--------|----------|-----------------------|
| folderName  | string | ✅       | Name of new folder    |
| in          | string | ❌       | Parent path (optional) |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "Missing POST parameter: folderName"
}
```

---

## ⬆️ UPLOAD_FILES

Uploads a file or entire folder from your host to GitHub.  
Requires valid server path; uploads to target GitHub folder.

### POST Parameters

| Parameter | Type   | Required | Description                   |
|-----------|--------|----------|-------------------------------|
| path      | string | ✅       | Path to file or folder on host |
| in        | string | ❌       | Target folder in GitHub       |

### Successful Response
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

### Failed Response
```json
{
  "status": false,
  "message": "Path does not exist."
}
```
