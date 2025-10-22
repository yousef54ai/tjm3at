# تجمعات (tjm3at) - Project Documentation

## Overview

**تجمعات** (tjm3at) is a static website for hosting educational exam collections, built with pure HTML, CSS, and JSON. The project is hosted on GitHub Pages and provides an interactive quiz-taking experience with immediate feedback and personalized review recommendations.

## Purpose

The website organizes exams by classes, allowing students to:
- Practice with multiple-choice and true/false questions
- Receive immediate feedback on their answers
- Get personalized study recommendations based on mistakes
- Track progress through visual indicators
- Celebrate perfect scores with fireworks animations

## Technology Stack

- **HTML5** - Structure and content
- **CSS3** - Styling with custom properties and animations
- **JavaScript (Vanilla)** - Dynamic content loading and interactivity
- **JSON** - All content storage (classes, exams, questions)
- **GitHub Pages** - Static site hosting
- **Google Fonts** - Rubik (Arabic), Noto Sans (English)

## Design System

### Color Palette - Solarized Dark

```css
--bg-primary: #002b36      /* Main background */
--bg-secondary: #073642    /* Card backgrounds */
--text-primary: #839496    /* Body text */
--text-emphasized: #93a1a1 /* Emphasized text */
--text-bright: #eee8d5     /* Brightest text (card titles) */
--cyan: #2aa198           /* Page headers only */
--purple: #6c71c4         /* Accents and borders */
--magenta: #d33682
--green: #859900          /* Success states */
--yellow: #b58900
--orange: #cb4b16         /* Warning states */
--red: #dc322f            /* Error states */
```

### Typography

- **Arabic (RTL)**: Rubik font family
- **English (LTR)**: Noto Sans font family
- **Responsive sizing**: All font sizes use `clamp()` for fluid scaling
- **Font sizes**:
  - `--fs-sm`: clamp(0.75rem, 0.7rem + 0.3vw, 0.875rem)
  - `--fs-base`: clamp(0.875rem, 0.8rem + 0.4vw, 1rem)
  - `--fs-lg`: clamp(1rem, 0.9rem + 0.5vw, 1.25rem)
  - `--fs-xl`: clamp(1.25rem, 1rem + 1vw, 1.5rem)
  - `--fs-2xl`: clamp(1.5rem, 1.2rem + 1.5vw, 2rem)
  - `--fs-3xl`: clamp(2rem, 1.5rem + 2vw, 3rem)

### Spacing

All spacing uses responsive clamp() values:
- `--spacing-xs`: clamp(0.25rem, 0.2rem + 0.25vw, 0.5rem)
- `--spacing-sm`: clamp(0.5rem, 0.4rem + 0.5vw, 1rem)
- `--spacing-md`: clamp(1rem, 0.8rem + 1vw, 1.5rem)
- `--spacing-lg`: clamp(1.5rem, 1.2rem + 1.5vw, 2rem)
- `--spacing-xl`: clamp(2rem, 1.5rem + 2vw, 3rem)

### Design Principles

1. **No white colors** - Uses `#eee8d5` (cream) as the brightest color
2. **No hover effects** - Mobile-first design (hover doesn't work on touch)
3. **Minimal borders** - Uses shadows and 3D effects instead
4. **Purple 3D effect** - 5px solid purple left border on cards for depth
5. **Cyan for h1 only** - Page headers use cyan, card titles use bright color
6. **Consistent card heights** - Text wrapping handled with min-height

## File Structure

```
tjm3at/
├── index.html              # Home page - class selection
├── class.html              # Class page - exam list
├── exam.html               # Exam page - quiz interface
├── css/
│   ├── global.css          # Base styles and CSS variables
│   ├── home.css            # Home page specific styles
│   ├── class.css           # Class page specific styles
│   └── exam.css            # Exam page specific styles
├── data/
│   ├── home.json           # Class list for home page
│   ├── classes/
│   │   ├── MATH-001.json   # Exam list for each class
│   │   ├── CS-001.json
│   │   ├── COMM-001.json
│   │   └── CI-001.json
│   └── exams/
│       ├── MATH-001/       # Exam data organized by class
│       │   └── exam-001.json
│       ├── CS-001/
│       ├── COMM-001/
│       └── CI-001/
├── imgs/
│   └── class/              # Class images (not used - emojis preferred)
├── examples/               # Original HTML exam examples
└── test/                   # Test files for GitHub Pages setup
```

## Pages

### 1. Home Page (index.html)

**Purpose**: Display all available classes for selection

**Features**:
- Fetches `data/home.json` on load
- Displays classes in a responsive card grid
- Each card shows:
  - Emoji icon
  - Class name (Arabic or English)
  - Class code
  - Description
- Fade-in animation on load
- Links to class page with `?id={classId}`

**Navigation**: Home → Class Page

### 2. Class Page (class.html)

**Purpose**: Display all exams for a selected class

**Features**:
- Fetches `data/classes/{CLASS-ID}.json` based on URL parameter
- Shows breadcrumb navigation (Home → Class Name)
- Header displays "تجميعات {className}"
- Lists all exams in cards showing:
  - Exam title
  - Exam description
- Purple 3D effect on cards
- Links to exam page with `?class={classId}&exam={examId}`

**Navigation**: Home → Class → Exam

### 3. Exam Page (exam.html)

**Purpose**: Interactive quiz interface with immediate feedback

**Features**:

#### Core Functionality
- Fetches exam data from `data/exams/{CLASS-ID}/{exam-id}.json`
- One question at a time display
- Progress bar visual indicator
- Progress text showing answered count (e.g., "5 / 20")
- Next/Back navigation buttons
- Finish button (appears on last question, enabled when all answered)

#### Question Display
- Question text
- Multiple choice options with letter labels (A, B, C, D)
- Immediate feedback on answer selection:
  - Selected answer highlighted (green for correct, red for incorrect)
  - Correct answer always shown in green
  - Feedback panel with explanation
  - All options disabled after selection

#### Results Screen
- **Perfect Score (100%)**:
  - Celebration message
  - 100% display in green
  - Fireworks animation (50 bursts)
  - "Try Again" button

- **Imperfect Score**:
  - Score percentage with color coding:
    - 80%+: cyan (good)
    - 60-79%: orange (needs work)
    - <60%: orange (review basics)
  - Personalized message
  - Review section listing all mistakes with:
    - Chapter name
    - Section name
    - Page number
    - Additional info/tips
  - "Try Again" button

#### Bilingual Support
- Detects language from exam JSON (`language: "ar"` or `"en"`)
- Sets page direction (RTL for Arabic, LTR for English)
- Uses translation object for all UI text:
  - Button labels (Back, Next, Finish, Try Again)
  - Feedback messages (Correct, Incorrect)
  - Results titles and messages
  - All static text

**Navigation**: Can restart exam or return to class page

## Key Features

### 1. Bilingual Support (Arabic RTL / English LTR)

The website automatically adapts to the language specified in JSON:

```javascript
// Language detection
document.documentElement.dir = examData.language === 'ar' ? 'rtl' : 'ltr';
document.documentElement.lang = examData.language;

// Translation system
const translations = {
  ar: { question: 'السؤال', back: 'السابق', ... },
  en: { question: 'Question', back: 'Back', ... }
};
```

CSS handles directional changes:
```css
.option-letter {
  margin-left: var(--spacing-sm);  /* Default for RTL */
}

[dir="ltr"] .option-letter {
  margin-left: 0;
  margin-right: var(--spacing-sm);
}
```

### 2. Immediate Feedback System

- On answer selection, user cannot change their answer
- Selected option highlighted immediately
- Correct answer always shown (even if user was wrong)
- Explanation panel displays with detailed reasoning
- Uses Solarized colors for success/error states

### 3. Review Recommendations

Based on incorrect answers, the system shows:
- **Chapter**: Where to find the topic
- **Section**: Specific section within chapter
- **Page**: Exact page number to review
- **Info**: Additional tip or explanation

This helps students focus their study time efficiently.

### 4. Perfect Score Celebration

When user answers all questions correctly:
- Fireworks animation with 50 colorful bursts
- Celebration message in the exam's language
- Visual reward for mastery

### 5. Progress Tracking

- Visual progress bar fills as questions are answered
- Text counter shows "X / Total"
- Users can navigate back to review previous questions
- Finish button only enabled when all questions answered

### 6. Responsive Design

All elements scale fluidly using `clamp()`:
- Works on mobile phones, tablets, and desktops
- No media queries needed for most sizing
- Container max-width: 900px for optimal readability
- Touch-friendly (no hover effects)

## CSS Architecture

### global.css
- CSS custom properties (variables)
- Reset and base styles
- Container styles
- Utility classes (fade-in, loaded)
- Font family settings for RTL/LTR

### home.css
- Class card grid layout
- Icon sizing and styling
- Card hover states (removed per design decision)
- Responsive card grid

### class.css
- Breadcrumb navigation
- Class header styling
- Exam card list layout
- Same purple 3D effect as home

### exam.css
- Progress bar and text
- Question card layout
- Option buttons (multiple choice)
- Option letter circles
- Feedback panels (correct/incorrect)
- Navigation buttons
- Results screen
- Review section
- Fireworks animation
- All exam-specific states

## Navigation Flow

```
Home Page (index.html)
    ↓ Click class card
Class Page (class.html?id=MATH-001)
    ↓ Click exam card
Exam Page (exam.html?class=MATH-001&exam=exam-001)
    ↓ Complete exam
Results Screen (same page)
    ↓ Click "Try Again"
Exam Page (reloads)
```

## Development Decisions

### Why No Images for Classes?
Initially, images were added for each class, but we reverted to emojis because:
- Emojis are easier to maintain
- No file management needed
- Perfect for icon-sized display
- Consistent across all devices
- Faster loading

### Why Remove Exam Count from JSON?
Originally, `data/home.json` included exam counts, but we removed it:
- Makes adding new exams simpler (one less place to update)
- Count can be calculated dynamically if needed
- Reduces maintenance burden

### Why No Hover Effects?
Mobile devices don't support hover states:
- Hover effects don't work on touch screens
- Creates inconsistent experience
- Better to design for touch-first

### Why Separate CSS Files?
Each page has unique styling needs:
- Smaller file sizes (users only load what they need)
- Easier to maintain and debug
- Clear separation of concerns
- `global.css` contains only shared styles

### Why One Question at a Time?
Better user experience:
- Reduces cognitive load
- Focuses attention on current question
- Works well on small screens
- Allows for better feedback presentation
- Progress tracking more meaningful

### Why Show Correct Answer Even When Right?
Reinforcement learning:
- Confirms user made the right choice
- Visual feedback is satisfying
- Consistent behavior (always highlight correct)
- No confusion about which was correct

### Why Remove Question Number Badge?
Design simplification:
- Progress text at top already shows position
- Reduces visual clutter
- One source of truth for progress
- Badge was redundant information

## Data Flow

1. **Home Page Load**:
   - Fetch `data/home.json`
   - Parse JSON
   - Generate class cards dynamically
   - Apply fade-in animation

2. **Class Page Load**:
   - Extract `id` from URL parameter
   - Fetch `data/classes/{id}.json`
   - Generate exam cards
   - Update breadcrumb with class name

3. **Exam Page Load**:
   - Extract `class` and `exam` from URL parameters
   - Fetch `data/exams/{class}/{exam}.json`
   - Set page language and direction
   - Initialize translation text
   - Render all questions (hidden)
   - Show first question
   - Track user answers in array

4. **Answer Selection**:
   - Save answer to `userAnswers` array
   - Disable all options
   - Highlight selected and correct answers
   - Show feedback panel
   - Update progress bar
   - Enable navigation

5. **Finish Exam**:
   - Calculate score
   - If perfect: show celebration + fireworks
   - If not: show score + review topics
   - Hide exam content
   - Show results screen

## GitHub Pages Deployment

The site is deployed as a static GitHub Pages site:

1. Push all files to GitHub repository
2. Enable GitHub Pages in repository settings
3. Source: main branch, root directory
4. Site is automatically built and deployed
5. Accessible at: `https://[username].github.io/tjm3at/`

No build process needed - pure HTML/CSS/JS!

## Future Enhancements (Not Implemented)

Potential features for future development:
- Local storage for saving progress
- Score history and analytics
- Timed exams
- Randomized question order
- Multiple attempts tracking
- Export results as PDF
- Study mode (see answers before taking exam)
- Difficulty levels
- Tags/categories for questions
- Search functionality

## Credits

- **Design**: Solarized Dark color scheme by Ethan Schoonover
- **Fonts**: Google Fonts (Rubik, Noto Sans)
- **Hosting**: GitHub Pages
- **Development**: Built with Claude Code

## License

This is an educational project. Exam content should respect copyright of original materials.
