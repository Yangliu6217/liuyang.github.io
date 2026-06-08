# MoodCash — Product Design Document for AI Design Tools

> This document describes MoodCash, a mood-based expense tracking WeChat Mini Program. It is intended for AI design tools (Stitch, Figma AI, etc.) to fully understand the product's architecture, design system, and user experience.

---

## Product Overview

**MoodCash** is a WeChat Mini Program that combines expense tracking with emotional awareness. Unlike traditional finance apps that treat spending as pure data, MoodCash asks users to record how they *feel* when they spend money. The core insight: understanding your emotional spending patterns leads to better financial habits.

**Platform:** WeChat Mini Program (compiled from Vue 3 + Pinia via uni-app)
**Current Version:** v2.2.0
**Primary Market:** China (WeChat ecosystem)
**Language:** Simplified Chinese

---

## Target Users

**Primary:** Young Chinese professionals (ages 20-35) who:
- Want to understand *why* they spend, not just *what* they spend
- Feel that traditional budgeting apps are cold and mechanical
- Use WeChat daily and prefer lightweight tools over standalone apps
- Value emotional self-awareness and personal growth

**Secondary:** Students tracking daily expenses with emotional context

**User Mindset:** "I want to see my spending story, not just a spreadsheet."

---

## Design Goals

1. **Emotional, not clinical** — Every interaction should feel warm, not like filling out a tax form
2. **Instantly understandable** — Zero learning curve; anyone can record a bill in 10 seconds
3. **Visually premium** — Must feel like a polished native app, not a student project
4. **Delightful micro-interactions** — Subtle animations that reward engagement without slowing users down
5. **Data with soul** — Charts and stats should tell emotional stories, not just show numbers

---

## Brand Personality

| Trait | Expression |
|-------|-----------|
| **Warm** | Soft purples, rounded corners, emoji-driven mood expression |
| **Playful** | Bouncy animations, mood emojis, delightful empty states |
| **Honest** | Clean data presentation, no dark patterns, transparent budgets |
| **Mindful** | Encourages reflection, not guilt; celebrates awareness |
| **Premium** | Multi-layer shadows, smooth transitions, consistent spacing |

**Brand Voice:** A supportive friend who's good with numbers — never judgmental, always encouraging.

---

## Information Architecture

```
MoodCash
├── 首页 (Home) — Today's mood + spending overview
│   ├── MoodOverviewCard (hero: mood emoji + today's total)
│   ├── Today's Timeline (list of today's bills)
│   ├── FAB → 记一笔 (quick add)
│   └── Update Popup (version announcements)
│
├── 时间轴 (Timeline) — Full bill history
│   ├── Search bar
│   ├── Category filter chips
│   ├── Grouped bill list (by date)
│   └── Empty state
│
├── 统计 (Stats) — Monthly analytics
│   ├── StatsOverview (hero: month total + trend)
│   ├── Month picker
│   ├── CategoryRankList (top spending categories)
│   ├── MoodAnalysisCard (mood vs. spending correlation)
│   ├── MoodCalendar (mood heatmap)
│   └── TimeSlotChart (spending by time of day)
│
├── 我的 (Profile) — Settings & personal stats
│   ├── ProfileHero (avatar + lifetime stats)
│   ├── Theme picker
│   ├── Budget management link
│   ├── Calendar link
│   ├── Monthly report link
│   └── Delete data
│
├── 记账 (Record) — Add/edit a bill [modal page]
│   ├── Amount display
│   ├── Category grid (9 categories)
│   ├── Mood selector (5 moods)
│   ├── Note input
│   ├── Photo upload (max 9 images)
│   ├── Time picker
│   └── Number pad
│
├── 详情 (Detail) — Single bill view [modal page]
│   ├── DetailHero (mood + amount)
│   ├── Bill info card
│   ├── Photo gallery
│   └── Edit / Delete actions
│
├── 预算 (Budget) — Monthly budget management
│   ├── BudgetHero (budget amount + progress)
│   ├── Preset budget options
│   ├── Custom budget input
│   └── Budget info card
│
├── 日历 (Calendar) — Calendar view of spending
│   ├── Month navigation
│   ├── Mood calendar grid
│   ├── Day detail
│   └── Monthly summary card
│
└── 月报 (Report) — Monthly deep dive
    ├── Dark-themed report card
    ├── Top categories
    ├── Mood insights
    └── Share actions
```

---

## Page Structure

Every page follows a consistent layout pattern:

```
┌─────────────────────────┐
│       NavBar (fixed)     │  44px, glass background
├─────────────────────────┤
│                          │
│   [Hero Card]            │  Gradient, mc-rise animation
│                          │
│   [Content Section 1]    │  White cards, fadeInUp animation
│   [Content Section 2]    │  Staggered list items
│   [Content Section 3]    │
│                          │
│   ┌─────────────────┐   │
│   │   Bottom Spacer  │   │  180-240rpx
│   └─────────────────┘   │
├─────────────────────────┤
│      TabBar (fixed)      │  4 tabs, SVG icons
└─────────────────────────┘
```

**Tab pages:** Home, Timeline, Stats, Profile
**Modal pages:** Record, Detail, Budget, Calendar, Report

---

## User Flow

### Core Flow: Record a Bill
```
Tap FAB "+" button
  → Enter Record page (slide up)
    → Enter amount (number pad)
    → Select category (emoji grid)
    → Select mood (emoji grid)
    → Optional: add note, photos, adjust time
    → Tap confirm
      → Return to previous page
        → Bill appears in timeline with fade-in animation
```

### View & Manage Flow
```
Home → Tap bill → Detail page
  → View bill info + photos
  → Edit → Record page (pre-filled)
  → Delete → Confirm modal → Bill removed

Timeline → Search/Filter → Find bill → Detail page
Stats → Browse analytics → Tap category → Filtered view
Profile → Budget → Set monthly budget
Profile → Calendar → Browse by date
Profile → Report → Monthly deep dive
```

---

## Current Problems

### What Makes It Look Like a Student Project

1. **TabBar used emoji instead of icons** — FIXED in v2.2.0 (now SVG)
2. **All hero cards were identical purple** — Partially addressed
3. **No page entry animations** — FIXED in v2.2.0 (mc-rise + stagger)
4. **TimelineItem was flat and undifferentiated** — FIXED in v2.2.0
5. **Section title colors were inconsistent** (orange, yellow, purple randomly)
6. **Delete confirmation uses system modal** — looks generic
7. **Empty states are mechanical** — emoji + text, no personality
8. **No number animation** on amount changes
9. **Detail page info is flat** — all fields at same visual weight
10. **Progress bars lack entrance animation**

---

## UI Upgrade Goals

| Priority | Goal | Status |
|----------|------|--------|
| 1 | TabBar with real SVG icons + bounce animation | ✅ Done |
| 2 | Unified page entry animations | ✅ Done |
| 3 | TimelineItem card redesign | ✅ Done |
| 4 | Consistent section title accent colors | 🔲 TODO |
| 5 | Number scroll animation on amounts | 🔲 TODO |
| 6 | Custom delete confirmation (brand-styled) | 🔲 TODO |
| 7 | Richer empty states with illustration | 🔲 TODO |
| 8 | Detail page hierarchy (amount dominant) | 🔲 TODO |
| 9 | Progress bar entrance animations | 🔲 TODO |
| 10 | Mood-responsive color tints on timeline cards | 🔲 TODO |

---

## Interaction Upgrade Goals

| Goal | Description |
|------|-------------|
| Haptic feedback | Light vibration on key actions (add, delete, switch tab) |
| Number scroll | Amount digits roll like a counter when values change |
| Pull-to-refresh | Smooth elastic overscroll with brand-colored indicator |
| Swipe gestures | Swipe left on timeline item to reveal quick actions |
| Long press preview | Long press on category/mood to see full-screen preview |
| Skeleton loading | Shimmer placeholders while data loads |
| Page transitions | Shared element transitions between list → detail |

---

## Visual Keywords

**Primary:** Soft, Warm, Rounded, Playful, Premium
**Secondary:** Clean, Spacious, Consistent, Delightful, Mindful
**Avoid:** Cold, Clinical, Crowded, Generic, Mechanical

---

## Reference Apps

| App | What to Learn |
|-----|--------------|
| **Daylio** | Mood tracking UX, emotional color coding, journal feel |
| **Apple Journal** | Premium card design, smooth transitions, warm tone |
| **WeChat Pay** | Bill list layout, amount hierarchy, clean data display |
| **Linear** | Card shadows, micro-interactions, keyboard shortcuts |
| **Notion** | Empty states, onboarding, block-based content |
| **Apple Music** | Hero cards, gradient usage, tab bar design |
| **支付宝** | Number animations, progress indicators, financial data |
| **小红书** | Card grid layout, image preview, social feel |

---

## Design System

### Spacing Scale (8rpx base grid)

| Token | Value | Usage |
|-------|-------|-------|
| `xs` | 8rpx | Tight gaps, icon margins |
| `sm` | 16rpx | Inner padding, small gaps |
| `md` | 24rpx | Card gaps, section spacing |
| `lg` | 32rpx | Card padding, page margins |
| `xl` | 48rpx | Large section breaks |

### Border Radius Scale

| Token | Value | Usage |
|-------|-------|-------|
| `sm` | 12rpx | Small elements, tags, badges |
| `md` | 16rpx | List items, buttons, inputs |
| `lg` | 24rpx | Cards, panels, modals |
| `xl` | 32rpx | Special cards, popups |
| `pill` | 50rpx | Full-round buttons, avatars |

### Shadow System

| Tier | Value | Usage |
|------|-------|-------|
| `sm` | `0 2rpx 8rpx rgba(0,0,0,.04), 0 8rpx 24rpx rgba(0,0,0,.06)` | Default cards |
| `md` | `0 4rpx 16rpx rgba(0,0,0,.08), 0 12rpx 32rpx rgba(0,0,0,.1)` | Hover/active state |
| `lg` | `0 16rpx 48rpx rgba(0,0,0,.14)` | Elevated modals |
| `brand` | `0 8rpx 32rpx rgba(108,99,255,.4)` | FAB, primary buttons |

---

## Motion Design

### Animation Principles

1. **Purposeful** — Every animation communicates state change
2. **Swift** — 200ms-400ms, never slow enough to wait
3. **Organic** — iOS-style easing, no linear or mechanical movement
4. **Layered** — Hero first, then content, then details (stagger)
5. **Consistent** — Same element type always animates the same way

### Animation Catalog

| Name | Effect | Duration | Easing | Usage |
|------|--------|----------|--------|-------|
| `mc-rise` | translateY(24rpx) + fade | 350ms | cubic-bezier(.22,1,.36,1) | Hero cards on page enter |
| `fadeInUp` | translateY(30rpx) + fade | 300ms | cubic-bezier(.34,1.56,.64,1) | Content sections |
| `fadeIn` | opacity 0→1 | 250ms | ease | List items, stagger base |
| `tabbar-bounce` | scale(.8→1.2→1) | 350ms | cubic-bezier(.34,1.56,.64,1) | Tab bar active icon |
| `cardIn` | scale(.9) + translateY(32rpx) + fade | 350ms | cubic-bezier(.22,1,.36,1) | Modal cards (popup) |
| `ti-in` | translateY(16rpx) + fade | 400ms | cubic-bezier(.22,1,.36,1) | TimelineItem entry |

### Stagger Pattern

- Base delay: 60ms between items
- Max items: 10 (cap at 600ms total)
- Applied via `:nth-child` CSS selectors
- Hero cards: 0ms delay (immediate)
- Content sections: 150ms delay after hero
- List items: 60ms × index

---

## Color System

### Brand Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `primary` | `#6C63FF` | Brand identity, buttons, active states, accents |
| `primary-light` | `#A29BFE` | Gradient endpoints, secondary accents |
| `primary-dark` | `#8B83FF` | Gradient midpoints |
| `accent` | `#FFB86C` | Warm highlights, category rank borders |

### Hero Gradient

```css
linear-gradient(135deg, #6C63FF 0%, #A29BFE 100%)
```

Used on: All hero cards, FAB, primary buttons, confirm actions.

### Text Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `text-primary` | `#1A1A2E` | Headings, important text |
| `text-secondary` | `#4A4A68` | Body text, descriptions |
| `text-tertiary` | `#9E9EB8` | Captions, labels |
| `text-disabled` | `#C8C8D8` | Placeholders, timestamps |
| `text-inverse` | `#FFFFFF` | Text on dark/gradient backgrounds |

### Background Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `bg-page` | `#F8F9FC` | Page background |
| `bg-card` | `#FFFFFF` | Card backgrounds |
| `bg-secondary` | `#F0F1F6` | Inactive elements, dividers |

### Semantic Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `danger` | `#FF6B6C` | Errors, overspend, delete actions |
| `success` | `#6BCB77` | Under-budget, positive trends |
| `warning` | `#FFB86C` | Approaching limits |

### Mood Colors

Each mood has an assigned color used for the timeline bar and emotional indicators:

| Mood | Emoji | Color |
|------|-------|-------|
| 开心 (Happy) | 😊 | `#6BCB77` |
| 平静 (Calm) | 😌 | `#6C63FF` |
| 焦虑 (Anxious) | 😰 | `#FFB86C` |
| 低落 (Down) | 😢 | `#A29BFE` |
| 愤怒 (Angry) | 😤 | `#FF6B6C` |

### Theme System

The app supports 6 themes, each defining a complete color set:

| Theme | Style | Hero Gradient |
|-------|-------|---------------|
| 经典紫 (Default) | Brand purple | `#6C63FF → #A29BFE` |
| 彩虹 (Rainbow) | Colorful | `#FF6B6B → #FFD93D → #6BCB77` |
| 黑白 (Monochrome) | Minimal | `#1A1A1A → #4A4A4A` |
| 深海 (Ocean) | Cool blue | `#0891B2 → #22D3EE` |
| 樱花 (Sakura) | Soft pink | `#EC4899 → #F472B6` |
| 森林 (Forest) | Natural green | `#059669 → #34D399` |

---

## Typography System

### Font Stack

```css
-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
"Hiragino Sans GB", "Microsoft YaHei", "Helvetica Neue",
Helvetica, Arial, sans-serif
```

### Type Scale

| Token | Size | Weight | Line-height | Usage |
|-------|------|--------|-------------|-------|
| `3xl` | 56rpx | 700 | 1.2 | Hero emojis, large mood indicators |
| `2xl` | 40rpx | 700 | 1.2 | Hero stat values |
| `xl` | 36rpx | 600 | 1.3 | Hero names, report titles |
| `lg` | 32rpx | 600 | 1.3 | Navbar titles, amount values |
| `md` | 28rpx | 500 | 1.5 | Body text, category names, section titles |
| `sm` | 24rpx | 400 | 1.5 | Descriptions, notes, weekday labels |
| `xs` | 22rpx | 400 | 1.4 | Captions, timestamps, tab labels |
| `2xs` | 20rpx | 400 | 1.4 | Trend indicators, version text |

### Amount Display

Large amounts (hero cards, detail page) use:
- Size: 64-80rpx
- Weight: 700
- Letter-spacing: -2rpx
- Symbol (¥): 40rpx, weight 600

---

## Component Library

### Layout Components

| Component | Description | Key Properties |
|-----------|-------------|----------------|
| **PageContainer** | Full-page flex column | 100vh, flex-direction: column |
| **NavBar** | Fixed top navigation | 44px height, glass bg, back arrow, title centered |
| **TabBar** | Fixed bottom tab bar | 4 tabs, SVG icons (line/fill), bounce animation |

### Business Components

| Component | Description | Animation |
|-----------|-------------|-----------|
| **MoodOverviewCard** | Home hero: mood emoji + today's total | mc-rise 350ms + emoji bounceIn |
| **MonthSummaryCard** | Stats hero: month total + mood | mc-rise 350ms |
| **TimelineItem** | Single bill card in timeline | fadeInUp 400ms with stagger |
| **TimelineDivider** | Date separator in timeline | fadeInUp 300ms |
| **CategoryGrid** | 3×3 emoji grid for category selection | scale bounce on select |
| **MoodSelector** | 5-mood emoji row | scale bounce on select |
| **NumberPad** | Calculator-style input | scale .95 on press |
| **AmountDisplay** | Large formatted amount | Static (TODO: number scroll) |
| **CategoryRankList** | Ranked list with progress bars | fadeIn with stagger |
| **MoodAnalysisCard** | Mood vs. spending correlation | fadeIn with stagger |
| **MoodCalendar** | Calendar grid with mood dots | fadeIn 300ms |
| **TimeSlotChart** | Bar chart by time of day | fadeIn 300ms |
| **BudgetHero** | Budget progress with gradient | mc-rise 350ms |
| **ProfileHero** | Avatar + lifetime stats | mc-rise 350ms |
| **UpdatePopup** | Version announcement modal | cardIn 350ms |
| **MoodFabButton** | Central action button | pulse 2s × 3 |

### Card Patterns

**Pattern A — White Elevated Card**
```css
background: #FFFFFF;
border-radius: 24rpx;
padding: 32rpx;
box-shadow: 0 2rpx 8rpx rgba(0,0,0,.04), 0 8rpx 24rpx rgba(0,0,0,.06);
```

**Pattern B — Gradient Hero Card**
```css
border-radius: 24rpx;
overflow: hidden;
/* Inner content: padding 36-48rpx, color #fff */
/* Stat sections: bg rgba(255,255,255,.12-.15), radius 16rpx */
/* Dividers: 1rpx wide, rgba(255,255,255,.3) */
```

**Pattern C — Dark Themed Card (Report only)**
```css
background: linear-gradient(160deg, #1A1A3E, #2D2B5E 40%, #4A4478);
/* Inner elements: bg rgba(255,255,255,.08) */
```

**Pattern D — List Item Card**
```css
background: #FFFFFF;
border-radius: 20rpx;
padding: 28rpx 32rpx;
box-shadow: (Pattern A shadow);
/* Left accent: 10rpx mood-colored bar */
```

### Section Title Pattern

```css
font-size: 28rpx;
font-weight: 600;
color: #1A1A2E;
padding-left: 20rpx;
border-left: 6rpx solid var(--mc-primary, #6C63FF);
margin-bottom: 20-28rpx;
```

### Glass Effect

```css
/* NavBar */
background: rgba(248, 249, 252, .95);
border-bottom: 1rpx solid rgba(0,0,0,.04);

/* TabBar */
background: rgba(255, 255, 255, .95);
border-top: 1rpx solid rgba(0,0,0,.04);
```

---

## Data Model

### Bill Object

```javascript
{
  id: string,           // Unique identifier
  amount: number,       // Expense amount
  category: string,     // Category key (food, shopping, etc.)
  categoryName: string, // Display name
  categoryIcon: string, // Emoji icon
  mood: string,         // Mood key (happy, calm, anxious, down, angry)
  note: string,         // Optional text note
  images: string[],     // Array of file paths (max 9)
  date: string,         // "YYYY-MM-DD"
  time: string,         // "HH:mm"
  displayTime: string,  // "14:30"
  formattedAmount: string // "128.50"
}
```

### Categories (9 total)

| Key | Icon | Name |
|-----|------|------|
| food | 🍜 | 餐饮 |
| shopping | 🛍️ | 购物 |
| transport | 🚗 | 交通 |
| entertainment | 🎮 | 娱乐 |
| study | 📚 | 学习 |
| medical | 💊 | 医疗 |
| drink | 🧋 | 饮品 |
| housing | 🏠 | 居住 |
| other | 📦 | 其他 |

### Moods (5 total)

| Key | Emoji | Name | Color |
|-----|-------|------|-------|
| happy | 😊 | 开心 | #6BCB77 |
| calm | 😌 | 平静 | #6C63FF |
| anxious | 😰 | 焦虑 | #FFB86C |
| down | 😢 | 低落 | #A29BFE |
| angry | 😤 | 愤怒 | #FF6B6C |

---

## CSS Variable System

All theme-able values use CSS custom properties with hardcoded fallbacks:

```css
--mc-primary:       #6C63FF
--mc-accent:        #FFB86C
--mc-bg-card:       #FFFFFF
--mc-bg-secondary:  #F0F1F6
--mc-hero-gradient: linear-gradient(135deg, #6C63FF 0%, #A29BFE 100%)
--mc-progress-gradient: linear-gradient(900deg, #6C63FF, #A29BFE)
--mc-navbar-bg:     #F8F9FC
--mc-tabbar-bg:     rgba(255, 255, 255, .95)
```

---

*Document generated from MoodCash v2.2.0 codebase analysis.*
*Intended for AI design tools to understand product context before generating UI proposals.*
