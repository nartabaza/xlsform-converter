# How to Upload Your Skill to GitHub | كيفية رفع مهارتك على GitHub

<div dir="rtl">

## الإعداد (مرة واحدة فقط)

إذا لم تكن قد قمت بإعداد Git من قبل، قم بتشغيل هذه الأوامر (استبدل بمعلوماتك):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## الخطوات

### 1. إنشاء مستودع على GitHub

1. اذهب إلى [GitHub.com](https://github.com) وسجل الدخول (أو أنشئ حساباً)
2. انقر على **+** في الأعلى اليمين ← **New repository**
3. املأ التفاصيل:
   - **Repository name**: `xlsform-converter`
   - **Description**: `Claude skill to convert text questionnaires to XLSForm format for KoboToolbox/ODK`
   - **Public**: اختر هذا الخيار (لجعله متاحاً للجميع)
   - **لا تضف** README أو .gitignore أو LICENSE (لديك هذه الملفات بالفعل)
4. انقر **Create repository**

### 2. ربط المجلد المحلي بـ GitHub

افتح Terminal/Command Prompt في مجلد المشروع وقم بتشغيل:

```bash
cd "C:\Users\Nart\OneDrive\1 Projects\Active. Linkedin Posts (Productivity & Performance)\xlsform-convertor"

# إعداد Git (إذا لم تكن قد قمت به)
git config user.name "Your Name"
git config user.email "your.email@example.com"

# إضافة جميع الملفات
git add .

# إنشاء أول commit
git commit -m "Initial commit: XLSForm Converter Claude Skill"

# إضافة رابط GitHub (استبدل YOUR_USERNAME باسم مستخدمك)
git remote add origin https://github.com/YOUR_USERNAME/xlsform-converter.git

# رفع الملفات إلى GitHub
git branch -M main
git push -u origin main
```

### 3. تحديث الروابط في README

بعد رفع الملفات:

1. افتح ملف `README.md`
2. ابحث عن `YOUR_USERNAME` واستبدلها باسم مستخدم GitHub الفعلي الخاص بك
3. احفظ الملف
4. قم برفع التحديث:
   ```bash
   git add README.md
   git commit -m "Update GitHub username in README"
   git push
   ```

### 4. تحديث ملف LICENSE

1. افتح ملف `LICENSE`
2. استبدل `[Your Name]` باسمك الحقيقي أو اسمك على GitHub
3. احفظ وقم بالرفع:
   ```bash
   git add LICENSE
   git commit -m "Update license with author name"
   git push
   ```

### 5. إنشاء إصدار (Release)

لتسهيل التثبيت على المستخدمين:

1. اذهب إلى صفحة مستودعك على GitHub
2. انقر **Releases** في القائمة الجانبية
3. انقر **Create a new release**
4. املأ التفاصيل:
   - **Tag version**: `v1.0.0`
   - **Release title**: `XLSForm Converter v1.0.0`
   - **Description**: صف الميزات الرئيسية
5. انقر **Publish release**

</div>

---

## Setup (One-Time Only)

If you haven't set up Git before, run these commands (replace with your info):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Steps

### 1. Create Repository on GitHub

1. Go to [GitHub.com](https://github.com) and sign in (or create an account)
2. Click **+** in top right ← **New repository**
3. Fill in the details:
   - **Repository name**: `xlsform-converter`
   - **Description**: `Claude skill to convert text questionnaires to XLSForm format for KoboToolbox/ODK`
   - **Public**: Choose this (to make it available to everyone)
   - **Don't add** README, .gitignore, or LICENSE (you already have these)
4. Click **Create repository**

### 2. Connect Your Local Folder to GitHub

Open Terminal/Command Prompt in the project folder and run:

```bash
cd "C:\Users\Nart\OneDrive\1 Projects\Active. Linkedin Posts (Productivity & Performance)\xlsform-convertor"

# Set up Git (if you haven't already)
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Add all files
git add .

# Create first commit
git commit -m "Initial commit: XLSForm Converter Claude Skill"

# Add GitHub remote (replace YOUR_USERNAME with your username)
git remote add origin https://github.com/YOUR_USERNAME/xlsform-converter.git

# Push files to GitHub
git branch -M main
git push -u origin main
```

### 3. Update Links in README

After uploading files:

1. Open `README.md`
2. Find `YOUR_USERNAME` and replace with your actual GitHub username
3. Save the file
4. Push the update:
   ```bash
   git add README.md
   git commit -m "Update GitHub username in README"
   git push
   ```

### 4. Update LICENSE File

1. Open `LICENSE`
2. Replace `[Your Name]` with your real name or GitHub name
3. Save and push:
   ```bash
   git add LICENSE
   git commit -m "Update license with author name"
   git push
   ```

### 5. Create a Release

To make it easy for users to install:

1. Go to your repository page on GitHub
2. Click **Releases** in the sidebar
3. Click **Create a new release**
4. Fill in the details:
   - **Tag version**: `v1.0.0`
   - **Release title**: `XLSForm Converter v1.0.0`
   - **Description**: Describe the main features
5. Click **Publish release**

---

## Quick Reference | مرجع سريع

<div dir="rtl">

### رفع تحديثات مستقبلية:

```bash
git add .
git commit -m "وصف التغييرات"
git push
```

### التحقق من الحالة:

```bash
git status
```

### عرض السجل:

```bash
git log --oneline
```

</div>

### Push future updates:

```bash
git add .
git commit -m "Description of changes"
git push
```

### Check status:

```bash
git status
```

### View history:

```bash
git log --oneline
```

---

## Troubleshooting | استكشاف الأخطاء

<div dir="rtl">

### خطأ: "Author identity unknown"

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### خطأ: "Permission denied"

استخدم Personal Access Token بدلاً من كلمة المرور:
1. GitHub Settings → Developer settings → Personal access tokens → Generate new token
2. اختر scope: `repo`
3. استخدم التوكن ككلمة مرور عند الـ push

### خطأ: "Repository not found"

تأكد من أن اسم المستخدم ورابط المستودع صحيحين في أمر `git remote add`

</div>

### Error: "Author identity unknown"

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### Error: "Permission denied"

Use a Personal Access Token instead of password:
1. GitHub Settings → Developer settings → Personal access tokens → Generate new token
2. Select scope: `repo`
3. Use the token as password when pushing

### Error: "Repository not found"

Make sure your username and repository URL are correct in the `git remote add` command

---

## Done! | تم!

<div dir="rtl">

بمجرد رفع مهارتك:
- شارك الرابط: `https://github.com/YOUR_USERNAME/xlsform-converter`
- يمكن للمستخدمين إضافتها مباشرة عبر Claude.ai
- يمكنك تحديثها في أي وقت بـ `git push`

</div>

Once your skill is uploaded:
- Share the link: `https://github.com/YOUR_USERNAME/xlsform-converter`
- Users can add it directly through Claude.ai
- You can update it anytime with `git push`
