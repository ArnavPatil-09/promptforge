# PromptForge ⚒️

**Turn a rough project idea into a build-ready AI prompt — in under a minute.**

A dark-themed, single-file web app that helps developers and students stop wasting tokens on vague first prompts. Describe your idea once, get a structured brief that any AI coding assistant can actually act on.

🔗 **Live site:** `https://arnavpatil-09.github.io/promptforge/`

---

## Why this exists

Half the "wasted tokens" problem in AI-assisted development comes from vague opening prompts — you explain, it asks, you clarify, repeat. PromptForge forces structure upfront:

- **Role** — tells the AI what kind of expert to act as
- **Idea** — your one-liner, kept as-is
- **Features** — explicit must-haves so nothing gets assumed away
- **Stack** — or a request for a recommendation if you don't have one
- **Constraints** — deadlines, platforms to avoid, complexity limits
- **Output format** — full code / architecture first / MVP / tutorial

---

## Features

| Feature | Details |
|---|---|
| 7 project types | Website, Mobile App, Backend/API, AI/ML, Browser Extension, Game, Custom |
| Live preview | Prompt updates as you type, no button click needed |
| Quick-add chips | Contextual feature suggestions per project type |
| Enhance with Claude | Calls Anthropic API to tighten wording (needs your API key) |
| Prompt history | Last 30 prompts saved across sessions via artifact storage |
| Download .md | Saves the prompt as a Markdown file |
| Autosave | Form state saved automatically as you type |
| No build step | Single HTML file — open in browser and it works |

---

## Setup — Enhance with Claude (optional)

The "Enhance" button calls the Anthropic API directly from your browser. To use it on the live site:

1. Go to [console.anthropic.com](https://console.anthropic.com) → API Keys → Create key
2. On PromptForge, click **API Key** in the top-right corner
3. Paste your key — it's kept in memory only and cleared on page refresh
4. Your key is **never stored** anywhere — verify this in the source code yourself

> New Anthropic accounts receive free credits. You won't need many — each "Enhance" call uses roughly 500–1500 tokens.

---

## Run locally

```bash
git clone https://github.com/ArnavPatil-09/promptforge.git
cd promptforge
# Just open index.html in your browser — no npm, no build, nothing
open index.html        # macOS
start index.html       # Windows
xdg-open index.html   # Linux
```

---

## Deploy to GitHub Pages

```bash
# 1. Push to GitHub (main branch)
git push origin main

# 2. In repo Settings → Pages
#    Source: Deploy from a branch
#    Branch: main  /  (root)
#    Save

# Live in ~60 seconds at:
# https://arnavpatil-09.github.io/promptforge/
```

---

## Tech stack

- Vanilla HTML/CSS/JS — zero dependencies, zero build tools
- [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono) + [Big Shoulders Display](https://fonts.google.com/specimen/Big+Shoulders+Display) + [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts
- Anthropic Messages API (`claude-sonnet-4-6`) for the Enhance feature
- Artifact persistent storage API (for history — works inside Claude.ai artifacts)

---

## Project structure

```
promptforge/
├── index.html      # The entire app — HTML + CSS + JS in one file
├── README.md       # This file
└── .gitignore      # Standard web project ignores
```

---

## Roadmap

- [ ] Export prompt as a shareable link
- [ ] More project type templates (CLI, Desktop app, Chrome extension, Figma plugin)
- [ ] Prompt versioning — compare before/after edits
- [ ] Dark/light mode toggle

---

## Built by

[Arnav Patil](https://github.com/ArnavPatil-09) — B.Tech Computer Engineering, Pillai University

Part of a series of tools built to solve real developer friction points.

---

## License

MIT — use it, fork it, build on it.
