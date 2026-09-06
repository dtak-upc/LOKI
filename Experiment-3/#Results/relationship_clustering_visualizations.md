# Relationship Clustering and Typed Relationship Materialization Visualizations

This page collects the default Relationship Clustering and Typed Relationship Materialization plots for quick inspection.
Relationship Clustering figures focus on cluster-quality diagnostics, while Typed Relationship Materialization figures isolate the comparable type-assignment P/R/F1 scores.
Shared comparison figures aggregate every prediction JSON under Pred/, and every figure is saved as both PNG and PDF with the same basename.
Model-specific diagnostics remain separate where the admission alignment itself depends on the prediction file.

## LOKI+GPT-OSS 20B

- Original full-dashboard macro F1: 0.734.
- Original full-dashboard mean accuracy: 0.846.
- Original final predicted clusters: 7671 across 382 admissions.

![LOKI+GPT-OSS 20B Relationship Clustering dashboard](Visualizations/relationship_clustering/LOKI_GPT-OSS_20B_relationship_clustering_dashboard.png)

## LOKI+Qwen-3.6

- Original full-dashboard macro F1: 0.722.
- Original full-dashboard mean accuracy: 0.817.
- Original final predicted clusters: 10554 across 382 admissions.

![LOKI+Qwen-3.6 Relationship Clustering dashboard](Visualizations/relationship_clustering/LOKI_Qwen-3.6_relationship_clustering_dashboard.png)

## LOKI Per-Admission Cluster Quality

- This companion figure keeps the original per-admission scatter style and combines the two LOKI labelers into one figure as separate plots.
- The two plots show per-admission cluster precision and recall, with one shared colorbar for cluster macro F1.

![LOKI per-admission cluster quality](Visualizations/relationship_clustering/loki_per_admission_relationship_clustering_quality.png)

## Combined Comparison

- The main paper-ready comparison figure places Relationship Clustering Quality and Typed Relationship Materialization side by side in one compact layout with a shared legend.

![Main comparison metrics for all models](Visualizations/relationship_clustering/all_models_main_comparison_metrics.png)

### Typed Relationship Materialization

- These figures isolate the comparable type-assignment metrics only: raw-oracle pair P/R/F1 for prompt-only systems versus cluster-label macro P/R/F1 for LOKI.
- Overlap slices use one shared admission intersection across all plotted models, so the typed relationship materialization comparison is shown on exactly the same support.
- Models included: Qwen-3.7, Qwen3.6-Local.

![Typed Relationship Materialization metrics for all models](Visualizations/relationship_clustering/all_models_semantic_integration_metrics.png)

![Typed Relationship Materialization overlap slices for all models](Visualizations/relationship_clustering/all_models_semantic_integration_slices.png)

### Relationship Clustering Quality

- These figures keep the secondary clustering diagnostics separate from typed relationship materialization: label accuracy, cluster purity, and ARI.
- Prompt-only systems remain conservatively scaled by raw-oracle recall for these quality diagnostics, matching the existing comparison policy.

![Relationship Clustering quality metrics for all models](Visualizations/relationship_clustering/all_models_relationship_clustering_metrics.png)

![Relationship Clustering quality overlap slices for all models](Visualizations/relationship_clustering/all_models_relationship_clustering_slices.png)

### Resource Allocation & Compute Cost

- Average latency (seconds per admission) and token footprint (prompt vs completion tokens) for each model.

- The half-circle companion view compresses the radial design into four stacked execution profiles, preserving the inward token markers while saving horizontal space.

![Half-circle compute cost comparison for all models](Visualizations/relationship_clustering/all_models_compute_cost_half_circle.png)

- The broken-axis companion view separates LOKI's small preprocessing stages from the much larger labeling/runtime regime, while keeping token footprint in a dedicated lower band.

![Broken-axis compute cost comparison for all models](Visualizations/relationship_clustering/all_models_compute_cost_broken_axis.png)

- The side-by-side companion view separates latency and token footprint into two coordinated panels, keeping the latency scale logarithmic while showing token usage directly by model.

![Side-by-side compute cost comparison for all models](Visualizations/relationship_clustering/all_models_compute_cost_side_by_side.png)

- The flattened companion view stacks the LOKI pipeline stages into one bar per model, giving a simpler paper-friendly comparison across the four systems.

![Flattened compute cost comparison for all models](Visualizations/relationship_clustering/all_models_compute_cost_flat.png)

![Resource and Compute Cost for all models](Visualizations/relationship_clustering/all_models_compute_cost.png)

### Data Quality & Relational Integrity

- Relational integrity violations (Out-of-Bounds row/sentence references) and schema anomalies (dropped empty rows) across the corpus.

![Data Quality and Relational Integrity for all models](Visualizations/relationship_clustering/all_models_data_quality.png)

## Qwen-3.7

- Comparable P/R/F1 uses Qwen raw-oracle pair metrics against LOKI cluster-macro metrics: 0.997 / 0.717 / 0.817 for Qwen-3.7 vs 0.747 / 0.747 / 0.734 for LOKI (GPT-OSS).
- Secondary diagnostics scale Qwen by raw-oracle recall before comparing against LOKI: Accuracy=0.696 vs 0.846, Purity=0.715 vs 0.996, ARI=0.706 vs 0.806.
- Overlap slices use the same comparable mapping, with hard-slice F1 at 0.75 for Qwen-3.7 vs 0.545 for LOKI.

### Prompt Dashboard Diagnostic

![Relationship Clustering dashboard for Qwen-3.7](Visualizations/relationship_clustering/Qwen-3.7_relationship_clustering_dashboard.png)

### Cluster Count Diagnostic

![Relationship Clustering cluster counts for Qwen-3.7](Visualizations/relationship_clustering/Qwen-3.7_relationship_clustering_cluster_counts.png)

### Raw Oracle F1 Admission Deltas

![Relationship Clustering raw oracle F1 deltas for Qwen-3.7](Visualizations/relationship_clustering/Qwen-3.7_relationship_clustering_raw_oracle_f1_delta.png)

## Qwen3.6-Local

- Comparable P/R/F1 uses Qwen raw-oracle pair metrics against LOKI cluster-macro metrics: 0.993 / 0.54 / 0.678 for Qwen3.6-Local vs 0.747 / 0.747 / 0.734 for LOKI (GPT-OSS).
- Secondary diagnostics scale Qwen by raw-oracle recall before comparing against LOKI: Accuracy=0.509 vs 0.846, Purity=0.539 vs 0.996, ARI=0.532 vs 0.806.
- Overlap slices use the same comparable mapping, with hard-slice F1 at 0.601 for Qwen3.6-Local vs 0.547 for LOKI.

### Prompt Dashboard Diagnostic

![Relationship Clustering dashboard for Qwen3.6-Local](Visualizations/relationship_clustering/Qwen3.6-Local_relationship_clustering_dashboard.png)

### Cluster Count Diagnostic

![Relationship Clustering cluster counts for Qwen3.6-Local](Visualizations/relationship_clustering/Qwen3.6-Local_relationship_clustering_cluster_counts.png)

### Raw Oracle F1 Admission Deltas

![Relationship Clustering raw oracle F1 deltas for Qwen3.6-Local](Visualizations/relationship_clustering/Qwen3.6-Local_relationship_clustering_raw_oracle_f1_delta.png)

