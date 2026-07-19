# PromptForge — Complete Dev Log

> Full build history: from idea to live deployed tool, documented in order.

---

## What is PromptForge?

A single-file web app that turns a rough project idea into a structured, build-ready AI prompt in under a minute.

**The problem it solves:** Most developers describe their idea vaguely → the AI asks clarifying questions → you clarify → it asks again. You've burned half your context window before writing a single line of code. PromptForge forces structure upfront so your first prompt is your best prompt.

**Live site:** https://arnavpatil-09.github.io/PromptForge  
**Repository:** https://github.com/ArnavPatil-09/promptforge

---

## Build Timeline

### v1.0 — Initial Build
**What was built:**
- Single-file HTML app with forge/industrial dark theme
- 7 project type chips (Website, Mobile App, Backend/API, AI/ML, Browser Extension, Game, Custom)
- Live prompt preview — updates as you type, no button click needed
- Template-based prompt assembly (Role → Idea → Features → Stack → Constraints → Output format)
- "Enhance with Claude" button — calls Anthropic API to tighten prompt wording
- Copy to clipboard + Download as .md
- Prompt history — last 30 prompts saved across sessions
- Form autosave — restores your last session on reload

**Tech decisions:**
- Zero dependencies, zero build step — vanilla HTML/CSS/JS only
- IBM Plex Mono + Big Shoulders Display + Inter via Google Fonts
- In-memory API key (never stored, cleared on refresh)
- `anthropic-dangerous-direct-browser-access: true` header for direct browser → Anthropic calls

---

### v1.1 — GitHub Pages Deployment
**What was done:**
- Renamed `index.html` (GitHub Pages serves root `index.html` as homepage)
- Created `README.md` with project description, setup guide, and deployment steps
- Created `.gitignore` (OS files, editor files, `.env`, `*.key`)
- Fixed Windows double-extension bug (`README.md.md`, `index.html.html`) — Windows hides extensions by default; fix via View → Show → File name extensions
- Removed duplicate files with `git rm`
- Enabled GitHub Pages: Settings → Pages → Deploy from branch → main → / (root)

**Lessons learned:**
- `git remote add origin` fails if already set → skip it, use `git push` directly
- GitHub Pages takes 60–90 seconds to go live after enabling
- Personal Access Token required instead of password for `git push`

**Live at:** https://arnavpatil-09.github.io/PromptForge

---

### v2.0 — Cinematic Redesign
**What was rebuilt:**

**Forge Loader**
- Full-screen loading screen on page open
- PROMPTFORGE wordmark animates in, orange progress bar fills, "Heating up the forge…" label
- Fades out after 1.8 seconds

**Cinematic Hero Section**
- Full-screen landing with giant `DESCRIBE / ONCE.` wordmark
- Second line rendered as outline text (CSS `-webkit-text-stroke`)
- Animated grid background with ember glow radial gradient
- Staggered animate-in for eyebrow, title, subtitle, CTA buttons
- Bouncing scroll arrow hint at bottom

**How It Works Strip**
- 4-step explainer below hero: 01 → 02 → 03 → 04
- Large faded numbers as background decoration

**Wizard Steps**
- Left panel split into 3 steps: Type → Idea → Details
- Slide-in animation (`translateX`) on step transition
- Progress pill bar at top (Type · Idea · Details)
- `Ctrl + Enter` to advance steps

**Typewriter Animation**
- Prompt output types character by character
- Speed auto-calculated based on prompt length
- Blinking ember cursor while typing
- Cursor removed on completion

---

### v2.1 — Templates + Keyboard Shortcuts + Mobile Fix

**Prompt Templates Library (14 templates, 6 categories)**

| Category | Templates |
|---|---|
| 🌐 Web | E-Commerce Store, Blog/CMS, Portfolio Site, SaaS Dashboard |
| 📱 Mobile | Habit Tracker, Expense Splitter |
| ⚙️ Backend | Auth Service, REST API + CRUD, Notification Service |
| 🤖 AI/ML | Text Classifier, Image Classifier, RAG Chatbot |
| 🎮 Game | Snake Game, 2D Platformer |
| 🧩 Extension | Pomodoro Timer, Tab Manager |

One click fills the entire form — type, idea, stack, features, audience, format all set. Automatically advances to Step 2 so user can tweak.

**Keyboard Shortcuts**

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Next step / Copy prompt on last step |
| `Ctrl + Shift + C` | Copy prompt |
| `Ctrl + T` | Open templates |
| `Ctrl + H` | Open history |
| `Esc` | Close any modal or drawer |
| `?` | Toggle shortcuts panel |

**Mobile Responsive Fixes**
- Hero title scales: `clamp(44px, 14vw, 72px)` on mobile
- CTA buttons stack vertically and go full-width
- How-it-works strip stacks vertically, dividers hidden
- Workshop goes single column
- Output panel not sticky on mobile (scroll below form)
- Plate max-height reduced to 320px on mobile
- Action buttons stack vertically
- Chip grid 2-col on mobile, 1-col below 400px
- Templates grid 2-col on mobile, 1-col below 400px
- API Key label hidden on mobile (icon only)
- Keyboard hint button hidden on mobile

---

## File Structure

```
promptforge/
├── index.html      ← entire app (HTML + CSS + JS, single file)
├── README.md       ← project documentation
└── .gitignore      ← OS/editor/env ignores
```

---

## Prompt Output Structure

Every generated prompt follows this exact structure:

```markdown
# Project Brief — {Type}

## Role
Act as an experienced {role}. Ask only if something is genuinely ambiguous...

## The Idea
{user's idea}

## Target Users          ← only if filled
{audience}

## Must-Have Features    ← only if features added
- Feature 1
- Feature 2

## Tech Stack
{stack or "No strong preference — recommend..."}

## Constraints           ← only if filled
{constraints}

## What I Want From You
{one of: full code / architecture first / MVP / tutorial}

## Response Format
Use clear headings, keep code in complete runnable blocks...
```

---

## API Integration

**Enhance with Claude** — calls `claude-sonnet-4-6` via Anthropic Messages API directly from the browser.

```javascript
fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': _apiKey,                                    // user-provided, in-memory only
    'anthropic-version': '2023-06-01',
    'anthropic-dangerous-direct-browser-access': 'true'      // required for browser calls
  },
  body: JSON.stringify({
    model: 'claude-sonnet-4-6',
    max_tokens: 1000,
    messages: [{ role: 'user', content: ENHANCE_PROMPT + draft }]
  })
})
```

API key is never persisted — stored in a JS variable (`let _apiKey = ''`), cleared on page refresh.

---

## Commit History

| Commit | Message | What changed |
|---|---|---|
| `d44db0d` | feat: PromptForge v1.0 initial release | First push (had double-extension bug) |
| `fac9e5c` | feat: PromptForge v1.0 initial release | Second push attempt |
| `d45edeb` | cleanup: remove duplicate double-extension files | `git rm README.md.md index.html.html` |
| `62b2f98` | feat: cinematic hero, wizard steps, typewriter animation | v2.0 full redesign |
| next | feat: templates library, keyboard shortcuts, mobile responsive | v2.1 |

---

## Planned Features (in priority order)

- [ ] **Prompt Scoring** — AI rates your prompt 1–10 with specific feedback ("tech stack missing", "add constraints")
- [ ] **Diff View** — Draft vs Enhanced side-by-side comparison
- [ ] **Dark/Light Mode Toggle**
- [ ] **Multi-language Support** — prompts in Hindi/Hinglish
- [ ] **Export Options** — JSON, share via URL, Notion export

---

## Deploy Commands (reference)

### First time setup
```bash
git init
git add .
git commit -m "feat: initial release"
git branch -M main
git remote add origin https://github.com/ArnavPatil-09/promptforge.git
git push -u origin main
```

### Every update after that
```bash
git add index.html
git commit -m "feat: what you changed"
git push
```

### Fix mistakes
```bash
git rm filename           # remove file from repo + local
git rm --cached filename  # remove from repo only, keep local
git status                # see what's staged / unstaged
```

---

## Tech Stack

| Layer | Choice | Why |
|---|---|---|
| Language | Vanilla JS | Zero build step, works anywhere, no node_modules |
| Styling | Pure CSS with CSS variables | No framework needed for single file |
| Fonts | Google Fonts (IBM Plex Mono, Big Shoulders Display, Inter) | Loaded via CDN |
| API | Anthropic Messages API | claude-sonnet-4-6 for enhance feature |
| Storage | Artifact persistent storage API | History + autosave across sessions |
| Hosting | GitHub Pages | Free, instant, no CI/CD needed |

---

*Built by Arnav Patil — B.Tech Computer Engineering, Pillai College, Navi Mumbai*
