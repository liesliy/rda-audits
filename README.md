# Robot Dataset Audits

A public ledger of reproducible audits on open robot-learning datasets.

**Published site:** https://liesliy.github.io/rda-audits/
**Audit tool:** [`robot-data-audit`](https://pypi.org/project/robot-data-audit/) 0.12.2 ·
[source](https://github.com/liesliy/rda)

## What this is

Each entry is produced by downloading a dataset and running tooling against it on real files —
not by reading a dataset card. Structural checks (parquet, video, timestamps), annotation-layer
checks (subtask segmentation, temporal coverage), and an explicit list of what the tooling cannot
see.

Two rules make the ledger useful rather than decorative:

1. **No verdict is a raw exit code.** Anything a tool flags is re-measured from scratch before it
   is reported. Where the tool is wrong, the entry says so in the same amount of detail as where
   the dataset is wrong.
2. **Facts and inferences are separated.** Generalisations are labelled as inference and left
   unverified until a second dataset confirms them.

## Structure

```
.
├── index.html              # ledger index + method
├── datasets/
│   └── fineart.html        # Entry 01 — lerobot/FineART
├── evidence/
│   └── fineart/            # raw audit output behind Entry 01, unedited
├── data/
│   └── audits.json         # machine-readable ledger
├── assets/
│   └── style.css
└── .nojekyll               # serve as-is on GitHub Pages
```

`data/audits.json` mirrors every entry in a fixed schema. If you want to consume the results
programmatically (benchmark tables, dataset selection, CI checks), that file is the entry point.

## Entries

| # | Dataset | Verdict | Audited |
|---|---|---|---|
| 01 | [FineART](datasets/fineart.html) — Scale AI × Hugging Face LeRobot | pass with caveats | 2026-11-02 (rechecked 2026-11-03 on 0.12.2) |

Queued: `droid_1.0.1`, `libero`, `pusht`, `aloha_sim_transfer_cube_human`.

## Status of published findings

| Finding | Status |
|---|---|
| `video_freeze` false positive — 5 of 6 episodes excluded on healthy data | Tool defect, **fixed in 0.12.2** (criterion now detects duplicated frames instead of small frame differences). Same subset re-runs 6/6 PASS, exit 0. |
| Annotation layer has zero metric coverage (`language_persistent` / `language_events` dropped by the loader) | **Open.** This is the dataset's main value and no mainstream tool checks it. |
| Trailing-gap reading in an earlier draft | **Withdrawn** — misread of the annotation protocol, correction kept in the entry. |

Evidence files under `evidence/` are raw tool output, unedited. Absolute paths in them reflect the
machine the audit ran on.

## Disclosure

The audit tool used here, [`robot-data-audit`](https://pypi.org/project/robot-data-audit/), is
maintained by the author of this ledger. That is a conflict of interest and it is stated on every
page. Read the entries accordingly: several findings are cases where the tool produced incorrect
verdicts, and one published finding has already been corrected in place.

## Corrections

If an entry misrepresents your dataset, open an issue or email
[luoxi@touchlabelai.cn](mailto:luoxi@touchlabelai.cn). Corrections are applied in place and kept
visible rather than deleted — the FineART entry contains one.

## Publishing on GitHub Pages

The directory is a self-contained static site. Push it to a repository, enable Pages on the
default branch, and set the source to the repository root. `.nojekyll` is present so that no
Jekyll processing is applied.

## Requests

If you maintain a dataset and want it audited, or you have in-house robot data that needs the same
treatment as a private report, get in touch:
[luoxi@touchlabelai.cn](mailto:luoxi@touchlabelai.cn) · [open an issue](https://github.com/liesliy/rda-audits/issues).
