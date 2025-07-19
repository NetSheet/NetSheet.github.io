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

ایجاد یک فایل جدید در ریپوی GitHub با نام و مسیر مشخص‌شده.  
در صورت معتبر بودن توکن، فایل جدید ایجاد و لینک آن بازگردانده می‌شود.

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| fileName | string | ✅ | نام فایل جدید |
| in       | string | ❌ | مسیر پوشه دلخواه |

**✅ خروجی موفق**
```json
{ "status": true, "message": "File successfully created.", "result": { "path": "docs/readme.txt", "url": "https://username.github.io/docs/readme.txt" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "Missing POST parameter: fileName" }
```

---

## ✏️ EDIT_DATA_FILE

ویرایش فایل موجود در ریپو با محتوای جدید از طریق POST.  
در صورت وجود فایل، تغییرات اعمال و لینک جدید بازگردانده می‌شود.

| پارامتر | نوع | اجباری | توضیح |
|-------------|-----|--------|--------|
| fileName    | string | ✅ | نام فایل |
| newContent  | string | ✅ | محتوای جدید |
| in          | string | ❌ | مسیر فایل |

**✅ خروجی موفق**
```json
{ "status": true, "message": "File successfully updated.", "result": { "path": "file.txt", "url": "https://username.github.io/file.txt" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "File not found or SHA missing." }
```

---

## ❌ DELETE_FILE

حذف فایل مشخص‌شده از مخزن GitHub با اعتبارسنجی توکن.  
در صورت وجود فایل و مجوز، عملیات حذف انجام می‌شود.

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| fileName | string | ✅ | نام فایل |
| in       | string | ❌ | مسیر فایل |

**✅ خروجی موفق**
```json
{ "status": true, "message": "File successfully deleted.", "result": { "path": "file.txt", "url": "https://username.github.io/file.txt" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "File not found or SHA missing." }
```

---

## 🔁 RENAME_FILE

تغییر نام فایل با کپی فایل جدید و حذف نسخه قبلی.  
نام‌های جدید و قدیم بررسی شده و فایل جایگزین می‌شود.

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| oldName | string | ✅ | نام فعلی فایل |
| newName | string | ✅ | نام جدید فایل |
| in      | string | ❌ | مسیر فایل |

**✅ خروجی موفق**
```json
{ "status": true, "message": "File successfully renamed.", "result": { "path": "newname.txt", "url": "https://username.github.io/newname.txt" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "File not found or SHA missing." }
```

---

## 🔍 GET_INFO_FILE

دریافت اطلاعات کامل یک فایل شامل سایز، محتوا و فرمت.  
در صورت وجود فایل، داده‌ها به‌صورت JSON برگردانده می‌شوند.

| پارامتر | نوع | اجباری | توضیح |
|----------|-----|--------|--------|
| fileName | string | ✅ | نام فایل |
| in       | string | ❌ | مسیر فایل |

**✅ خروجی موفق**
```json
{ "status": true, "message": "File successfully retrieved.", "result": { "path": "folder/file.txt", "url": "...", "size": 102, "content": "hello world", "format": "txt", "name": "file.txt", "namenoext": "file", "folder": "folder" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "File does not exist in the repository." }
```

---

## 📂 GET_ALL_FILES

دریافت لیست تمام فایل‌ها از پوشه یا کل مخزن.  
خروجی شامل اطلاعات هر فایل به همراه مسیر و محتوا است.

| پارامتر | نوع | اجباری | توضیح |
|--------|-----|--------|--------|
| in     | string | ❌ | مسیر پوشه دلخواه |

**✅ خروجی موفق**
```json
{ "status": true, "message": "All files retrieved.", "result": [ { "name": "file1.txt", "url": "...", "size": 54, "content": "hello world", "format": "txt", "namenoext": "file1", "folder": "docs" } ] }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "No files found in the repository." }
```

---

## 📁 CREATE_NEW_FOLDER

ساخت پوشه جدید در مخزن GitHub در مسیر مشخص‌شده.  
در صورت موفقیت، فایل ابتدایی README.md داخل آن قرار می‌گیرد.

| پارامتر | نوع | اجباری | توضیح |
|-------------|-----|--------|--------|
| folderName  | string | ✅ | نام پوشه |
| in          | string | ❌ | مسیر والد |

**✅ خروجی موفق**
```json
{ "status": true, "message": "Folder successfully created.", "result": { "path": "newfolder/README.md", "url": "https://username.github.io/newfolder/README.md" } }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "Missing POST parameter: folderName" }
```

---

## ⬆️ UPLOAD_FILES

آپلود فایل یا پوشه از سرور فعلی به مسیر مشخص در ریپوی GitHub.  
در صورت موجود بودن فایل روی سرور، عملیات کامل انجام می‌شود.

| پارامتر | نوع | اجباری | توضیح |
|---------|-----|--------|--------|
| path    | string | ✅ | مسیر فایل روی هاست |
| in      | string | ❌ | مسیر آپلود در GitHub |

**✅ خروجی موفق**
```json
{ "status": true, "message": "Upload(s) completed.", "result": [ { "path": "img1.png", "url": "..." }, { "path": "img2.png", "url": "..." } ] }
```

**❌ خروجی ناموفق**
```json
{ "status": false, "message": "Path does not exist." }
```
