# OPIC Practice — Development Rules

## Version Numbering

Format: `YY.NN` — e.g. `26.01`, `26.02`, `26.03` ...

Every change to `index.html` MUST:
1. Increment `APP_VERSION` in JS (e.g. `26.02` → `26.03`)
2. Add a new top entry to the changelog in HTML (see below)
3. The `ver-num` span is auto-populated from `APP_VERSION` at DOMContentLoaded — do NOT edit it manually

## Changelog Format (inside index.html)

Location: `<div class="changelog-body" id="changelog-body">` on the home page.

Always insert a new `chg-cur` row at the top and demote the previous top entry to a plain `chg-item`:

```html
<!-- NEW (current) -->
<div class="chg-item chg-cur"><span class="chg-ver">vXX.NN</span><span class="chg-dot">·</span><span class="chg-desc">Short description of what changed</span></div>
<!-- previous entries below, no chg-cur class -->
<div class="chg-item"><span class="chg-ver">vXX.NN-1</span>...</div>
```

## Deploy Workflow

Branch: `claude/opic-practice-app-9arYU` → squash-merge to `main`

```bash
git add index.html
git commit -m "vXX.NN: <short description>"
git fetch origin main && git rebase origin/main
git push -f origin claude/opic-practice-app-9arYU
# create PR → merge → send user the link
```

After every push+merge, send the user: **https://toluen-kid.github.io/opic-practice/**

## Single-file App

- All code is in `/home/user/opic-practice/index.html` (~3000 lines)
- No build step — edit directly
- GitHub Pages serves `main` branch automatically

## Key Constants

| Symbol | Purpose |
|--------|---------|
| `APP_VERSION` | Semver badge + auto-update check |
| `QDB` | Question database (topics: warmup, home, work, hobby, music, movie, sports, travel, read, food, tech, shop, health) |
| `EXTRAS` | Roleplay/unexpected questions |
| `ST` | Global exam state object |
| `LEVELS` | 7 OPIC levels: NL, NM, NH, IL, IM, IH, AL |

## Exam Flow

Survey → startExam() → renderQ() → startListen1() → [wait 5s or btn] → startListen2() or skipListen() → startSpeak() → nextQ() → ... → finishExam()

- 15 questions: 1 warmup + 9 survey (65%) + 5 non-survey (35%)
- Listen1: TTS plays, then 5s countdown; user can press "Nghe lần 2" or let it auto-proceed
- Listen2: TTS plays, then auto-proceeds to speak immediately
- Question text hidden during listen phases; btn-toggle-q reveals on demand
