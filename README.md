# some_skills

A collection of AI-assistant skills and project notes maintained by [zhulei227](https://github.com/zhulei227).

[中文说明](README.zh-CN.md)

## Contents

### `kaggle_solutions/` — Kaggle Solutions Skill

A [CodeBuddy](https://www.codebuddy.ai) skill for searching, reading offline, and learning from **721 Kaggle competitions / 4700+ ranked solution links** (mirrored from [kaggle.farid.one](https://kaggle.farid.one), source: [faridrashidi/kaggle-solutions](https://github.com/faridrashidi/kaggle-solutions)).

**Key features**

- **Offline-first**: the full metadata archive (721 competitions) and ~1280 top-1/2/3 solution page texts are stored in `kaggle_solutions/data/` — all queries run locally, no network needed
- **Task matching**: given a business description or a project directory, automatically finds similar past competitions and distills winner techniques into an actionable optimization report (`match_task.py`)
- **Semantic search**: local numpy TF-IDF vector index over competitions and solution texts (`embedding_index.py`), hybrid-ranked with keyword matching
- **Chinese input**: built-in CN→EN concept map — descriptions, docs and queries in Chinese work out of the box
- **Auto download**: resumable browser-based downloader for solution texts (`download_solutions.py`, playwright-cli + Edge/Chrome)

**Quick start**

```powershell
cd kaggle_solutions

# match your task and get an optimization report (Chinese or English)
python scripts\match_task.py "detect credit card fraud" --metric auc --out report.md
python scripts\match_task.py --dir path\to\your\project --out report.md

# read an offline winner solution / full-text search / query metadata
python scripts\kaggle_solutions.py read 721 1
python scripts\kaggle_solutions.py grep "pseudo label"
python scripts\kaggle_solutions.py search image segmentation --kind Research --solutions

# semantic search
python scripts\embedding_index.py "sales forecasting time series" --corpus solutions --top 10
```

See `kaggle_solutions/SKILL.md` for the full workflow and data schema.

**Activate as a skill**: copy `kaggle_solutions/` into `~/.codebuddy/skills/` (user scope) or `.codebuddy/skills/` (project scope).

### `easymlops/` — EasyMLOps Skill

Notes, reference docs and a sample wheel for [EasyMLOps](https://github.com/zhulei227/EasyMLOps), an MLOps toolkit. See `easymlops/SKILL.md`.

## Repository layout

```
some_skills/
├── kaggle_solutions/          # Kaggle solutions skill
│   ├── SKILL.md               # skill manifest: workflows, data schema
│   ├── scripts/               # query / download / match / embedding CLIs
│   └── data/                  # offline snapshot (yml, pages, vector index)
└── easymlops/                 # EasyMLOps notes and reference docs
```

## Updating the offline archive

```powershell
cd kaggle_solutions
python scripts\download_solutions.py --status      # progress (rank<=3 pages)
python scripts\download_solutions.py --max-rank 3  # resume/continue downloads
python scripts\kaggle_solutions.py refresh         # refresh metadata
python scripts\embedding_index.py build            # rebuild the vector index
```

## Notes

- `logs/` and `.codebuddy/` are working data and intentionally not tracked.
- Kaggle solution texts are fetched from publicly readable pages for personal learning use; all rights remain with their authors.
