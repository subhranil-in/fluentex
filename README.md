# 🏆 Fluentex — AI-Powered English Learning Platform
### Version 1.6.0 BETA

> *Learn English the smart way — adaptive AI quizzes, real-time flashcards, global leaderboards, and streak tracking. All in a single HTML file.*

---

## 📌 Overview

**Fluentex** is a fully self-contained, browser-based English learning platform built in the spirit of Duolingo. It uses the **Anthropic Claude AI API** to generate adaptive flashcards, quizzes, and daily vocabulary — all personalised to your CEFR level and performance history.

No installation. No server. No sign-up. Just open the HTML file in any modern browser and start learning.

---

## ✨ Features

### 🤖 AI-Powered Learning
- **Adaptive Flashcards** — 5 AI-generated flashcards per session, tailored to your chosen CEFR level and category
- **Adaptive Quiz Engine** — 6 questions per quiz; the AI analyses your weak areas and biases 60% of questions toward them
- **Word of the Day** — A fresh vocabulary word generated daily by AI (cached by IST timezone, refreshes at midnight)
- **Fallback Content** — Full offline-capable fallback with 5 flashcards and 6 quiz questions per category if the API is unavailable

### 📊 Progress & Performance
- **6 CEFR Levels** — A1 Starter · A2 Elementary · B1 Intermediate · B2 Upper-Intermediate · C1 Advanced · C2 Expert
- **6 Categories** — Vocabulary · Grammar · Idioms & Phrases · Spelling · Conversation · Reading & Inference
- **Per-Category Accuracy Dashboard** — Colour-coded performance pills (green ≥75%, yellow ≥50%, red <50%)
- **XP System** — Earn 15–25 XP per correct answer; level up every 100 XP
- **Streak Tracking** — Daily streaks tracked in IST timezone; streak bonus (+20 XP) for scoring ≥80%
- **Hearts System** — 5 hearts per quiz; lose one per wrong answer

### 🏅 Badges (16 Total)
| Badge | Requirement |
|---|---|
| 🌟 First Step | Complete 1 quiz |
| 🔥 On Fire | 3-day streak |
| 🏅 Week Warrior | 7-day streak |
| 💎 Diamond Habit | 30-day streak |
| ⚡ Spark | Earn 100 XP |
| 🏆 Champion | Earn 500 XP |
| 👑 Master | Earn 1,000 XP |
| 🌌 Legend | Earn 5,000 XP |
| 📚 Bookworm | Complete 5 quizzes |
| 🎓 Scholar | Complete 20 quizzes |
| 🏛️ Professor | Complete 50 quizzes |
| 🎯 Sharpshooter | 100% accuracy on a quiz |
| 💨 Speed Demon | Complete a quiz in under 60 seconds |
| 📖 Word Wizard | 50 vocabulary questions correct |
| ✏️ Grammar Guru | 50 grammar questions correct |
| 🌈 Renaissance | Play all 6 categories |

### 🌐 Global Leaderboard
- **Real-time cross-session sync** via `window.storage` shared API
- **Cross-tab live updates** via the browser `BroadcastChannel` API
- Parallel batch-fetching of up to 80 remote entries
- Automatically merges remote and local data
- Polls for updates every 30 seconds
- Gracefully falls back to localStorage if shared storage is unavailable

### 🎨 UI & Experience
- **Dark / Light theme** toggle via the sidebar (persisted across sessions)
- **Duolingo-style typography** — Nunito font (900 weight for headings, 700–800 for body)
- **Smooth animations** — spring curves, card slide-ins, confetti on good results, XP pop-ups, ripple effects
- **Particle background** — twinkling stars + floating nodes with connecting lines (adapts to theme)
- **3D flip flashcards** — perspective card flip with front/back faces
- **Sidebar navigation** — hamburger menu with stats, theme toggle, nav links, and legal links
- **Fully responsive** — `clamp()`-based fluid layout from 360px phones to 1600px+ monitors; `viewport-fit=cover` for iPhone notch support

---

## 🗂️ App Screens

| Screen | Description |
|---|---|
| **Splash** | Animated logo with loading bar, auto-advances after 2.4s |
| **Home** | Greeting, hero CTA, Word of the Day, XP progress, performance dashboard, level grid, global leaderboard |
| **Level Select** | 6 CEFR level cards with colour-coded borders and hover effects |
| **Category Select** | 6 category cards showing per-category accuracy badges |
| **Training** | 3D flip flashcard mode with confidence rating (Got it / Learning / Tricky) |
| **Quiz** | Adaptive quiz with 3 question types; progress bar and hearts |
| **Results** | Trophy, accuracy, XP earned, time taken, streak bonus, confetti |
| **Badges** | Full badge showcase — earned (gold glow) vs locked (greyscale) |

---

## 🧩 Question Types

1. **Multiple Choice (MCQ)** — 4 options, keyboard shortcut keys 1–4 supported
2. **Fill in the Blank** — Free-text input, case-insensitive, supports `/`-separated alternative answers
3. **Word Arrange** — Tap words from a bank to build a sentence; tap placed words to remove them

---

## 💾 Data Storage

All user data is stored **locally in the browser** (localStorage + sessionStorage dual-save for resilience):

| Key | Contents |
|---|---|
| `fx_pro_v6` | Full user profile (name, XP, streak, badges, perf stats, hearts) |
| `fx_lb_v6` | Local leaderboard cache (up to 100 entries, sorted by XP) |
| `fx_wod_v5` | Word of the Day cache (keyed by IST date) |
| `fx_theme_v5` | Theme preference (dark / light) |
| `fluentex_lb:{name}` | Global leaderboard entry (shared storage, public) |

### 🔄 Save Migration
Fluentex automatically migrates data from all previous storage key versions:
- `fx_pro_v5` → `fluentex_pro_v4` → `fluentex_pro_v3` → `fluentex_profile_v2`

Your progress is **never lost on updates**.

---

## 🤖 AI Integration

- **Model:** `claude-sonnet-4-20250514`
- **Endpoint:** `https://api.anthropic.com/v1/messages`
- **API Key:** Handled externally (injected at runtime by the hosting environment)

### AI Calls Made Per Session
| Call | Purpose | Fallback |
|---|---|---|
| `fetchFlashcards` | 5 CEFR-aware flashcards per category | `FB` object (5 cards × 6 categories) |
| `beginQuiz` | 6 adaptive questions with performance context | `FBQ` object (6 questions × 6 categories) |
| `loadWoD` | Daily vocabulary word (cached) | `WODFB` array (7 fallback words) |

### Performance Context Injection
Before generating a quiz, Fluentex builds a `perfCtx()` string summarising the user's accuracy per category and instructs the AI to bias 60% of questions toward weak areas.

---

## 📁 File Structure

Fluentex is delivered as a **single self-contained HTML file**:

```
fluentex.html          ← Everything: HTML + CSS + JavaScript (~140 KB)
README.md              ← This file
```

No build step. No dependencies to install. No CDN required beyond Google Fonts.

**External resources loaded at runtime:**
- `fonts.googleapis.com` — Nunito font family
- `flagcdn.com` — Country flag images (with emoji fallback)
- `api.anthropic.com` — Claude AI API for content generation

---

## 🚀 Getting Started

### Option 1 — Open Directly
Simply double-click `fluentex.html` in any modern browser. No server needed.

### Option 2 — Serve Locally
```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .
```
Then open `http://localhost:8080/fluentex.html`

### Option 3 — Deploy to Web
Upload `fluentex.html` to any static hosting:
- GitHub Pages
- Netlify (drag & drop)
- Vercel
- Cloudflare Pages
- Amazon S3 + CloudFront

---

## 🌐 Browser Compatibility

| Browser | Support |
|---|---|
| Chrome 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| Edge 90+ | ✅ Full |
| Mobile Chrome | ✅ Full |
| Mobile Safari | ✅ Full (viewport-fit=cover) |
| IE 11 | ❌ Not supported |

**Required browser features:** CSS Custom Properties, CSS Grid, `backdrop-filter`, `clamp()`, `BroadcastChannel`, `fetch`, `async/await`

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `1` `2` `3` `4` | Select MCQ option A/B/C/D |
| `Enter` | Submit answer (when enabled) |

---

## 📱 Responsive Breakpoints

| Breakpoint | Behaviour |
|---|---|
| `≥ 1600px` | Max container width 1100px; 3-column level/category grids |
| `1200px–1600px` | 3-column grids; standard layout |
| `768px–1200px` | 2-column grids; full nav |
| `480px–768px` | 1-column grids; hero paragraph hidden on small screens |
| `≤ 480px` | Streak display hidden; HP stat hidden; beta tag hidden |
| `≤ 360px` | Fire streak stat hidden; minimal nav |

---

## 🏗️ Architecture

### State Objects
```javascript
U = {
  name, flag, xp, streak, hearts, maxHearts,
  lastDay, quizzes, perfectQuizzes, fastQuizzes,
  badges[], catsPlayed{},
  perf: {
    vocabulary: {c, w},  // correct, wrong
    grammar:    {c, w},
    idioms:     {c, w},
    spelling:   {c, w},
    conversation:{c, w},
    reading:    {c, w}
  }
}

tr = { cards[], idx, flipped, done[] }           // Training state
qz = { questions[], idx, correct, wrong, t0,     // Quiz state
       answered, mcqSel, arranged[], arrAns, label }
```

### Key Functions
| Function | Purpose |
|---|---|
| `init()` | Bootstrap: theme, flags, canvas, profile load, routing |
| `loadProfileWithMigration()` | Load profile with legacy key fallback |
| `applyProfile(p)` | Merge profile into state with default filling |
| `checkStreak()` | Compare IST dates and increment/reset streak |
| `beginQuiz()` | Fetch AI quiz with performance context |
| `pickCat(id)` | Fetch AI flashcards for selected level + category |
| `loadWoD()` | Load or fetch Word of the Day (IST date cache) |
| `fetchGlobalLB()` | Fetch + merge global + local leaderboard |
| `pushLBEntry()` | Push score to localStorage + BroadcastChannel + shared storage |
| `awardBadges()` | Check all 16 badge conditions and award with toast |
| `perfCtx()` | Build AI performance context string |
| `renderPerf()` | Render per-category accuracy dashboard |
| `toggleTheme()` | Switch dark/light theme + persist |
| `openSidebar()` / `closeSidebar()` | Sidebar open/close with overlay |
| `openLegal(which)` | Open Terms of Use or Privacy Policy modal |
| `doConfetti()` | Full-screen confetti canvas animation |
| `initBg()` | Stars + particle nodes background canvas |

---

## 📜 Legal

- **Terms of Use** — Accessible in every page footer and the sidebar
- **Privacy Policy** — Accessible in every page footer and the sidebar
- **Data** — No external tracking, no cookies, no analytics. All data stays in your browser except leaderboard entries which are stored in shared storage.

---

## 👨‍💻 Credits

| Role | Credit |
|---|---|
| **Developer** | SubhraniL |
| **Publisher** | Classify Technologies LLC |
| **AI Engine** | Anthropic Claude (claude-sonnet-4) |
| **Font** | Nunito — Google Fonts |
| **Flags** | flagcdn.com |

---

## 📄 License

© 2026 Classify Technologies LLC. All Rights Reserved.

This software is proprietary. Unauthorised reproduction, distribution, or modification of any part of this software is strictly prohibited without written permission from Classify Technologies LLC.

---

*Made with ❤️ in India · Fluentex v1.7.0 BETA*
