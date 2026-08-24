# TianoForge: Automated Bug Triage for TianoCore EDK II

TianoForge is an integrated automated bug triage script for the [TianoCore EDK II](https://github.com/tianocore/edk2) ecosystem. It unifies four key triage tasks — invalid issue detection, duplicate detection, priority prediction, and developer assignment — into a single sequential workflow powered by Large Language Models (LLMs), domain-specific prompt engineering, and hybrid retrieval (BGE + BM25 + RRF).

---

## Repository Structure

```
TianoForge/
├── finalized-integrated-script/
│   ├── triage_integrated_script.ipynb        # End-to-end TianoForge script
│   └── finialized-integrated-script-results/
│       ├── RUN1/                             # Predictions and metrics for run 1
│       ├── RUN2/                             # Predictions and metrics for run 2
│       └── RUN3/                             # Predictions and metrics for run 3
│
├── finalized_4_sub_tasks/
│   ├── bug_assignment.ipynb                  # Developer assignment (10 models × 2 systems)
│   ├── duplicate_detection.ipynb             # Duplicate detection (10 models × 2 systems)
│   ├── invalid_detection.ipynb               # Invalid issue detection (10 models × 2 systems)
│   ├── priority_prediction.ipynb             # Priority classification (10 models × 2 systems)
│   └── Results-for-each-task/
│       ├── bug-assignment/
│       ├── duplicate-detection/
│       ├── invalid-detection/
│       └── priority-prediction/
│
└── LICENSE
```

---

## Models Evaluated

Ten state-of-the-art LLMs are evaluated across all tasks:

| Provider | Models |
|---|---|
| OpenAI | `gpt-4o-mini`, `gpt-4o`, `gpt-4.1-mini`, `gpt-4.1`, `gpt-5.4-mini`, `gpt-5.4`, `gpt-5.5` |
| Anthropic | `claude-haiku-4-5`, `claude-sonnet-4-6`, `claude-opus-4-7` |

---

## Selected Best-Performing Configuration (TianoForge Default)

| Task | Model | System |
|---|---|---|
| Invalid Detection | `claude-sonnet-4-6` | A |
| Duplicate Detection | `claude-sonnet-4-6` | A |
| Priority Prediction | `claude-sonnet-4-6` | A |
| Bug Assignment | `gpt-5.5` | A |

All other models and configurations remain available as a comment in the integrated notebook.

---

## Dataset

The experiments use a dataset of **2,610 closed EDK II bug issues** collected from the [TianoCore GitHub issue tracker](https://github.com/tianocore/edk2/issues) on April 3, 2026:

- **2,535 Bugzilla-transferred issues** 
- **75 GitHub-native issues** — evaluation set (filed directly on GitHub, Dec 2024 – Feb 2026)
  - Priority distribution: 32 medium, 28 low, 15 high
  - 37 issues carry a known first assignee (used for assignment evaluation)

The dataset is publicly available on Dataverse. Place the CSV file at:
- Google Drive root, or
- Inside the task-specific Drive folder (see notebook Cell 2 for path details)

---

## Setup and Usage

All notebooks are designed to run on **Google Colab**.

### 1. Add API Keys to Colab Secrets

In Colab, go to **Secrets** (🔑 icon) and add:

| Secret name | Value |
|---|---|
| `OPENAI_API_KEY` | Your OpenAI API key |
| `OPENAI_ORG_ID` | Your OpenAI organization ID (optional) |
| `OPENAI_PROJECT_ID` | Your OpenAI project ID (optional) |
| `ANTHROPIC_API_KEY` | Your Anthropic API key |

### 2. Upload the Dataset

Upload the dataset to your Google Drive root, or to the task-specific folder defined in Cell 2 of each notebook.

### 3. Run the Integrated Script

Open `finalized-integrated-script/triage_integrated_script.ipynb` and run all cells from top to bottom. The script processes all 37 labeled issues sequentially across all four tasks and saves results to Google Drive under `triage_framework/results/`.

### 4. Run Individual Sub-Task Notebooks

Each notebook in `finalized_4_sub_tasks/` evaluates all 10 models under both System A and System B for a single triage task. Run cells top to bottom. Results are saved to the task-specific Drive folder.

---

## Dependencies

```
openai
anthropic
chromadb
sentence-transformers
rank_bm25
scikit-learn
pandas
tqdm
```

All dependencies are installed automatically in Cell 1 of each notebook via `pip install`.

---

## Key Results


TianoForge reduces average triage time from **10.86 days** (manual) to **7.08 minutes** (automated), a reduction of **99.95%** (~2,208× speedup).


---
 
## Paper
 
This repository supports the following paper:

> N. Siavash, T. E. Boult, and A. Moin, “TianoForge: An automated bug triage approach for the TianoCore UEFI firmware development community,” in  Proc. Int. Workshop on Firmware Testing and Analysis (FTA), ISSTA 2026, Oakland, CA, USA, 2026.
 
---

## License

This project is licensed under the terms of the LICENSE file included in this repository.

---

## Acknowledgments

This material is based upon work supported by the U.S. National Science Foundation (NSF) under Grant No. 2534021. In preparing this work, we used generative AI models and tools, including GPT and Claude models, to assist with generating and revising content, including code and text.
