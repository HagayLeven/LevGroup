# LEV GROUP - Quote System

## Project Overview
A single-file HTML quote-generation system for **LEV GROUP** (קבוצת לב), an Israeli building management company in Ashdod. The system creates quotes, sends signed links to clients, captures digital signatures, and exports PDFs.

**Owner:** Hagay Levenshtein (@HagayLeven)
**Language:** Hebrew (RTL)
**Repo:** https://github.com/HagayLeven/LevGroup
**Live site:** https://hagayleven.github.io/LevGroup/ (once deployed)

## Read HANDOFF.md First
The file `HANDOFF.md` contains the full context of this project - business model, pricing logic, data schemas, and pending tasks. **Read it before starting work.**

## Tech Stack
- **Vanilla JavaScript** (no frameworks) in a single `index.html` file
- **Firebase Firestore** for quote storage and tariff settings
- **SignaturePad** for finger-based digital signatures
- **jsPDF + html2canvas** for client-side PDF generation
- **EmailJS + ntfy.sh** for sign-notifications
- **Google Fonts:** Heebo (Hebrew) + Instrument Serif (decorative)

## Key Files
- `index.html` - The entire application (~2540 lines)
- `README.md` - Public project documentation
- `404.html` - Custom error page
- `.gitignore` - Git exclusions
- `HANDOFF.md` - Full handoff document (read this!)

## Design Constraints
- **RTL Hebrew layout** - never break this
- **Mobile-first** - field agents use this on phones
- **Single-file architecture** - do NOT split into separate JS/CSS files
- **Works offline-ish** - minimal dependencies, no backend server needed
- **Brand:** Black (#1A1A1A) + Gold (#C5A572) + Cream (#FAF7F2)

## Immediate Tasks
1. **Deploy to GitHub Pages** via the empty repo at HagayLeven/LevGroup
2. **Configure Firebase** - replace the `YOUR_*` placeholders in the config block
3. **Configure EmailJS** (optional) and **ntfy topic**
4. **Test the full flow**: create quote → share link → sign → download PDF

## User Preferences
- **Communication language:** Hebrew (שיחה עם המשתמש בעברית)
- **Code comments:** Can be English
- **Philosophy:** Ship fast, iterate. Practical over theoretical.
- **Git expertise:** Basic - explain commands when running them.

## Commands Cheat Sheet
```bash
# Initial deploy to GitHub
git init
git add .
git commit -m "Initial commit - LEV GROUP quote system"
git branch -M main
git remote add origin https://github.com/HagayLeven/LevGroup.git
git push -u origin main

# After changes
git add .
git commit -m "Your message"
git push
```

## GitHub Pages Setup
After first push:
1. Go to https://github.com/HagayLeven/LevGroup/settings/pages
2. Source: Deploy from a branch
3. Branch: `main`, folder: `/ (root)`
4. Save → wait 1-2 minutes → site is live
