# TianoShield — Task 1.1: Automated Triage Tooling for TianoCore EDK II

This repository is **Task 1.1 of the TianoShield project**. It collects a family of automated tools built around the [TianoCore EDK II](https://github.com/tianocore/edk2) issue tracker. 

TianoForge (below) is one component of this task alongside three complementary tools for report enhancement, title optimization, and advisory enhancement.

All tools are implemented as Jupyter/Colab notebooks that call GitHub's API for issue data and an LLM (primarily `claude-sonnet-4-6`, with broader multi-model comparisons in TianoForge) to produce structured, exportable results.

---

## Repository Structure

```
Task1.1/                             # TianoShield Task 1.1
├── TianoForge/                      # Core automated triage system (see its own README)
│   ├── TianoForge-main/
│   │   ├── finalized-integrated-script/   # End-to-end 4-task triage pipeline
│   │   ├── finalized_4_sub_tasks/         # Per-task, multi-model (10 LLMs) benchmarks
│   │   └── README.md
│   └── AllBugs/                     # Local-Ollama (qas1) run of the integrated triage script
│       ├── triage_integrated_script.ipynb
│       └── triage_results/
│
├── Bug-Report-Enhancement/          # Enriches bug reports with structured fields + scores
│   ├── sample/                      # Runs on a small sample set via Colab Secrets / Anthropic API
│   └── AllBugs/                     # Runs on all open AND closed 'type:bug' issues via local Ollama runtime
│
├── Bug-Title-Opt/                   # Optimizes bug titles 
│   ├── sample/
│   └── AllBugs/
│
└── Advisory-Enhancement/            # Enriches security advisories with structured fields + scores
    └── results/
```

Each `sample/` notebook is the same pipeline as its `AllBugs/` counterpart, scoped to a small hand-picked set of issues for quick iteration. `sample/` fetches only **open** `type:bug` issues; `AllBugs/` fetches **both open and closed** `type:bug` issues. Every `AllBugs/` notebook (Bug-Report-Enhancement, Bug-Title-Opt, and TianoForge's AllBugs/ triage run) uses the same local model, qwen2.5:14b served via Ollama on host qas1, in place of the hosted claude-sonnet-4-6 used by `sample/`.

---

## Sub-Projects

### 1. TianoForge
A single pipeline that runs four triage tasks — invalid-issue detection, duplicate detection, priority prediction, and developer assignment — using hybrid retrieval (BGE + BM25 + RRF) and LLM prompting, benchmarked across 10 models (OpenAI + Anthropic). Reduces triage time from ~10.9 days manual to ~7 minutes automated in the reported evaluation.
See [`TianoForge/TianoForge-main/README.md`](TianoForge/TianoForge-main/README.md) for full details, dataset info, and setup. `TianoForge/AllBugs/` contains a parallel run of the same integrated script against a local Ollama deployment (`qwen2.5:14b`) instead of hosted APIs.

### 2. Bug Report Enhancement
Fetches `type:bug` issues (title + body) from `tianocore/edk2` — open only in `sample/`, open and closed in `AllBugs/` — and asks an LLM to produce an enhanced structured record for each: verified/corrected metadata (bug type, regression status, package, build target, architecture), an inferred impact statement, and three prioritization scores (severity, patch complexity, component criticality). Exports to JSON and CSV.

### 3. Bug Title Optimizer
Fetches the same `type:bug` issues (open only in `sample/`, open and closed in `AllBugs/`) and rewrites each title, returning a confidence-scored result alongside a quality score for the original title. Exports to JSON and CSV; `sample/morethan80.txt` lists issues that scored ≥80 on both title quality and reflective confidence.

### 4. Advisory Enhancement
A triager-assist tool for published GitHub security advisories. The LLM confirms or corrects metadata GitHub already reports (severity, affected package) and extracts fields GitHub doesn't structure (component, build target, architecture, impact, patch complexity, criticality). Results are then checked against independently fetched GitHub ground truth (repo tree structure, linked fix commits/PRs) so a human triager knows how much to trust each field — the LLM's raw answer, the ground truth, and whether they matched are all kept as separate columns rather than merged.

---

## Setup and Usage

All notebooks are designed to run on **Google Colab** (the `AllBugs` triage/enhancement notebooks instead target a local Ollama runtime — see the notebook's Step 1 for details).

1. **Add API keys as Colab Secrets** (🔑 icon in the left sidebar), enabling notebook access for each:

   | Secret name | Used by | Notes |
   |---|---|---|
   | `ANTHROPIC_API_KEY` | All `sample/` notebooks, Advisory-Enhancement, TianoForge | https://console.anthropic.com |
   | `OPENAI_API_KEY` | TianoForge multi-model notebooks | Optional org/project ID secrets also supported |
   | `GITHUB_TOKEN` | All notebooks that call the GitHub API | Optional but recommended — raises the rate limit from 60 to 5,000 req/hr |

2. **Upload any required dataset** to Google Drive as instructed in each notebook's configuration cell (TianoForge only — the other notebooks fetch live data directly from the GitHub API).

3. **Run cells top to bottom.** Each notebook follows the same shape: API key setup → configuration → data model → fetch from GitHub → LLM processing → per-item results → summary dashboard → JSON/CSV export.

### Dependencies

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

These are installed automatically in the first cell of each notebook via `pip install`.

---

## Outputs

Every notebook exports its results as matching `.json` and `.csv` files under a `results/` (or `Results/`) folder next to it, e.g. `bug_enhancement_report.csv`, `bug_title_report.csv`, `advisory_enhancement_report.csv`. TianoForge's per-task and integrated runs additionally include `metrics_*.csv` files and `runtime_summary.csv` for benchmarking across models and runs (`RUN1`–`RUN3`).

---
## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to
propose changes, coding/notebook conventions, and how to report bugs.

## Security

If you discover a security issue (e.g. leaked credentials, unsafe handling
of fetched data, prompt-injection risk), please do **not** open a public
issue. See [SECURITY.md](SECURITY.md) for how to report it privately.

## License

This project is licensed under the terms of the LICENSE file included in this repository.

## Acknowledgments

This material is based upon work supported by the U.S. National Science Foundation (NSF) under Grant No. 2534021. In preparing this work, generative AI models and tools, including GPT and Claude models, were used to assist with generating and revising content, including code and text.
