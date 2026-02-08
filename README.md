# XLSForm Converter - Claude Skill | محول XLSForm - مهارة كلود

<div dir="rtl">

## ما الذي تفعله هذه المهارة

هذه المهارة تساعدك على تحويل الاستبيانات النصية (مستندات Word، ملفات نصية، إلخ) إلى ملفات Excel بصيغة XLSForm التي تعمل مع:
- **KoboToolbox**
- **ODK Collect**
- **SurveyCTO**
- منصات أخرى قائمة على ODK

</div>

---

## What This Skill Does

This skill helps you transform plain text questionnaires (Word documents, text files, etc.) into properly formatted XLSForm Excel files that work with:
- **KoboToolbox**
- **ODK Collect**
- **SurveyCTO**
- Other ODK-based platforms

---

<div dir="rtl">

### الميزات الرئيسية

- **الكشف التلقائي عن نوع السؤال** (نص، رقمي، اختيار واحد/متعدد، تاريخ، GPS، إلخ)
- **دعم النماذج ثنائية اللغة** (خاصة الإنجليزية-العربية)
- **قواعد التحقق الذكية** (نطاقات العمر، أرقام الهاتف، البريد الإلكتروني، إلخ)
- **المنطق الشرطي** (الأسئلة المشروطة)
- **ميزات متقدمة**: أسئلة المصفوفة، القوائم المتتالية، المجموعات المتكررة
- **استخراج محتوى ملفات DOCX** مع معالجة صحيحة للجداول والفقرات

</div>

### Key Features

- **Automatic question type detection** (text, numeric, select one/multiple, date, GPS, etc.)
- **Bilingual form support** (especially English-Arabic)
- **Smart validation rules** (age ranges, phone numbers, email, etc.)
- **Skip logic** (conditional questions)
- **Advanced features**: Matrix questions, cascading selects, repeat groups
- **DOCX file extraction** with proper handling of tables and paragraphs

---

<div dir="rtl">

## التثبيت

### الطريقة 1: عبر موقع Claude.ai (الأسهل - موصى بها)

1. اذهب إلى [صفحة الإصدارات](https://github.com/nartabaza/xlsform-converter/releases)
2. قم بتحميل ملف **xlsform-converter-skill.zip**
3. افتح [Claude.ai](https://claude.ai)
4. اذهب إلى **الإعدادات** (Settings) > **القدرات** (Capabilities) > **المهارات** (Skills)
5. انقر على **+ إضافة** (+ Add)
6. اختر **Upload a skill**
7. قم برفع ملف ZIP الذي قمت بتحميله
8. ستكون المهارة جاهزة للاستخدام فوراً!

### الطريقة 2: التثبيت المحلي (لمستخدمي Claude Code CLI)

#### لمستخدمي Windows:
1. افتح File Explorer
2. انتقل إلى: `%USERPROFILE%\.claude-code\skills\user\`
3. أنشئ مجلداً باسم `xlsform-converter`
4. قم بتحميل الملفات من [GitHub](https://github.com/nartabaza/xlsform-converter) وانسخها إلى هذا المجلد

#### لمستخدمي Mac/Linux:
```bash
mkdir -p ~/.claude-code/skills/user/xlsform-converter
cd ~/.claude-code/skills/user/xlsform-converter
# قم بنسخ الملفات من GitHub إلى هنا
```

يجب أن يبدو الهيكل النهائي كالتالي:
```
~/.claude-code/skills/user/xlsform-converter/
├── SKILL.md
├── scripts/
│   └── docx_extractor.py
└── README.md
```

ثم أعد تشغيل Claude Code لتحميل المهارة.

</div>

---

## Installation

### Method 1: Via Claude.ai (Easiest - Recommended)

1. Go to the [Releases page](https://github.com/nartabaza/xlsform-converter/releases)
2. Download **xlsform-converter-skill.zip**
3. Open [Claude.ai](https://claude.ai)
4. Go to **Settings** > **Capabilities** > **Skills**
5. Click **+ Add**
6. Select **Upload a skill**
7. Upload the ZIP file you downloaded
8. The skill is ready to use immediately!

### Method 2: Local Installation (For Claude Code CLI Users)

#### For Windows:
1. Open File Explorer
2. Navigate to: `%USERPROFILE%\.claude-code\skills\user\`
3. Create a folder named `xlsform-converter`
4. Download files from [GitHub](https://github.com/nartabaza/xlsform-converter) and copy them here

#### For Mac/Linux:
```bash
mkdir -p ~/.claude-code/skills/user/xlsform-converter
cd ~/.claude-code/skills/user/xlsform-converter
# Copy files from GitHub to here
```

The final structure should look like:
```
~/.claude-code/skills/user/xlsform-converter/
├── SKILL.md
├── scripts/
│   └── docx_extractor.py
└── README.md
```

Then restart Claude Code to load the skill.

---

<div dir="rtl">

## الاستخدام

### الاستخدام الأساسي

ببساطة، أخبر Claude بتحويل استبيانك:

```
"حول هذا الاستبيان إلى صيغة XLSForm لـ KoboToolbox"
```

ثم الصق نص استبيانك أو قدم مسار ملف DOCX.

### أمثلة على سير العمل

#### 1. تحويل مستند Word
```
"لدي استبيان في survey.docx - من فضلك حوله إلى XLSForm"
```

#### 2. تحويل استبيان نصي
```
"حول هذا إلى XLSForm:

1. ما اسمك؟
2. كم عمرك؟
3. ما جنسك؟
   - ذكر
   - أنثى
   - آخر
```

#### 3. إنشاء نموذج ثنائي اللغة (الإنجليزية-العربية)
```
"حول هذا الاستبيان إلى XLSForm مع ترجمة إنجليزية وعربية"
```

#### 4. الميزات المتقدمة
تكتشف المهارة تلقائياً وتتعامل مع:
- أسئلة المصفوفة (الشبكات)
- المنطق الشرطي (الأسئلة المشروطة)
- قواعد التحقق
- القوائم المنسدلة المتتالية
- الأقسام المتكررة

</div>

---

## Usage

### Basic Usage

Simply tell Claude to convert your questionnaire:

```
"Convert this questionnaire to XLSForm format for KoboToolbox"
```

Then paste your questionnaire text or provide a DOCX file path.

### Example Workflows

#### 1. Convert a Word Document
```
"I have a survey in survey.docx - please convert it to XLSForm"
```

#### 2. Convert Text Questionnaire
```
"Convert this to XLSForm:

1. What is your name?
2. How old are you?
3. What is your gender?
   - Male
   - Female
   - Other
```

#### 3. Create Bilingual Form (English-Arabic)
```
"Convert this questionnaire to XLSForm with English and Arabic translations"
```

#### 4. Advanced Features
The skill automatically detects and handles:
- Matrix rating questions (grids)
- Skip logic (conditional questions)
- Validation rules
- Cascading dropdowns
- Repeat sections

---

<div dir="rtl">

## المتطلبات

- **Claude Code CLI** مثبت
- **Python 3.7+** (إذا كنت تستخدم استخراج DOCX)
- **حزمة python-docx** (لملفات DOCX): `pip install python-docx`

## كيف يعمل

1. **يحلل** بنية استبيانك
2. **يكتشف** أنواع الأسئلة تلقائياً
3. **ينشئ** بنية XLSForm الصحيحة (صفحات survey، choices، settings)
4. **يطبق** التحقق الذكي والمنطق الشرطي
5. **يولد** ملف Excel جاهز للاستخدام

</div>

---

## Requirements

- **Claude Code CLI** installed
- **Python 3.7+** (if using DOCX extraction)
- **python-docx** package (for DOCX files): `pip install python-docx`

## How It Works

1. **Analyzes** your questionnaire structure
2. **Detects** question types automatically
3. **Creates** proper XLSForm structure (survey, choices, settings sheets)
4. **Applies** smart validation and skip logic
5. **Generates** a ready-to-use Excel file

---

<div dir="rtl">

## التوثيق

- [توثيق KoboToolbox](https://support.kobotoolbox.org/)
- [XLSForm.org](https://xlsform.org/)
- [توثيق ODK](https://docs.getodk.org/)

## استكشاف الأخطاء وإصلاحها

### المهارة لا تعمل
- تأكد من أن الملفات في المجلد الصحيح: `~/.claude-code/skills/user/xlsform-converter/`
- أعد تشغيل Claude Code بالكامل
- تحقق من وجود ملف `SKILL.md`

### مشاكل استخراج DOCX
- قم بتثبيت python-docx: `pip install python-docx`
- تحقق من تثبيت Python: `python --version`

### النموذج لا يعمل في KoboToolbox
- تحقق من صحة النموذج على [XLSForm Online](https://getodk.org/xlsform/)
- راجع قسم الافتراضات الذي يقدمه Claude بعد التوليد
- راجع المنطق الشرطي وقواعد التحقق

</div>

---

## Documentation

- [KoboToolbox Documentation](https://support.kobotoolbox.org/)
- [XLSForm.org](https://xlsform.org/)
- [ODK Documentation](https://docs.getodk.org/)

## Troubleshooting

### Skill Not Loading
- Make sure files are in the correct directory: `~/.claude-code/skills/user/xlsform-converter/`
- Restart Claude Code completely
- Check that `SKILL.md` is present

### DOCX Extraction Issues
- Install python-docx: `pip install python-docx`
- Verify Python is installed: `python --version`

### Form Not Working in KoboToolbox
- Validate your form at [XLSForm Online](https://getodk.org/xlsform/)
- Check the assumptions section Claude provides after generation
- Review skip logic and validation rules

---

<div dir="rtl">

## المساهمة

المساهمات مرحب بها! لا تتردد في إرسال المشاكل أو طلبات السحب.

## الترخيص

هذا المشروع مرخص بموجب ترخيص MIT - راجع ملف [LICENSE](LICENSE) للتفاصيل.

## الدعم

إذا واجهت أي مشاكل أو لديك أسئلة:
1. تحقق من صفحة [المشاكل](https://github.com/nartabaza/xlsform-converter/issues)
2. أنشئ مشكلة جديدة مع تفاصيل مشكلتك
3. قم بتضمين نص الاستبيان النموذجي (إن أمكن)

## الفضل

تم إنشاؤه للاستخدام مع Claude Code لتبسيط إنشاء XLSForm لمشاريع جمع البيانات الإنسانية والبحثية.

</div>

---

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

If you encounter any issues or have questions:
1. Check the [Issues](https://github.com/nartabaza/xlsform-converter/issues) page
2. Create a new issue with details about your problem
3. Include sample questionnaire text (if possible)

## Credits

Created for use with Claude Code to streamline XLSForm creation for humanitarian and research data collection projects.

---

**Note** | **ملاحظة**: Replace `YOUR_USERNAME` with your actual GitHub username after creating the repository. | استبدل `YOUR_USERNAME` باسم مستخدم GitHub الفعلي الخاص بك بعد إنشاء المستودع.
