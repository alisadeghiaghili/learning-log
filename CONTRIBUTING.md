# Contributing to Learning Log

Thank you for considering a contribution — whether it's one flashcard, a typo fix, or a brand-new topic folder, it's genuinely welcome here. 🎉

از اینکه برای مشارکت در این مخزن وقت می‌گذارید ممنونم. فرقی نمی‌کند یک فلش‌کارت اضافه کنید، اشتباه کوچکی را اصلاح کنید یا موضوع تازه‌ای پیشنهاد دهید؛ هر مشارکت مفیدی به بهتر شدن این منبع یادگیری کمک می‌کند.

## Ground rules

- Be kind and constructive in reviews and discussions.
- Keep notes accurate — cite a source or reference when adding a factual claim you didn't verify yourself.
- Prefer clarity over cleverness: these notes should help a learner, not impress one.

## How to contribute

1. **Fork** the repository and create a branch from `main`:
   `git checkout -b add/topic-flashcard-description`
2. Make your change inside the relevant topic folder (see structure below).
3. If you touched flashcards, run that topic's `validate.py` locally before committing:
   ```bash
   python <topic>/validate.py
   ```
4. Commit with a clear message (e.g. `Add flashcards on gradient clipping to deep-learning`).
5. Open a **pull request** describing what you added or changed and why.

لازم نیست تغییرتان بزرگ باشد. اصلاح یک غلط تایپی یا افزودن یک فلش‌کارت باکیفیت هم یک Pull Request کاملاً ارزشمند است.

## Adding flashcards

Flashcards live in `flashcards.md` (English) and `flashcards-fa.md` (Persian) inside each topic folder. Follow the existing Q&A format in those files exactly, for example:

```markdown
### Q: What does DVC track that Git does not?
A: DVC tracks large data/model files and their versions via lightweight pointer
files, while Git tracks the pointers themselves — keeping the Git repo small
while data stays versioned and reproducible.
```

Guidelines:

- One concept per card — keep it atomic so spaced repetition works well.
- Write the answer as you'd explain it in an interview: precise, not padded.
- If you write a card in English, a Persian counterpart is appreciated but not mandatory (and vice versa).
- Do not hand-edit the `*_anki.txt` export files — those are regenerated automatically by the GitHub Actions workflow from the Markdown source.

## Adding or improving notes

Long-form notes go in `<topic>/notes/`. Use descriptive filenames (e.g. `backpropagation.md`, `window-functions.md`) and structure notes with headers, short paragraphs, and code/SQL snippets where relevant.

## Adding references

Add curated books, courses, papers, or articles to `<topic>/references.md`, following the existing list format. A one-line note on why the reference is worth including is encouraged.

## Proposing a new topic

Open an issue first to discuss scope before creating a new top-level folder. If approved, mirror the existing structure:

```
new-topic/
├── flashcards.md
├── flashcards-fa.md
├── notes/
├── references.md
└── progress.md
```

## Validation & CI

Each topic's `validate.py` checks flashcard formatting and structure. Pull requests that modify flashcards should pass validation locally before review — the maintainer may also run the Anki-export workflow on merge.

## Questions?

Open an issue or start a discussion — happy to clarify format questions or brainstorm new topics with you.
