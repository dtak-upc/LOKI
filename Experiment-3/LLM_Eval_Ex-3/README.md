# Experiment 3: Evaluation Suite & Materialization Trace

This directory contains the core evaluation pipeline for **Experiment 3**, benchmarking **LOKI** against direct frontier large language model prompting baselines on the MIMIC-IV dataset (382 admissions).

The workflow is organized into two sections:
1. **Primary Evaluation Pipeline (`compare_type_bucket_clusters.py`)**: Executes directly on pre-materialized prediction artifacts under `Pred/` against ground truth annotations in `GT/`, generating all benchmark tables, technical reports, and publication figures under `../#Results/`.
2. **End-to-End Materialization Trace (`materialize_joins.py`)**: An optional reproducibility trace detailing how candidate join paths and relation clusters were originally materialized from raw MIMIC-IV data to populate `Pred/`.

---

## Part 1: Primary Evaluation Pipeline (`compare_type_bucket_clusters.py`)

### 1.1 Overview & Purpose

`compare_type_bucket_clusters.py` is the primary benchmark evaluation engine for Experiment 3. It performs cross-model comparative evaluation across all 382 admissions:
- **Relationship-Type Table Materialization:** Evaluates discrete relational table synthesis, entity-pair resolution accuracy, and exact confusion counts ($\text{TP}, \text{FP}, \text{FN}$).
- **Cluster Partition Diagnostics:** Evaluates structural cluster partition metrics including Label Accuracy, Cluster Purity, and Adjusted Rand Index (ARI).
- **Matched-Support Cohort Analysis:** Assesses system robustness on strictly matched admission subsets (`matched_all`) and high-complexity admissions with multiple co-occurring relationship types (`matched_multitype_overlap`).
- **Figure Generation:** Renders all evaluation plots in both high-resolution PNG and publication-ready vector PDF formats.

---

### 1.2 Quick Start (One-Click Execution)

To reproduce all benchmark tables, reports, and visualization figures directly from the provided prediction artifacts:

```bash
cd LLM_Eval_Ex-3
python3 compare_type_bucket_clusters.py
```

All input paths, ground-truth references, and output directories resolve automatically to project defaults. No command-line flags are required.

---

### 1.3 Input Data Layout

The evaluation script processes reference ground truth and prediction outputs structured as follows:

```
LLM_Eval_Ex-3/
├── GT/
│   └── Annotated_Test.json              # Ground truth annotations across 382 MIMIC-IV admissions
└── Pred/
    ├── Qwen-3.7.json                    # Direct prompt baseline: Frontier API outputs
    ├── Qwen3.6-Local.json               # Direct prompt baseline: Local model outputs
    └── LOKI/
        ├── LOKI_Batch_mimic_GPT_OSS/    # LOKI + GPT-OSS 20B batch results & resume state
        │   ├── materialized_batch_results_mimic.csv
        │   └── materialized_batch_resume_state_mimic.json
        └── Loki_Batch_mimic_Qwen-3.6/   # LOKI + Qwen-3.6 batch results & resume state
            ├── materialized_batch_results_mimic.csv
            └── materialized_batch_resume_state_mimic.json
```

---

### 1.4 Generated Output Artifacts

Running `compare_type_bucket_clusters.py` automatically updates and generates the following artifacts under `../#Results/`:

#### 1. Metric Summaries & CSV Datasets
- **`../#Results/relationship_clustering_summary.csv`:** Cross-system aggregate scores across full and matched cohorts.
- **`../#Results/relationship_clustering_per_admission.csv`:** Granular admission-by-admission performance records.
- **`../#Results/relationship_clustering_fairness_summary.csv`:** Cohort-matched scores on shared-support subsets.
- **`../#Results/relationship_clustering_dashboard_summary.csv`:** Summary-level dashboard metrics.
- **`../#Results/relationship_clustering_dashboard_per_admission.csv`:** Per-admission dashboard metrics.

#### 2. Detailed Technical Reports
- **`../#Results/relationship_table_report.md`:** Primary physical table materialization evaluation, including table-level and pair-level contingency matrices ($\text{TP}, \text{FP}, \text{FN}$).
- **`../#Results/relationship_clustering_fairness_report.md`:** Detailed matched-support analysis across simple and multi-type admissions.

#### 3. Publication Visualizations (`../#Results/Visualizations/relationship_clustering/`)
All figures are saved concurrently as PNG and vector PDF:
- **`all_models_main_comparison_metrics.png` (+ `.pdf`):** Dual-panel primary comparison chart (Cluster Quality vs. Materialization scores).
- **`all_models_semantic_integration_slices.png` (+ `.pdf`):** Materialization performance across matched cohorts.
- **`all_models_relationship_clustering_metrics.png` (+ `.pdf`):** Standalone structural diagnostics (Accuracy, Purity, ARI).
- **`all_models_relationship_clustering_slices.png` (+ `.pdf`):** Cluster quality diagnostics across matched subsets.
- **`all_models_compute_cost_half_circle.png` (+ `.pdf`):** Compute runtime and token reduction trade-off visualization.
- **`all_models_data_quality.png`:** Relational schema compliance and key integrity diagnostics.
- **`loki_per_admission_relationship_clustering_quality.png`:** Admission-level scatter distributions.
- Model-specific cluster count distributions and relative F1 delta plots.

---

### 1.5 Command-Line Options & Configuration

For custom evaluation runs (e.g., evaluating alternative prediction files or routing outputs to custom directories), `compare_type_bucket_clusters.py` provides the following CLI flags:

| Argument | Default | Description |
|---|---|---|
| `--gt-file` | `GT/Annotated_Test.json` | Path to the ground-truth annotation JSON file. |
| `--pred-dir` | `Pred` | Directory containing direct prompting prediction JSON files. |
| `--loki-gpt-resume` | `Pred/LOKI/LOKI_Batch_mimic_GPT_OSS/...` | Path to LOKI + GPT-OSS 20B batch resume state JSON. |
| `--loki-gpt-results-csv` | `Pred/LOKI/LOKI_Batch_mimic_GPT_OSS/...` | Path to LOKI + GPT-OSS 20B batch results CSV. |
| `--loki-qwen-resume` | `Pred/LOKI/Loki_Batch_mimic_Qwen-3.6/...` | Path to LOKI + Qwen-3.6 batch resume state JSON. |
| `--loki-qwen-results-csv` | `Pred/LOKI/Loki_Batch_mimic_Qwen-3.6/...` | Path to LOKI + Qwen-3.6 batch results CSV. |
| `--output-dir` | `../#Results` | Target directory for generated CSV tables and markdown reports. |
| `--viz-dir` | `../#Results/Visualizations/relationship_clustering` | Target directory for generated PNG and PDF visualization figures. |

---

## Part 2: End-to-End Materialization Trace (`materialize_joins.py`)

*Note: Executing Part 2 is optional. The evaluation suite in Part 1 functions directly on the provided prediction outputs without requiring local LLM inference or GPU hardware.*

### 2.1 Pipeline Overview

The LOKI materialization pipeline (`materialize_joins.py`) extracts candidate multi-hop join paths between clinical entities (diagnoses and medications), computes dense contextual embeddings, performs topological density clustering via HDBSCAN, and assigns semantic relationship types (`TREATS`, `ADVERSE_EFFECT`, `DISCONTINUED`, `CONTRAINDICATED`, `NEGATIVE`).

---

### 2.2 Execution Modes

#### 1. Full Dataset Batch Execution (382 Admissions)
To run end-to-end batch materialization across all admissions in the evaluation cohort:

```bash
python materialize_joins.py --dataset mimic --run_all_admissions --batch_progress_every 1
```

By default, the script connects to a local LM Studio endpoint hosting the configured open-weight model.

#### 2. Model Backend Selection
- **LOKI + GPT-OSS 20B:**
  ```bash
  python materialize_joins.py --dataset mimic --run_all_admissions \
    --cluster_label_backend lmstudio \
    --llm_model openai/gpt-oss-20b
  ```
- **LOKI + Qwen-3.6-35B:**
  ```bash
  python materialize_joins.py --dataset mimic --run_all_admissions \
    --cluster_label_backend lmstudio \
    --llm_model qwen3.6-35b-a3b
  ```

#### 3. Single-Admission Verification (Smoke Testing)
To verify the materialization pipeline on a single admission without running the full cohort:

```bash
python materialize_joins.py --single_admission --dataset mimic --admission_id 20393363
```
Outputs are written to `Batch_Materialization/loki_run_20393363/` containing per-admission JSON, CSV, audit markdown, and diagnostic figures.

---

### 2.3 Resuming Interrupted Batches & Fault Tolerance

For large batch runs over hundreds of admissions, network drops or server timeouts can be handled without data loss:

```bash
python materialize_joins.py --dataset mimic --run_all_admissions --batch_progress_every 1 --resume --llm_retry_attempts 8
```
- `--resume`: Reloads the existing `materialized_batch_results_mimic.csv`, skips previously completed admissions, and resumes processing from the first unfinished admission.
- `--llm_retry_attempts <N>`: Sets the maximum retry budget per LLM request before raising an error.

---

### 2.4 Validated Algorithmic Defaults

The pipeline incorporates the following validated defaults:

| Component | Flag / Setting | Description |
|---|---|---|
| **Joint Contextual Encoding** | `--pair_embedding_mode contextual_sentence_average` | Computes contextual sentence averages for candidate entity pairs. |
| **Evidence Reranking** | `--use_cross_encoder` | Cross-encoder per-pair sentence reranking (Phase D.5). |
| **Heuristic Pair Filtering** | `--ce_pair_filter_mode combined --ce_pair_filter_quantile 0.25` | Prunes candidate pairs weak under both cross-encoder and heuristic signals (Phase D.6). |
| **Topological Clustering** | `--llm_hdbscan --hdbscan_min_cluster_size 4` | Groups join paths by dense vector connectivity while preserving fine-grained clusters. |
| **Cluster Refinement** | `--cluster_refine_semantic_subsplit` | Sub-splits same-label clusters along semantic boundaries. |
| **Evidence Voting** | `--cluster_refine_llm_per_path_vote` | Employs per-path evidence voting during pair refinement. |
| **Gated Path Splitting** | `--cluster_refine_path_subsplit` | Partitions mixed same-pair evidence when support thresholds are satisfied. |
| **Negative Suppression** | `--suppress_negative_clusters` | Suppresses non-annotated negative relationship clusters post-refinement. |

---

### 2.5 Mapping Materialization Outputs to `Pred/`

When batch runs complete:
1. Copy `materialized_batch_results_mimic.csv` and `materialized_batch_resume_state_mimic.json` into the corresponding `Pred/LOKI/` directory:
   - For GPT-OSS 20B: `Pred/LOKI/LOKI_Batch_mimic_GPT_OSS/`
   - For Qwen-3.6: `Pred/LOKI/Loki_Batch_mimic_Qwen-3.6/`
2. Direct prompting baselines (`Pred/Qwen-3.7.json` and `Pred/Qwen3.6-Local.json`) are compiled from single-pass prompting responses across the 382 admission prompt files.
3. Run `python3 compare_type_bucket_clusters.py` as described in Part 1 to regenerate all evaluation tables, reports, and visualization artifacts.
