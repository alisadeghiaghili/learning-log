# DVC Learning Log

**Course:** Introduction to Data Versioning with DVC  
**Platform:** DataCamp  
**Repo:** [DataCamp Course Link](https://app.datacamp.com/learn/courses/introduction-to-data-versioning-with-dvc)  
**Goal:** Bulletproof, practical mastery of DVC for ML projects and interviews.

---

## Folder Structure

```
dvc/
├── README.md            ← this file
├── progress.md          ← session-by-session progress tracker
├── flashcards.md        ← Anki-ready flashcards (all sections)
├── notes/               ← section & module notes
│   └── .gitkeep
└── master/              ← cumulative master compendium (topic-first)
    ├── README.md
    ├── core-model-git-and-cache.md
    ├── data-versioning-and-remotes.md
    ├── pipelines-and-dag.md
    ├── metrics-params-and-plots.md
    ├── experiments.md
    ├── model-registry-and-studio.md
    ├── cicd-and-cml.md
    └── references.md
```

---

## Mental Model (North Star)

| Layer | Tool | What it tracks |
|---|---|---|
| Code & pointers | Git | `.dvc` files, `dvc.yaml`, `dvc.lock`, params |
| Data & models | DVC cache + remote | Actual large files (content-addressed by MD5) |

> **Rule #1:** `git push` moves pointers. `dvc push` moves data. Both are always needed.
