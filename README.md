# 📚 Learning Log

> Structured deep-dive notes, flashcards, and spaced-repetition tracking across **Machine Learning**, **Math**, **SQL**, **DVC**, **Python**, and **Web Scraping** — built for interview-grade mastery.

[![Anki Export](https://img.shields.io/badge/anki-auto--export-blue)](https://github.com/alisadeghiaghili/learning-log/actions)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Language: FA/EN](https://img.shields.io/badge/lang-فارسی%20%7C%20English-orange)](#)

این مخزن، حافظه‌ی بیرونی من برای یادگیری و مرور است: یادداشت‌های عمیق، فلش‌کارت فارسی/انگلیسی، گزارش پیشرفت و منابع، برای هر موضوعی که مطالعه می‌کنم. هدف، تسلطی است که در مصاحبه‌ی تخصصی و پروژه‌ی واقعی به کار بیاید — نه خواندن و رد شدن. عمومی‌اش کردم چون یادگیری در فضای باز، هم
مرا منظم نگه می‌دارد و هم شاید به کار کسی بیاید که همین مسیر را می‌رود. نقد و اصلاح هم پذیرفته است: اگر جایی اشتباه نوشته‌ام، Issue باز کنید.

This repo is my living learning notebook: deep-dive notes, bilingual (Persian/English) flashcards, progress tracking, and curated references for topics I'm actively studying or reviewing — aimed at interview-grade mastery, not one-time reading.

---

## ✨ Why this repo exists

- **Spaced repetition, not passive notes.** Every topic ships with Anki-ready flashcards, auto-exported via GitHub Actions.
- **Bilingual by design.** Notes and cards exist in both English and Persian (`flashcards.md` / `flashcards-fa.md`) to serve a wider learning community.
- **Quality-checked.** A `validate.py` script per topic enforces structure and catches malformed cards before they reach Anki.
- **Transparent progress.** `progress.md` files track what's mastered, what's in review, and what's queued next.

## 🗂️ Repository structure

```
learning-log/
├── deep-learning/       # Neural nets, DL theory & math
├── dvc/                 # Data Version Control workflows
├── python/              # Python language & packaging deep-dives
├── sql/                 # SQL & dimensional modeling
├── web-scraping/        # Scraping techniques & tooling
└── .github/workflows/   # Automated Anki export pipelines
```

Each topic folder follows the same convention:

| File / folder | Purpose |
|---|---|
| `flashcards.md` / `flashcards-fa.md` | Human-readable Q&A cards (English / Persian) |
| `flashcards_anki.txt` / `flashcards-fa_anki.txt` | Auto-generated, Anki-importable exports |
| `flashcards_needs_review_anki.txt` | Cards flagged for quality review before import |
| `notes/` | Long-form deep-dive notes on subtopics |
| `references.md` | Curated books, courses, papers, and articles |
| `progress.md` | Status log — mastered / in-progress / planned |
| `validate.py` | Validates card formatting and structure for that topic |

## 🚀 Getting started

```bash
git clone https://github.com/alisadeghiaghili/learning-log.git
cd learning-log

# Validate a topic's flashcards before importing to Anki
python python/validate.py
```

Flashcard exports (`*_anki.txt`) are generated automatically by GitHub Actions and can be imported directly into [Anki](https://apps.ankiweb.net/) via **File → Import**.

## 🤝 Contributing

**این مخزن برای مشارکت همه باز است.** اگر روی این موضوعات یا حوزه‌های نزدیک به آن‌ها مطالعه می‌کنید، خوشحال می‌شوم نتیجه‌ی یادگیری‌تان را با دیگران به اشتراک بگذارید. لازم نیست مشارکت بزرگی داشته باشید؛ اضافه کردن یک فلش‌کارت دقیق، اصلاح یک اشتباه کوچک یا معرفی یک منبع خوب هم بسیار ارزشمند است.

**Contributions are genuinely welcome — this is meant to grow as a community learning resource, not stay a solo notebook.**

Ways to contribute:

- ➕ Add new flashcards (English and/or Persian) to an existing topic
- 📝 Improve or expand deep-dive notes in `notes/`
- 🔗 Suggest high-quality references (books, papers, courses) in `references.md`
- 🌱 Propose a brand-new topic folder following the existing structure
- 🐛 Fix typos, broken formatting, or inaccurate explanations
- 🧪 Improve `validate.py` scripts or the Anki-export workflows

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide on card format, folder conventions, and the PR process before opening a pull request. Even a single well-written flashcard is a valid, appreciated contribution.

پیش از باز کردن Pull Request، لطفاً راهنمای [CONTRIBUTING.md](CONTRIBUTING.md) را بخوانید. در آن، قالب فلش‌کارت‌ها، ساختار پوشه‌ها و روند ارسال تغییرات توضیح داده شده است. یک فلش‌کارت خوب هم می‌تواند شروع یک همکاری مفید باشد؛ منتظر نمانید تا تغییرتان خیلی بزرگ شود.

### Good first issues

Look for issues labeled [`good first issue`](https://github.com/alisadeghiaghili/learning-log/labels/good%20first%20issue) or [`help wanted`](https://github.com/alisadeghiaghili/learning-log/labels/help%20wanted) — these are curated as approachable entry points for new contributors.

## 🧭 Roadmap ideas

- Expand `web-scraping` and `dvc` note depth
- Add a `data-quality` topic folder
- Automated card-count / coverage badge per topic

## 📄 License

This project is licensed under the [MIT License](LICENSE) — use, adapt, and share freely, with attribution.

## 🙌 Acknowledgements

Maintained by [Ali Sadeghi Aghili](https://github.com/alisadeghiaghili). Built with help from AI-assisted tooling for card generation, validation, and review — and with gratitude to every future contributor who helps this grow into a shared learning resource.
