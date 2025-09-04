# Duolingo UI Clone - Complete Documentation

## Overview
This is a comprehensive documentation of the TryLingo UI system - a Duolingo clone built with Next.js 14, Tailwind CSS, and shadcn/ui.

## Core UI System

### 1. Color Palette & Theme

```css
/* Primary Colors */
--primary: 142.1 76.2% 36.3% (green - #22cc5e)
--primary-foreground: white
--primary-depth: darker green for shadows
--primary-light: lighter green variant

/* Secondary Colors */
--secondary: 24 9.8% 10% (dark gray)
--secondary-foreground: white

/* Special Colors */
--highlight: 45.2 100% 51.2% (yellow/gold)
--super: 234 89% 73% (blue)
--destructive: 0 84.2% 60.2% (red)
```

### 2. Button System

The button system has 12+ variants for different states:

```tsx
// Button variants from components/ui/button.tsx
const buttonVariants = {
  variant: {
    default: "bg-white text-black border-slate-200 hover:bg-slate-100",
    primary: "bg-primary text-primary-foreground hover:brightness-110 border-primary-depth",
    secondary: "bg-secondary text-secondary-foreground hover:brightness-110 border-secondary-depth",
    danger: "bg-destructive text-destructive-foreground hover:brightness-110 border-destructive-depth",
    super: "bg-super text-super-foreground hover:brightness-110 border-super-depth",
    highlight: "bg-highlight text-background hover:brightness-110 border-highlight-depth",
    golden: "bg-gradient-to-b from-[#FFDE7B] via-[#FFC542] to-[#FFB110]",
    locked: "bg-disabled text-disabled-foreground border-disabled hover:bg-disabled",
    ghost: "bg-transparent hover:bg-black/5",
    immersive: "bg-transparent border-transparent",
    active: "bg-primary-light text-primary border-primary",
    correct: "bg-correct-light text-correct border-correct",
    incorrect: "bg-incorrect-light text-incorrect border-incorrect"
  },
  size: {
    default: "h-11 px-4 py-2",
    sm: "h-9 px-3",
    lg: "h-12 px-8",
    icon: "h-11 w-11",
    rounded: "h-full w-full rounded-full",
  }
}
```

### 3. Lesson Button Components

The lesson system uses a sophisticated button hierarchy:

```tsx
// Learn Button Structure
/components/user/learn/LearnButton/
├── index.tsx         // Main export
├── ButtonBase.tsx    // Core 70px circular button
├── ActiveButton.tsx  // Unlocked, clickable lesson
├── CurrentButton.tsx // In-progress with gauge
└── LockedButton.tsx  // Locked state

// ButtonBase Properties
- Size: 70px diameter
- Border: 8px colored border
- Shadow: Drop shadow for depth
- Icon: Centered icon (Check, Star, Crown, Swords)
```

### 4. Learn Page Layout

The learn page uses a zigzag pattern for visual interest:

```tsx
// Zigzag positioning logic
const cycleLength = 8;
const cycleIndex = index % cycleLength;

let indentationLevel;
if (cycleIndex <= 2) {
  indentationLevel = cycleIndex;
} else if (cycleIndex <= 4) {
  indentationLevel = 4 - cycleIndex;
} else if (cycleIndex <= 6) {
  indentationLevel = cycleIndex - 4;
} else {
  indentationLevel = cycleIndex - 8;
}

// Results in pattern: 0, 1, 2, 1, 0, 1, 2, 1...
```

### 5. Unit Structure

Each unit contains:
- **Banner**: Decorative header with title/description
- **Lessons**: Array of lesson buttons in zigzag layout
- **Progress tracking**: Percentage completion
- **Color variants**: Cycles through primary, secondary, danger, super

### 6. Progress Visualization

```tsx
// Circular Progress Gauge (using @suyalcinkaya/gauge)
<Gauge
  value={Number(percentage)}
  size="small"
  showValue={false}
  primary={variant === "primary" ? "#22cc5e" : "#FFB110"}
  secondary="#e5e5e5"
/>
```

### 7. Responsive Interactions

Desktop vs Mobile patterns:
- **Desktop**: Popover for lesson info
- **Mobile**: Drawer sliding from bottom
- **Breakpoint**: 640px (sm in Tailwind)

```tsx
// Adaptive Modal Pattern
const isDesktop = useMediaQuery("(min-width: 640px)");

if (isDesktop) {
  return <Popover>{content}</Popover>;
} else {
  return <Drawer>{content}</Drawer>;
}
```

### 8. Animation System

Key animation patterns using Framer Motion:

```tsx
// Entrance animations
initial={{ opacity: 0, y: 20 }}
animate={{ opacity: 1, y: 0 }}
transition={{ duration: 0.5, delay: index * 0.1 }}

// Clip-path reveal for titles
initial={{ clipPath: "polygon(0% 0%, 0% 0%, 0% 100%, 0% 100%)" }}
animate={{ clipPath: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)" }}

// Spring animations for numbers
transition={{ type: "spring", stiffness: 100, damping: 10 }}
```

### 9. Layout Architecture

```tsx
// User Layout Structure
<div className="container flex">
  <header className="sm:w-20 lg:w-64"> {/* Sidebar */}
    <SideMenu />
  </header>
  <main className="flex-1">
    {children}
  </main>
</div>

// Dashboard Layout (Parallel Routes)
<div className="flex gap-6">
  <div className="flex-grow-[2]">{main}</div>
  <div className="flex-grow sticky">{userProgress}</div>
</div>
```

### 10. Database Schema for Lessons

```sql
-- Units Table
units: {
  id, title, description, course_id, order
}

-- Lessons Table  
lessons: {
  id, title, unit_id, order
}

-- Challenges Table
challenges: {
  id, type (SELECT/ASSIST), question, lesson_id, order
}

-- Challenge Options
challenge_options: {
  id, challenge_id, option, correct, image_src, audio_src
}

-- User Progress
user_progress: {
  user_id, active_course_id, hearts, points, user_name, user_img_src
}

-- Challenge Progress
challenge_progress: {
  id, user_id, challenge_id, completed
}
```

### 11. Lesson Page Implementation

The actual lesson/quiz interface (`/app/lesson/page.tsx`) should include:

```tsx
// Core lesson components needed:
1. Question display
2. Multiple choice options
3. Progress bar
4. Hearts/lives counter
5. Exit confirmation dialog
6. Correct/incorrect feedback
7. Audio playback for pronunciations
8. Image display for visual questions
```

### 12. Component File Structure

```
/components/
├── ui/                    # Base primitives
│   ├── button.tsx        # 12+ variants
│   ├── dialog.tsx        # Modal system
│   ├── drawer.tsx        # Mobile panels
│   ├── popover.tsx       # Desktop tooltips
│   └── adaptive-modal.tsx # Responsive modal
├── theme/
│   ├── provider.tsx      # Theme context
│   └── toggle.tsx        # Dark/light switch
├── motion/               # Animations
│   ├── AnimatedTitle.tsx
│   ├── AnimatedList.tsx
│   └── AnimatedNumber.tsx
└── user/
    ├── learn/           # Learning interface
    │   └── LearnButton/
    ├── courses/         # Course selection
    └── UserProgress.tsx # Progress sidebar
```

### 13. Key Implementation Details

**SVG Handling**: Custom webpack config for importing SVGs as components
```js
// next.config.mjs
webpack(config) {
  config.module.rules.push({
    test: /\.svg$/i,
    use: ['@svgr/webpack']
  })
}
```

**Font System**: 
- Sans: Gabarito
- Display: Capriola
- Loaded via Next.js font optimization

**Depth Effects**: Border-bottom technique
```css
.button-3d {
  border-bottom-width: 4px;
  active:border-bottom-width: 2px;
}
```

**Mobile-First Responsive**:
- Base: Mobile styles
- sm: 640px+ 
- md: 768px+
- lg: 1024px+

### 14. Server Actions Pattern

```tsx
// actions/selectCourse.ts
'use server'

export async function selectCourse(courseId: number) {
  const { userId } = await auth();
  // Update user progress
  // Revalidate cache
  // Redirect to /learn
}
```

### 15. Critical UI Patterns to Replicate

1. **3D Button Effect**: Use border-bottom-4 with darker shade
2. **Zigzag Layout**: Implement cyclic indentation (0,1,2,1,0...)  
3. **Progress Rings**: Circular SVG or gauge library
4. **Adaptive Modals**: Popover on desktop, drawer on mobile
5. **Color Cycling**: Rotate through color variants per unit
6. **Entrance Animations**: Staggered fade-in with delays
7. **Responsive Sidebar**: Collapsed on mobile, expanded on desktop
8. **Card Hover States**: Lift effect with shadow and brightness
9. **Loading States**: Skeleton screens with shimmer
10. **Error Boundaries**: Graceful error handling with fallbacks

## To Implement the Lesson/Quiz Interface:

1. Create a quiz component with question display
2. Add multiple choice option cards
3. Implement progress tracking bar
4. Add hearts/lives system
5. Create feedback modals (correct/incorrect)
6. Add sound effects and audio playback
7. Implement streak counting
8. Add XP/points calculation
9. Create completion celebration screen
10. Handle navigation between questions

This documentation provides a complete blueprint for replicating the Duolingo-style UI in any React application.