---
name: xlsform-converter
description: "Use this skill when the user wants to convert text questionnaires or survey forms into XLSForm format for KoboToolbox, ODK, or other ODK-based data collection platforms. Triggers include: requests to 'convert to XLSForm', 'create a form for Kobo/ODK', 'transform questionnaire to XLS', or when the user provides a text-based survey that needs to be structured for mobile data collection. Also use when the user wants to create bilingual forms (especially English-Arabic), needs proper question type detection, validation rules, skip logic, or advanced features like cascading selects and repeat groups. The deliverable must be a properly formatted .xlsx file with survey, choices, and settings sheets following XLSForm specifications."
license: MIT License
---

# XLSForm Converter Skill

## Core Capabilities

### 1. Question Type Auto-Detection
Claude should automatically identify question types from the context and structure of questions:

**Question Types:**
- **text**: Open-ended questions, names, descriptions, comments
- **integer**: Numeric questions expecting whole numbers (age, counts)
- **decimal**: Numeric questions allowing decimals (amounts, prices, rates, measurements)
- **select_one**: Single choice from predefined options
- **select_multiple**: Multiple choices allowed from predefined options
- **date**: Date entry
- **time**: Time entry
- **datetime**: Combined date and time
- **geopoint**: Location/GPS coordinate capture
- **image/audio/video/file**: Media capture or file upload
- **note**: Display-only informational text (instructions, section headers)
- **calculate**: Computed fields (totals, derived values)
- **acknowledge**: Confirmation checkbox (consent, agreements)
- **begin_group / end_group**: Grouping questions together (always use `appearance = 'field-list'`)
- **begin_repeat / end_repeat**: Repeating sections (household members, items) (always use `appearance = 'field-list'`)

**Special Presentation Patterns:**

**Matrix Questions** - Multiple rating questions displayed as a grid:
- **Trigger**: Introductory text ("Rate the following...", "Evaluate each...") followed by multiple questions with same response scale
- **Structure**: First row is the intro question (same type as subsequent questions), subsequent rows are actual rating questions
- **Type**: First row uses the same question type as following questions (usually `select_one`)
- **Appearance**: `label` (first row) + `list-nolabel` (all subsequent rows)
- **Important**: The intro question is NOT a `note` type - it must be the same type (e.g., `select_one rating`) with `appearance='label'`

**Likert Questions** - Standalone rating scale questions:
- **Trigger**: Single rating question without being part of a matrix
- **Appearance**: `likert`

**Decision Priority**: If questions form a matrix pattern (intro + grid), use matrix appearance. Otherwise, use likert for individual rating questions.

---

## CRITICAL: Content Preservation Principle

**Do NOT modify question or choice phrasings unless explicitly requested by the user.**

- Keep all question text exactly as written in the original questionnaire
- Keep all choice labels exactly as written
- Do NOT rephrase for clarity, grammar, or style
- Do NOT translate without explicit permission
- Do NOT add or remove words

**Only modify phrasings when:**
- User explicitly requests it ("please rephrase...", "fix the grammar...", "make it clearer...")
- Technical requirements demand it (e.g., field names, choice values)

**If you must adjust any phrasing** (with permission or due to technical constraints):
- Document each change in the assumptions section
- List the original text and the modified text
- Explain why the change was necessary

**Example of what NOT to do:**
- Original: "How old are you?"
- ❌ DON'T change to: "What is your age?"

**Example of acceptable technical changes:**
- Choice text: "Yes, I agree" → Choice value: `yes_agree` (internal identifier)
- These technical conversions don't need to be documented unless the text itself changes

### 2. XLSForm Structure
Generate an Excel file with three primary sheets:

#### **survey sheet** (required columns):
- `type`: Question type (text, integer, select_one, etc.)
- `name`: Unique identifier (lowercase, no spaces, meaningful)
- `label`: Human-readable question text (primary language)
- `label::English (en)`: Additional language column(s) for bilingual forms (format: `label::Language Name (code)`)
- `hint`: Optional guidance text
- `hint::English (en)`: Translated hints
- `required`: yes/no (default: no unless context suggests otherwise)
- `constraint`: Validation expression
- `constraint_message`: Error message for validation failures
- `constraint_message::English (en)`: Translated constraint messages
- `relevant`: Skip logic expression
- `calculation`: For calculated fields
- `choice_filter`: For cascading selects
- `appearance`: UI styling hints
- `default`: Default value

**CRITICAL: Every form must include start and end metadata fields at the beginning:**

```
type      | name  | label | calculation
start     | start |       |
end       | end   |       |
```

These automatically capture when the form was opened (`start`) and submitted (`end`). Place them as the first two rows in the survey sheet.

#### **choices sheet** (for select questions):
- `list_name`: Reference name for the choice list
- `name`: Choice value (unique within list)
- `label`: Human-readable choice text
- `label::English (en)`: Translated choice labels (format: `label::Language Name (code)`)

#### **settings sheet** (optional but recommended):
- `form_title`: Form name
- `form_id`: Unique form identifier
- `version`: Form version number
- `default_language`: Primary language (format: `English (en)` or `Arabic (ar)`)
- `style`: Display style for the form (use `'pages'` for page-by-page navigation)

---

## Naming Conventions (Review Before Finalization)

### Suggested Structure:
**Format:** `section_subsection_questiontype_descriptor`

**Examples:**
- `demo_age_int` (demographics section, age, integer)
- `health_symptoms_sm` (health section, symptoms, select_multiple)
- `income_monthly_dec` (income section, monthly income, decimal)
- `consent_agreement_ack` (consent section, agreement, acknowledge)

**Choice List Names:**
- Format: `descriptor`
- Examples: `yesno`, `education_level`, `provinces`

**Principles:**
1. Use lowercase only
2. Use underscores, no spaces or hyphens
3. Keep names concise but meaningful
4. Use consistent abbreviations (int, dec, sm, so, txt)
5. Group related questions with section prefixes
6. Avoid numbers unless they represent actual sequence

---

## Validation and Constraints

### Smart Assumptions (User Can Edit Later):

**Age validation:**
- Constraint: `. >= 0 and . <= 120`
- Message: "Please enter a valid age between 0 and 120"

**Phone numbers:**
- Constraint: `regex(., '^[0-9]{10,15}$')`
- Message: "Please enter a valid phone number (10-15 digits)"

**Email:**
- Constraint: `regex(., '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}$')`
- Message: "Please enter a valid email address"

**Date ranges:**
- For birth dates: `. <= today()`
- For future dates: `. >= today()`

**Numeric ranges:**
- Use context clues (e.g., "on a scale of 1-5" → `. >= 1 and . <= 5`)

**Required fields:**
- Assume demographic fields are required (name, age, location)
- Leave assessment/opinion questions as optional unless specified

**Select multiple with limits:**
- Upper limit (e.g., "choose at most 2"): 
  - Constraint: `count-selected(.)<=2`
  - Message (Arabic): `يرجى تحديد خيارين على الأكثر`
  - Message (English): `Please select at most 2 options`
- Lower limit (e.g., "choose at least 2"):
  - Constraint: `count-selected(.)>=2`
  - Message (Arabic): `يرجى تحديد خيارين على الأقل`
  - Message (English): `Please select at least 2 options`

**Select multiple with "none of the above":**
- Constraint: `if(selected(.,'none'),count-selected(.)=1,count-selected(.)>0)`
- Message (Arabic): `لا يمكنك تحديد "ليس أي مما سبق" مع خيار آخر`
- Message (English): `You cannot select "None of the above" with other options`
- Logic: If 'none' is selected, only that option can be selected; otherwise, at least one option must be selected

**Arabic text only:**
- For fields explicitly requiring Arabic letters (e.g., "اكتب اسمك بالعربية")
- Constraint: `regex(.,'^[\u0621-\u064A ]+$')`
- Message (Arabic): `هذا الحقل يقبل الأحرف العربية فقط`
- Message (English): `This field accepts Arabic letters only`

**Latin text only:**
- For fields explicitly requiring Latin letters (e.g., "Write your name in English")
- Constraint: `regex(.,'^[A-Za-z ]+$')`
- Message (Arabic): `هذا الحقل يقبل الأحرف الإنجليزية فقط`
- Message (English): `This field accepts English letters only`

### Constraint Best Practices:
- Use `.` to refer to current field value
- Use `${field_name}` to reference other fields
- Combine with `and`, `or`, `not()`
- Common functions: `regex()`, `today()`, `selected()`, `count-selected()`

---

## Skip Logic (Relevant Column)

### Detection Patterns:
- "If answered yes to Q3..." → `${q3} = 'yes'`
- "For males only..." → `${gender} = 'male'`
- "If age is under 18..." → `${age} < 18`
- "Only if employed..." → `${employment_status} = 'employed'`

### Syntax Rules:
- Use `${field_name}` to reference previous questions
- For select_one: `${field} = 'value'`
- For select_multiple: `selected(${field}, 'value')`
- Combine conditions: `${field1} = 'yes' and ${field2} > 5`

---

## Advanced Features

### 1. Matrix Questions (Rating Grids)
For a set of rating questions displayed as a matrix (e.g., rate multiple items on the same scale):

**Pattern Recognition:**
- Must have an **introduction/label question** with the same type as subsequent questions
  - Examples: "Rate the following statements:", "Evaluate each aspect:", "How satisfied are you with:"
  - This question uses `appearance='label'` to display as a header
- Followed by **multiple actual questions** using the same response scale
  - Examples: "Quality of service", "Staff courtesy", "Cleanliness"

**survey sheet:**
```
type              | name              | label                           | required | appearance
select_one rating | service_intro     | Rate the following services:    | no       | label
select_one rating | service_quality   | Quality of service              | yes      | list-nolabel
select_one rating | staff_courtesy    | Staff courtesy                  | yes      | list-nolabel
select_one rating | cleanliness       | Cleanliness                     | yes      | list-nolabel
```

**Implementation:**
- First row: Same question type, `appearance = 'label'`, `required = no` (displays as header, no input needed)
- Subsequent rows: Same question type, `appearance = 'list-nolabel'` (hides column labels, creates matrix effect)
- All questions use the same `select_one [list_name]` or `select_multiple [list_name]`

**Critical: Verify the first question is truly introductory**
- ✓ "Rate the following aspects" → Matrix (introductory)
- ✓ "Evaluate each service below" → Matrix (introductory)
- ✗ "How satisfied are you with quality?" → NOT matrix (actual question)
- ✗ "Quality of service" → NOT matrix (actual question)

If there's no clear introductory/label question, use individual likert questions instead.

### 2. Groups with Field-List
For questions that should appear together on one screen:

```
type         | name            | label                  | appearance
begin_group  | demographics    | Demographics Section   | field-list
text         | first_name      | First Name             |
text         | last_name       | Last Name              |
integer      | age             | Age                    |
end_group    |                 |                        |
```

**Always set `appearance = 'field-list'` for `begin_group`** to improve data collection flow.

### 3. Likert Scale Questions
For rating scale questions (1-5, strongly disagree to strongly agree, etc.):

```
type                  | name              | label                        | appearance
select_one agreement  | service_satisfied | Satisfied with the service?  | likert
```

**Always set `appearance = 'likert'`** for standalone rating scale questions.

**IMPORTANT: Matrix vs Likert Decision Rule**

When you encounter rating questions, determine which pattern to use:

| Scenario | Pattern to Use | Appearance |
|----------|----------------|------------|
| Multiple rating questions with introductory text ("Rate the following...") | **Matrix** | `label` + `list-nolabel` |
| Single rating question | **Likert** | `likert` |
| Multiple rating questions WITHOUT introductory text | **Individual Likert** | `likert` for each |

**Example - Matrix Pattern:**
```
"Rate the following services:"  ← Introductory (not a real question)
- Quality                       ← Actual questions
- Speed
- Price
```
→ Use matrix (label + list-nolabel)

**Example - Likert Pattern:**
```
"How satisfied are you with quality?"  ← Standalone question
"How satisfied are you with speed?"    ← Separate standalone question
```
→ Use individual likert questions

**Key principle:** Matrix structure (intro + grid) takes precedence over the fact that questions use rating scales.

### 4. Cascading Selects
When questions have hierarchical choices (Country → Province → District):

**survey sheet:**
```
type              | name        | label          | choice_filter
select_one country| country     | Select country |
select_one province| province   | Select province| country=${country}
select_one district| district   | Select district| province=${province}
```

**choices sheet:**
```
list_name | name    | label          | country | province
country   | syria   | Syria          |         |
province  | damascus| Damascus       | syria   |
district  | mazzeh  | Mazzeh         | syria   | damascus
```

### 5. Repeat Groups
For repeating sections (household members, items, etc.):

```
type           | name              | label            | appearance
begin_repeat   | household_member  | Household Member | field-list
text           | member_name       | Name             |
integer        | member_age        | Age              |
end_repeat     |                   |                  |
```

### 6. Calculate Fields
For automatic computations:

```
type      | name        | label          | calculation
integer   | child_count | Children count |
calculate | adult_count | Adults count   | ${total_members} - ${child_count}
```

---

## Working with DOCX Input Files

When the user provides a DOCX questionnaire, extract content in the proper order (including tables).

**CRITICAL**: Using `doc.paragraphs` directly skips all tables. You must iterate through `doc.element.body` to capture everything in document order.

**Reference script available at:** `/mnt/skills/user/xlsform-converter/docx_extractor.py`
- Production-ready extraction function
- Handles paragraphs and tables in correct sequence
- Can be copied, imported, or used as a pattern

**Core extraction pattern:**
```python
from docx import Document

def extract_docx_content(filepath):
    doc = Document(filepath)
    content = []
    
    # Iterate through doc.element.body (NOT doc.paragraphs!)
    for i, element in enumerate(doc.element.body):
        if element.tag.endswith('p'):  # Paragraph
            for para in doc.paragraphs:
                if para._element == element:
                    text = para.text.strip()
                    if text:
                        content.append({
                            'type': 'paragraph',
                            'index': i,
                            'text': text
                        })
                    break
        
        elif element.tag.endswith('tbl'):  # Table
            for table in doc.tables:
                if table._element == element:
                    table_data = [[cell.text.strip() for cell in row.cells] 
                                  for row in table.rows]
                    content.append({
                        'type': 'table',
                        'index': i,
                        'data': table_data
                    })
                    break
    
    return content
```

**Output structure:**
- Paragraphs: `{'type': 'paragraph', 'index': N, 'text': '...'}`
- Tables: `{'type': 'table', 'index': N, 'data': [[cell1, cell2], ...]}`

### Common DOCX Gotchas

Watch out for:
- **Hidden/empty rows** in tables - May add unwanted structure
- **Merged cells** - Can cause extraction issues, verify cell content carefully
- **Nested tables** - Rare but possible in complex documents
- **RTL languages** (Arabic) - Ensure UTF-8 encoding is maintained
- **Images in questions** - Note them for manual handling or separate processing
- **Complex formatting** - Strip unnecessary formatting, preserve essential structure

---

## Workflow Steps

When user provides a text questionnaire:

### 1. **Analyze Input**
- **If DOCX file provided**: Use `/mnt/skills/user/xlsform-converter/scripts/docx_extractor.py` to extract content in proper order (tables + paragraphs)
- Read through the entire questionnaire
- Identify sections and subsections
- Note any explicit instructions about logic or validation

### 2. **Detect Question Types**
- Classify each question using detection patterns
- Identify choice lists for select questions
- Flag questions needing validation or constraints

### 3. **Handle Bilingual Requirements**
- Ask if translation is needed
- Extract domain-specific terms for verification
- **Output a table** showing proposed translations for user approval:

```
| Original Term | Proposed Translation | Context |
|---------------|---------------------|---------|
| [term1]       | [translation1]      | [where it appears] |
| [term2]       | [translation2]      | [where it appears] |
```

- Wait for user approval/corrections of translations before proceeding
- Apply approved translations to generate bilingual form

### 4. **Generate Naming Structure**
- Create meaningful field names following conventions
- Create choice list names
- Ensure uniqueness across the form

### 5. **Apply Smart Logic**
- Add relevant skip logic where context suggests it
- Add validation constraints for sensitive fields
- Mark appropriate fields as required

### 6. **Create Excel File**
- Structure survey, choices, and settings sheets
- Apply proper column headers
- Include bilingual columns if needed

### 7. **Document Assumptions**
After generating the file, provide a clear summary of **inferred** assumptions (not explicitly stated in the original questionnaire):

**Format:**
"I've made the following assumptions that you may want to review:"

For each **inferred** assumption, explain **why** it was chosen:
- **Validation rules**: "Added age constraint (0-120) because this is a standard demographic validation" *(only if not specified in original)*
- **Skip logic**: "Made Q5 depend on Q3='yes' because the phrasing implied conditionality" *(only if not explicitly stated)*
- **Required fields**: "Marked name/age as required because these are typically core identifiers" *(only if not specified)*
- **Naming conventions**: "Used `section_descriptor_type` format for consistency (e.g., `demo_age_int`)"
- **Question types**: "Classified Q7 as select_multiple based on the phrasing style" *(only if ambiguous)*
- **Matrix structure**: "Created matrix for Q8-Q11 because they share the same scale" *(if structure wasn't explicit)*
- **Phrasing adjustments**: If ANY text was modified (with permission), list one-by-one: 
  - Original: "How old are you?" → Modified: "What is your age?"

**Do NOT document:**
- Explicit instructions from the original questionnaire (e.g., "If yes, skip to Q10" → this is explicit skip logic)
- Clearly stated validation requirements (e.g., "Age must be 18+" → this is explicit constraint)
- Obviously indicated question types (e.g., "Select all that apply" → clearly select_multiple)

**End with iteration note:**
"You can refine any of these decisions - naming conventions, validation rules, skip logic, or question types. Just let me know what you'd like to adjust."

---

## Output Format

Generate an actual .xlsx file with:
1. Properly formatted sheets (survey, choices, settings)
2. All required columns present
3. No empty rows between questions
4. Consistent formatting
5. Clear, descriptive labels

**Always include a brief README in a comment at the top of the survey sheet explaining:**
- Form purpose (if evident)
- Number of questions
- Languages included
- Any special features (cascading, repeats, calculations)

---

## Best Practices Checklist

Before finalizing output, verify:
- ✓ Form includes `start` and `end` metadata fields as first two rows
- ✓ All field names are unique and lowercase
- ✓ No spaces in field names or choice values
- ✓ All select questions have corresponding choice lists
- ✓ Choice list names don't use redundant prefixes (use `yesno`, not `list_yesno`)
- ✓ Required fields are logically necessary
- ✓ Constraints have user-friendly error messages
- ✓ Skip logic references valid field names
- ✓ Bilingual forms have consistent translations
- ✓ choice_filter syntax is correct for cascading
- ✓ No circular dependencies in skip logic
- ✓ Repeat groups are properly closed
- ✓ Calculate fields have valid expressions
- ✓ All `begin_group` and `begin_repeat` have `appearance = 'field-list'`

---

## Special Considerations

### Arabic Language Support
- Use `label::Arabic (ar)` for Arabic labels (full format: `Language Name (code)`)
- Set `default_language = 'Arabic (ar)'` if Arabic is primary
- Watch for proper character encoding (UTF-8)

### Validation Messages
- Keep error messages helpful and non-technical
- Use positive language ("Please enter..." vs "Invalid...")
- Bilingual error messages for better user experience

---

## Reference Documentation

**KoboToolbox Documentation:** https://support.kobotoolbox.org/

Consult when encountering edge cases or features not covered in this skill.

---

## Final Notes

- **Don't over-engineer**: Keep forms as simple as possible while meeting requirements
- **Prioritize usability**: Data collectors should find the form intuitive
