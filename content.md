# Content Management Guide

This guide explains how to add new classes and exams to the تجمعات (tjm3at) website.

## Table of Contents

1. [Quick Start](#quick-start)
2. [Adding a New Class](#adding-a-new-class)
3. [Adding a New Exam](#adding-a-new-exam)
4. [JSON Schema Reference](#json-schema-reference)
5. [Naming Conventions](#naming-conventions)
6. [Question Writing Guidelines](#question-writing-guidelines)
7. [Common Mistakes to Avoid](#common-mistakes-to-avoid)

---

## Quick Start

**To add a new class:**
1. Add class entry to `data/home.json`
2. Create `data/classes/{CLASS-ID}.json` file
3. Done! The class appears on the home page

**To add a new exam:**
1. Create `data/exams/{CLASS-ID}/{exam-id}.json` file
2. Add exam entry to `data/classes/{CLASS-ID}.json`
3. Done! The exam appears in the class page

---

## Adding a New Class

### Step 1: Update Home Page Data

Edit `data/home.json` and add a new class object to the `classes` array:

```json
{
  "language": "ar",
  "title": "تجمعات",
  "description": "مجموعة من الامتحانات التعليمية",
  "classes": [
    {
      "id": "MATH-001",
      "name": "أساسيات الرياضيات",
      "code": "MATH-001",
      "description": "Fundamentals of Math",
      "icon": "📐"
    },
    {
      "id": "NEW-CLASS-001",
      "name": "اسم المادة الجديدة",
      "code": "NEW-CLASS-001",
      "description": "English Description",
      "icon": "🎓"
    }
  ]
}
```

**Field Explanations:**
- `id`: Unique identifier (e.g., "MATH-001", "CS-001") - must match filename
- `name`: Display name in Arabic or primary language
- `code`: Class code (usually same as id)
- `description`: English or secondary description
- `icon`: Emoji icon (single emoji character)

### Step 2: Create Class File

Create a new file at `data/classes/{CLASS-ID}.json`:

```json
{
  "language": "ar",
  "id": "NEW-CLASS-001",
  "name": "اسم المادة الجديدة",
  "code": "NEW-CLASS-001",
  "description": "English Description",
  "exams": []
}
```

**Important:**
- The filename must match the `id` field (e.g., `NEW-CLASS-001.json`)
- `exams` array starts empty - you'll add exams later
- The `id` must match the `id` in `data/home.json`

### Step 3: Create Exams Folder

Create a folder for the class's exams:

```bash
mkdir -p data/exams/NEW-CLASS-001
```

Now you can add exams to this class!

---

## Adding a New Exam

### Step 1: Create Exam File

Create `data/exams/{CLASS-ID}/{exam-id}.json`:

```json
{
  "language": "en",
  "id": "exam-001",
  "classId": "NEW-CLASS-001",
  "title": "First Exam",
  "subtitle": "Introduction to the Topic",
  "description": "Practice fundamental concepts",
  "skills": [
    "Skill 1",
    "Skill 2",
    "Skill 3"
  ],
  "questions": [
    {
      "id": 1,
      "type": "multiple-choice",
      "question": "What is 2 + 2?",
      "options": [
        "3",
        "4",
        "5",
        "6"
      ],
      "correctAnswer": 1,
      "explanation": "Correct! 2 + 2 = 4. This is basic arithmetic.",
      "review": {
        "chapter": "Chapter 1: Basic Arithmetic",
        "section": "1.1 Addition",
        "page": "5",
        "info": "Review addition of single-digit numbers"
      }
    }
  ]
}
```

**Field Explanations:**

- `language`: "ar" for Arabic (RTL) or "en" for English (LTR)
- `id`: Unique exam identifier within the class (e.g., "exam-001")
- `classId`: Must match the class folder name
- `title`: Main exam title (shown in header)
- `subtitle`: Secondary title (currently not displayed)
- `description`: Exam description (currently not displayed)
- `skills`: Array of skills covered (for reference)
- `questions`: Array of question objects (see below)

### Step 2: Add Exam to Class

Edit `data/classes/{CLASS-ID}.json` and add the exam to the `exams` array:

```json
{
  "language": "ar",
  "id": "NEW-CLASS-001",
  "name": "اسم المادة الجديدة",
  "code": "NEW-CLASS-001",
  "description": "English Description",
  "exams": [
    {
      "id": "exam-001",
      "title": "First Exam",
      "description": "Introduction to the Topic"
    }
  ]
}
```

**Important:**
- The `id` must match the exam filename (without `.json`)
- The `title` and `description` appear on the class page

---

## JSON Schema Reference

### 1. Home Data (`data/home.json`)

```typescript
{
  language: "ar" | "en",
  title: string,
  description: string,
  classes: [
    {
      id: string,              // Unique, matches filename
      name: string,            // Display name
      code: string,            // Class code (usually same as id)
      description: string,     // Secondary description
      icon: string             // Single emoji character
    }
  ]
}
```

### 2. Class Data (`data/classes/{CLASS-ID}.json`)

```typescript
{
  language: "ar" | "en",
  id: string,                  // Must match filename
  name: string,                // Display name
  code: string,                // Class code
  description: string,         // Secondary description
  exams: [
    {
      id: string,              // Exam identifier
      title: string,           // Exam title
      description: string      // Exam description
    }
  ]
}
```

### 3. Exam Data (`data/exams/{CLASS-ID}/{exam-id}.json`)

```typescript
{
  language: "ar" | "en",       // Determines RTL/LTR and UI text
  id: string,                  // Must match filename
  classId: string,             // Must match class folder
  title: string,               // Main exam title
  subtitle: string,            // Secondary title (optional)
  description: string,         // Exam description (optional)
  skills: string[],            // Array of skills covered
  questions: Question[]        // Array of question objects
}
```

### 4. Question Object

```typescript
{
  id: number,                  // Sequential number starting from 1
  type: "multiple-choice" | "true-false",
  question: string,            // The question text
  options: string[],           // Array of answer choices
  correctAnswer: number,       // Index of correct option (0-based)
  explanation: string,         // Detailed explanation of answer
  review: {
    chapter: string,           // Chapter name or number
    section: string,           // Section within chapter
    page: string,              // Page number (as string)
    info: string               // Additional tip or guidance
  }
}
```

**Question Types:**
- `"multiple-choice"`: Standard multiple choice (2-6 options)
- `"true-false"`: True/false questions (2 options)

**correctAnswer Index:**
- 0 = first option (A)
- 1 = second option (B)
- 2 = third option (C)
- 3 = fourth option (D)
- etc.

---

## Naming Conventions

### Class IDs

Format: `{SUBJECT}-{NUMBER}`

Examples:
- `MATH-001` - Fundamentals of Math
- `CS-001` - Introduction to CS
- `COMM-001` - Communication Skills
- `CI-001` - Academic Skills

**Rules:**
- Use uppercase letters
- Use hyphens, not underscores
- Use 3-digit zero-padded numbers (001, 002, 003)
- Keep subject codes short (2-5 characters)

### Exam IDs

Format: `exam-{NUMBER}`

Examples:
- `exam-001` - First exam
- `exam-002` - Second exam
- `exam-010` - Tenth exam

**Rules:**
- Lowercase "exam"
- Hyphen separator
- 3-digit zero-padded numbers
- Sequential within each class

### File Paths

**Class files:**
```
data/classes/MATH-001.json
data/classes/CS-001.json
```

**Exam files:**
```
data/exams/MATH-001/exam-001.json
data/exams/MATH-001/exam-002.json
data/exams/CS-001/exam-001.json
```

**Pattern:**
- Class ID must match folder name
- Exam ID must match filename (without .json)

---

## Question Writing Guidelines

### 1. Writing Good Questions

**DO:**
- Be clear and specific
- Use simple, direct language
- Focus on one concept per question
- Include all necessary information
- Make distractors (wrong answers) plausible

**DON'T:**
- Use ambiguous wording
- Include trick questions
- Make questions too long or complex
- Use "all of the above" or "none of the above"
- Include hints in the question

### 2. Writing Explanations

Good explanations should:
- Start with "Correct!" for positive reinforcement
- Explain WHY the answer is correct
- Reference the underlying concept or rule
- Mention when/where this skill is used
- Be concise but thorough (1-3 sentences)

**Example:**
```
"Correct! When adding numbers with different signs, subtract the smaller
absolute value from the larger (15 − 8 = 7) and keep the sign of the
larger absolute value. Since |−15| > |8|, the answer is negative: −7.
This skill is used constantly when simplifying algebraic expressions."
```

### 3. Creating Review Objects

The review object helps students find topics to study:

```json
"review": {
  "chapter": "Chapter 1: Introduction to Algebra",
  "section": "1.2 Operations with Signed Numbers",
  "page": "12",
  "info": "Review addition rules for numbers with different signs"
}
```

**Tips:**
- `chapter`: Include chapter number and full name
- `section`: Include section number and name
- `page`: Page number as string (can include ranges: "12-15")
- `info`: Specific guidance on what to review

### 4. Multiple Choice Options

**Best practices:**
- Use 4 options for most questions (A, B, C, D)
- Make all options similar in length
- Avoid obvious patterns
- Order options logically (numerically, alphabetically, etc.)

**For true/false questions:**
```json
"options": ["True", "False"]
```
or in Arabic:
```json
"options": ["صح", "خطأ"]
```

### 5. Language Consistency

**If `language: "ar"`:**
- Question text in Arabic
- Options in Arabic
- Explanation in Arabic
- Review object in Arabic
- UI will display in Arabic (RTL)

**If `language: "en"`:**
- Question text in English
- Options in English
- Explanation in English
- Review object in English
- UI will display in English (LTR)

**Important:** Don't mix languages within an exam!

---

## Common Mistakes to Avoid

### ❌ Mistake 1: ID Mismatches

```json
// WRONG: ID doesn't match filename
// File: data/classes/MATH-001.json
{
  "id": "MATH-002",  // ❌ Wrong ID
  ...
}

// RIGHT:
// File: data/classes/MATH-001.json
{
  "id": "MATH-001",  // ✓ Matches filename
  ...
}
```

### ❌ Mistake 2: Wrong correctAnswer Index

```json
// Question with 4 options
{
  "options": ["A", "B", "C", "D"],
  "correctAnswer": 4  // ❌ Out of bounds! (0-3 are valid)
}

// RIGHT:
{
  "options": ["A", "B", "C", "D"],
  "correctAnswer": 0  // ✓ First option (A)
}
```

### ❌ Mistake 3: Forgetting to Update Both Files

When adding an exam, you must:
1. ✓ Create `data/exams/{CLASS-ID}/{exam-id}.json`
2. ✓ Add entry to `data/classes/{CLASS-ID}.json`

Missing step 2 means the exam won't appear on the class page!

### ❌ Mistake 4: Invalid JSON Syntax

Common JSON errors:
```json
// WRONG:
{
  "name": "Test",  // ❌ Trailing comma
}

// WRONG:
{
  name: "Test"  // ❌ Unquoted key
}

// WRONG:
{
  "name": 'Test'  // ❌ Single quotes
}

// RIGHT:
{
  "name": "Test"
}
```

**Tip:** Use a JSON validator or IDE with JSON support to catch syntax errors!

### ❌ Mistake 5: Mixed Languages

```json
// WRONG:
{
  "language": "ar",
  "question": "What is 2+2?",  // ❌ English question in Arabic exam
  "options": ["٢", "٤", "٦", "٨"]
}

// RIGHT:
{
  "language": "ar",
  "question": "ما هو ٢+٢؟",  // ✓ Arabic question
  "options": ["٢", "٤", "٦", "٨"]
}
```

### ❌ Mistake 6: Missing Required Fields

All fields in the schema are required. Missing fields will cause errors:

```json
// WRONG:
{
  "id": 1,
  "question": "What is 2+2?",
  "options": ["2", "4", "6"],
  "correctAnswer": 1
  // ❌ Missing: type, explanation, review
}

// RIGHT:
{
  "id": 1,
  "type": "multiple-choice",
  "question": "What is 2+2?",
  "options": ["2", "4", "6"],
  "correctAnswer": 1,
  "explanation": "Correct! 2 + 2 = 4.",
  "review": {
    "chapter": "Chapter 1",
    "section": "1.1 Addition",
    "page": "5",
    "info": "Review basic addition"
  }
}
```

### ❌ Mistake 7: Wrong Folder Structure

```
// WRONG:
data/exams/exam-001.json  // ❌ Missing class folder

// RIGHT:
data/exams/MATH-001/exam-001.json  // ✓ Inside class folder
```

---

## Icon Reference

Recommended emojis for class icons:

**Academic Subjects:**
- 📐 Mathematics
- 💻 Computer Science
- 🧪 Chemistry
- 🔬 Physics
- 🧬 Biology
- 📚 Literature
- ✍️ Writing
- 💬 Communication
- 🌍 Geography
- 📜 History
- 🎨 Art
- 🎵 Music
- ⚖️ Law
- 💼 Business
- 🏥 Medicine

**Skills:**
- 🎓 Academic Skills
- 🗣️ Speaking
- 👂 Listening
- 📖 Reading
- ✏️ Writing
- 🧮 Calculation
- 🧠 Critical Thinking
- 🔍 Research

**Pick emojis that:**
- Clearly represent the subject
- Display well on all devices
- Are easily recognizable
- Use single character (not combinations)

---

## Testing Your Content

After adding new content:

1. **Validate JSON**: Use an online JSON validator or `jq` command
   ```bash
   jq . data/classes/NEW-CLASS-001.json
   ```

2. **Check links**:
   - Home → Class should load the class page
   - Class → Exam should load the exam page
   - No 404 errors in browser console

3. **Test exam functionality**:
   - All questions display correctly
   - Correct answers highlight properly
   - Explanations show up
   - Results screen displays review topics
   - "Try Again" button works

4. **Test both languages**:
   - Create one exam with `"language": "ar"`
   - Create another with `"language": "en"`
   - Verify UI text changes appropriately

---

## Quick Reference Card

### Add New Class Checklist
- [ ] Add entry to `data/home.json`
- [ ] Create `data/classes/{CLASS-ID}.json`
- [ ] Create `data/exams/{CLASS-ID}/` folder
- [ ] Choose an emoji icon
- [ ] Verify class appears on home page

### Add New Exam Checklist
- [ ] Create `data/exams/{CLASS-ID}/{exam-id}.json`
- [ ] Write all questions with correct schema
- [ ] Add review objects for each question
- [ ] Add exam entry to `data/classes/{CLASS-ID}.json`
- [ ] Validate JSON syntax
- [ ] Test exam in browser
- [ ] Verify all answers work correctly
- [ ] Check review topics display properly

### JSON Validation Checklist
- [ ] All required fields present
- [ ] No trailing commas
- [ ] All keys in double quotes
- [ ] All string values in double quotes
- [ ] `correctAnswer` index is valid (0-based)
- [ ] `language` is either "ar" or "en"
- [ ] `id` matches filename
- [ ] `classId` matches folder name

---

## Example: Complete Workflow

Let's add a new class "Physics 101" with one exam:

### 1. Update home.json
```json
{
  "id": "PHYS-101",
  "name": "فيزياء ١٠١",
  "code": "PHYS-101",
  "description": "Introduction to Physics",
  "icon": "🔬"
}
```

### 2. Create data/classes/PHYS-101.json
```json
{
  "language": "ar",
  "id": "PHYS-101",
  "name": "فيزياء ١٠١",
  "code": "PHYS-101",
  "description": "Introduction to Physics",
  "exams": [
    {
      "id": "exam-001",
      "title": "الحركة والقوة",
      "description": "Motion and Forces - Chapter 1"
    }
  ]
}
```

### 3. Create data/exams/PHYS-101/exam-001.json
```json
{
  "language": "ar",
  "id": "exam-001",
  "classId": "PHYS-101",
  "title": "الحركة والقوة",
  "subtitle": "الفصل الأول",
  "description": "اختبار المفاهيم الأساسية",
  "skills": ["الحركة", "القوة", "التسارع"],
  "questions": [
    {
      "id": 1,
      "type": "multiple-choice",
      "question": "ما هي وحدة قياس القوة في النظام الدولي؟",
      "options": ["جول", "نيوتن", "واط", "باسكال"],
      "correctAnswer": 1,
      "explanation": "صحيح! النيوتن هو وحدة قياس القوة في النظام الدولي (SI). القوة = الكتلة × التسارع، والنيوتن يساوي كيلوغرام متر لكل ثانية مربعة.",
      "review": {
        "chapter": "الفصل الأول: الحركة والقوة",
        "section": "١.٣ قوانين نيوتن",
        "page": "٢٥",
        "info": "راجع تعريف القوة ووحدات القياس"
      }
    }
  ]
}
```

### 4. Test
- Visit home page → See Physics card
- Click Physics → See "الحركة والقوة" exam
- Click exam → Take the quiz
- Verify RTL layout
- Check Arabic UI text

Done! 🎉

---

## Need Help?

If you encounter issues:
1. Check browser console for errors (F12)
2. Validate your JSON syntax
3. Verify all IDs match filenames
4. Check that folder structure is correct
5. Review this guide for common mistakes

Happy content creation! 📚
