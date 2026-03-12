# 🏆 Fluentex — AI-Powered English Learning Platform

> **Version:** v1.8.0 BETA &nbsp;|&nbsp; **Built by:** SubhraniL &nbsp;|&nbsp; **© 2026 Classify Technologies LLC**

---

## Table of Contents

1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Features](#features)
   - [AI Flashcard Training](#ai-flashcard-training)
   - [Adaptive Quiz Engine](#adaptive-quiz-engine)
   - [CEFR Levels & Categories](#cefr-levels--categories)
   - [Progress & XP System](#progress--xp-system)
   - [Badge System](#badge-system)
   - [Global Leaderboard](#global-leaderboard)
   - [Word of the Day](#word-of-the-day)
   - [Community Chat](#community-chat)
   - [Push Notifications](#push-notifications)
   - [Feedback System](#feedback-system)
4. [Architecture](#architecture)
5. [Storage Keys Reference](#storage-keys-reference)
6. [BroadcastChannels Reference](#broadcastchannels-reference)
7. [Technology Stack](#technology-stack)
8. [Deployment](#deployment)
9. [Known Limitations](#known-limitations)

---

## Overview

Fluentex is a fully self-contained, single-HTML-file English learning platform powered by the Anthropic Claude API. It requires no backend server, no database, and no build step — open the file in any modern browser and it works immediately.

Learners progress through CEFR-aligned levels (A1 → C2), study AI-generated flashcards, take adaptive quizzes that adjust to their strengths and weaknesses, track streaks and XP, earn badges, compete on a global leaderboard, and interact with the broader Fluentex community in real time.

---

## Getting Started

1. Open `fluentex.html` in any modern browser (Chrome, Firefox, Edge, Safari).
2. On the splash screen, enter your display name and choose your country flag.
3. You are taken to the home screen. Your profile is saved automatically.
4. Tap **⚡ Start Learning** to choose a CEFR level and topic, study flashcards, then take the quiz.

No account, login, or internet connection is required for core learning features. An internet connection is needed only for AI-generated content (flashcards, quizzes, Word of the Day) and the Community Chat.

---

## Features

### AI Flashcard Training

Before each quiz, Fluentex generates a set of 5 AI flashcards tailored to the selected CEFR level and category. Each card presents a target word or phrase with its definition, example sentence, and part of speech — all generated on demand by `claude-sonnet-4-20250514`.

Cards are navigated with swipe-style Next/Previous controls. Progress dots at the top show how many cards remain.

---

### Adaptive Quiz Engine

Each quiz session contains **10 questions** generated live by the AI based on:

- The selected CEFR level (A1–C2)
- The selected topic category
- The learner's historical accuracy per category (stored in `U.perf`)

Question types include multiple choice (4 options), fill-in-the-blank, and sentence correction. The AI is instructed to focus on the learner's weakest sub-topics, making each session progressively more targeted.

After each question the learner receives instant feedback — correct/incorrect, with an explanation. At the end, a results screen shows:

| Stat | Description |
|------|-------------|
| Score | Questions correct out of 10 |
| XP Earned | Based on score and accuracy |
| Streak | Current daily streak |
| Accuracy | Running accuracy per category |

Bonus XP is awarded for perfect scores (100%) and fast completions.

---

### CEFR Levels & Categories

**6 CEFR Levels**

| Code | Name | Description |
|------|------|-------------|
| A1 | Starter | Basic words, greetings & simple phrases |
| A2 | Elementary | Everyday words and simple exchanges |
| B1 | Intermediate | Can handle most situations |
| B2 | Upper-Intermediate | Understands complex text with real fluency |
| C1 | Advanced | Effective, flexible language use |
| C2 | Expert | Near-native mastery |

**6 Topic Categories**

| Category | Focus |
|----------|-------|
| 📖 Vocabulary | Word meanings, synonyms & usage |
| ✍️ Grammar | Tenses, articles, sentence structure |
| 💬 Idioms & Phrases | Native expressions & phrasal verbs |
| 🔤 Spelling | Master tricky English spellings |
| 🗣️ Conversation | Social phrases & real-world dialogue |
| 📚 Reading | Comprehension & text analysis |

---

### Progress & XP System

**XP (Experience Points)** are earned by completing quizzes. Bonus XP is given for:
- Perfect scores (+20 XP bonus)
- Fast completions (+10 XP bonus)

**Levels** are based on cumulative XP with increasing thresholds:

| Rank | Description |
|------|-------------|
| Beginner | Starting rank |
| Learner | First milestone |
| Explorer | Mid-tier progress |
| Scholar | Dedicated learner |
| Master | Advanced stage |
| Legend | Near top tier |
| Grandmaster | Highest rank |

**Hearts** represent lives (max 5). A wrong answer costs one heart. Hearts regenerate over time.

**Daily Streak** tracks consecutive days of activity. Missing a day resets the streak to zero.

**Weekly XP** resets every Monday at midnight and drives the weekly leaderboard rankings.

---

### Badge System

Fluentex has **33 achievement badges** across 7 categories:

**🔥 Streak Badges**
`Getting Started` · `Week Warrior` · `Fortnight Fighter` · `Diamond Habit` · `Iron Will` · `Century Champion`

**⚡ XP Badges**
`Spark` · `Dynamo` · `Champion` · `Master` · `Legend` · `Grandmaster`

**📚 Quiz Count Badges**
`First Step` · `Bookworm` · `Studious` · `Scholar` · `Professor` · `Genius`

**🎯 Accuracy Badges**
`Sharpshooter` · `Marksman` · `Sniper`

**⚡ Speed Badges**
`Speed Demon` · `Hyperdrive`

**📖 Topic Mastery Badges**
`Word Wizard` · `Lexicon Master` · `Grammar Guru` · `Syntax Sage` · `Phrase Whisperer` · `Deep Reader`

**🌟 Special Badges**
`Renaissance` · `Polyglot` · `Comeback Kid` · `Crowd Pleaser`

New badge awards trigger a full-screen confetti celebration with a toast notification.

---

### Global Leaderboard

The home screen shows two live leaderboard views:

- **Weekly Top 10** — ranked by XP earned since the current Monday
- **All-Time ranking** — shows the learner's personal all-time position

The leaderboard auto-refreshes every **30 seconds** via `setInterval`.

**Simulated global activity** — 20 international bot learners are seeded into the leaderboard to simulate a global community. They gain XP organically every 45 seconds to 4 minutes, keeping the board active even with a single real user. Bots are indistinguishable from real users by design. Their names and flags represent learners from India, China, Japan, South Korea, Germany, Brazil, UK, Egypt, UAE, Vietnam, Mexico, Philippines, Indonesia, Senegal, and Nigeria.

---

### Word of the Day

Every session, a new English word is fetched from the AI (cached in localStorage for 24 hours). The Word of the Day card on the home screen shows:
- The word
- Part of speech
- Full definition
- Example sentence in context

---

### Community Chat

A full real-time global chat room accessible from the home screen or sidebar. Features:

**Chat Feed (Global Chat tab)**
- Messages rendered newest-at-bottom with coloured avatar initials
- Date dividers group messages by day
- Each message supports **👍 Like**, **👎 Dislike**, **↩ Reply**, and **👤 View Profile**
- Clicking a reply quote scrolls to the original message
- Own messages are highlighted with a blue border

**Members Panel (Members tab)**
- Lists every user who has chatted
- Inline **Follow / Unfollow** buttons per user
- XP and message count shown per member

**Profile Modal**
- Click any name or avatar to open a profile card
- Shows XP, streak, message count, and Follow/Unfollow button

**Follow System**
- Follow or unfollow any user
- Following state persisted in `fx_follows_v1` (localStorage)

**🛡️ FluentexMod (Automatic Moderator)**
- Posts a daily welcome message setting community guidelines
- Every message is scanned for phone numbers using the regex `(\+?\d[\d\s\-\.()]{7,}\d)`
- Phone numbers trigger automatic removal and a public moderator warning tagging the user

**Bot Activity**
- The 20 leaderboard bots also participate in chat every 20–60 seconds
- 70% of the time they post original learning tips and encouragement
- 30% of the time they reply to recent real user messages with reactions
- Bots appear as regular community members with no distinguishing label

**Real-time sync** via `BroadcastChannel('fluentex_chat')` with 4-second localStorage polling as fallback.

---

### Push Notifications

Admins can push notifications to individual users or all users from the Admin Panel. Notifications are delivered via:

1. **BroadcastChannel** — instant delivery if user and admin share the same browser
2. **localStorage polling** — 30-second polling for cross-session delivery

Users see a **🔔 bell icon** in the navigation bar with a red badge showing the unread count. The bell shakes when new notifications arrive.

The notification panel supports:
- All / Unread tabs
- Per-notification read/delete
- Mark all as read
- Clear all
- Brief slide-in toast for each new notification

---

### Feedback System

Two feedback options available from footer links on all screens:

- **💡 Suggest a Feature** — submit a category (UI/UX, AI/Content, Performance, New Feature, Other) and message
- **🐛 Report Bug** — submit a bug report with severity level (Low / Medium / High / Critical) and description

All submissions are stored locally in `fx_suggests_local` and `fx_bugs_local` and broadcast to the Admin Panel in real time via `BroadcastChannel('fluentex_lb')`.

---

## Architecture

Fluentex is a **zero-dependency single-file SPA** (Single Page Application).

```
fluentex.html
├── <head>         Google Fonts (Nunito), inline CSS (~600 lines)
├── <body>
│   ├── Sidebar overlay + Sidebar drawer
│   ├── Screens (display:none / .active)
│   │   ├── #home               — Dashboard, leaderboard, WoD
│   │   ├── #community          — Global chat room
│   │   ├── #level-screen       — CEFR level picker
│   │   ├── #category-screen    — Topic category picker
│   │   ├── #training-screen    — AI Flashcard training
│   │   ├── #quiz-screen        — Adaptive quiz
│   │   ├── #results-screen     — Post-quiz results
│   │   └── #badges             — Badge gallery
│   ├── Modals (name, TOS, privacy, suggestions, bugs, profile, legal)
│   ├── Notification tray (#notif-tray)
│   ├── Canvas layers (background particles + confetti)
│   └── <script>  ~3,000 lines of application logic
└── Notification script block (BroadcastChannel + polling)
```

**Screen navigation** is handled by `showScreen(id)` which toggles the `.active` class. There is no routing library.

**API calls** go directly to `https://api.anthropic.com/v1/messages` from the browser using `fetch()`. The Anthropic API key must be available (handled by the hosting environment or injected via a proxy).

---

## Storage Keys Reference

All data is stored in `localStorage` (with `sessionStorage` as fallback for profile data).

| Key | Type | Contents |
|-----|------|----------|
| `fx_pro_v5` | Object | User profile (name, flag, XP, streak, hearts, badges, perf stats, quizzes) |
| `fx_lb_v6` | Array | Leaderboard entries (all users + bots, up to 120) |
| `fx_bots_v2` | Array | Bot learner states (XP, streak, weekXp, next update time) |
| `fx_wod_v5` | Object | Word of the Day cache (word, definition, date) |
| `fx_theme_v5` | String | `"dark"` or `"light"` |
| `fx_chat_v2` | Array | Community chat messages (up to 300) |
| `fx_follows_v1` | Array | Names of users the current learner follows |
| `fx_suggests_local` | Array | Feature suggestions submitted by user |
| `fx_bugs_local` | Array | Bug reports submitted by user |
| `fx_notif_store_v2` | Array | Received push notifications (up to 200) |
| `fx_notif_seen_v2` | Array | IDs of notifications already read |
| `fx_notifs_broadcast` | Array | Admin broadcast notifications |
| `fx_notifs_u_{slug}` | Array | User-targeted notifications (slug = name, lowercase, max 24 chars) |

---

## BroadcastChannels Reference

| Channel | Direction | Payload |
|---------|-----------|---------|
| `fluentex_lb` | App → Admin | `{type:'new_feedback', item: entry}` — new suggestion or bug report |
| `fluentex_notif` | Admin → App | `{type:'push_notif', notif: notif}` — push notification |
| `fluentex_chat` | Bidirectional | `{type:'chat_update', ts: number}` — chat updated, re-render |

---

## Technology Stack

| Component | Technology |
|-----------|-----------|
| Language | Vanilla JavaScript (ES6+, no frameworks) |
| Styling | Hand-written CSS with CSS custom properties (dark/light themes) |
| Font | Google Fonts — Nunito |
| AI Model | `claude-sonnet-4-20250514` (Anthropic Claude API) |
| Storage | `localStorage` + `sessionStorage` |
| Real-time | Web `BroadcastChannel` API |
| Animations | CSS keyframes + `cubic-bezier` easing |
| Canvas | HTML5 Canvas (background particles + confetti) |

---

## Deployment

Because Fluentex calls the Anthropic API directly from the browser, you need either:

**Option A — Proxy server:** Host a lightweight proxy that injects the API key server-side and forwards requests to `https://api.anthropic.com/v1/messages`.

**Option B — Cloudflare Worker / Edge Function:** Use a Worker to proxy API calls and add the `Authorization` header.

**Option C — Local use:** Open the file locally in a browser that has the API key pre-injected (e.g. via a browser extension or local dev proxy).

The file itself has no external dependencies beyond Google Fonts (loaded from CDN) and the Anthropic API endpoint. It works offline for all non-AI features.

---

## Known Limitations

- **Single-user storage** — localStorage is per-browser. Two users on the same device share storage if they use the same browser profile.
- **No real cross-device sync** — leaderboard and chat data is stored locally. The `window.storage` API (Claude.ai-specific) was intentionally removed in v1.7.0 in favour of localStorage for standard browser compatibility.
- **Bot activity is local** — bot score updates and chat messages are generated client-side. Each browser instance runs its own bot engine independently.
- **API key exposure** — if deployed as a public file, the Anthropic API key will be visible in network requests. Always proxy API calls in production.
- **No moderation persistence across devices** — chat moderation actions (delete/restore) only propagate to other tabs in the same browser via BroadcastChannel.

---

*Made with ❤️ in India · Developed by SubhraniL · © 2026 Classify Technologies LLC*
