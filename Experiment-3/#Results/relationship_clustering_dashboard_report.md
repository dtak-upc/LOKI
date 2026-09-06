# Relationship Clustering Dashboards

These summaries use LOKI's original per-admission dashboard aggregation semantics.

- LOKI is loaded directly from the original batch-results CSV.
- Prompt systems are reconstructed into the same per-admission row shape before aggregation.
- Admissions with zero evaluated clusters contribute 0.0 to macro P/R/F1 and mean accuracy, matching the original dashboard behavior.
- Prompt cluster silhouette is unavailable because the prompt JSON does not expose the embedding space used by LOKI's silhouette computation.

## Summary

| System | Admissions | Pred pairs | GT pairs | Final clusters | Evaluated clusters | Correct clusters | Macro P | Macro R | Macro F1 | Mean accuracy | Raw purity | Raw oracle F1 | Cluster ARI | Cluster silhouette |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| LOKI+GPT-OSS 20B | 382 | 23444 | 5456 | 7671 | 2018 | 1696 | 0.747 | 0.747 | 0.734 | 0.846 | 0.996 | 0.627 | 0.806 | 0.501 |
| LOKI+Qwen-3.6 | 382 | 36840 | 5456 | 10554 | 2113 | 1705 | 0.746 | 0.729 | 0.722 | 0.817 | 0.995 | 0.652 | 0.858 | 0.534 |
| Qwen-3.7 | 382 | 6511 | 5456 | 595 | 548 | 525 | 0.964 | 0.961 | 0.962 | 0.97 | 0.998 | 0.817 | 0.984 | n/a |
| Qwen3.6-Local | 382 | 3867 | 5456 | 557 | 523 | 481 | 0.929 | 0.926 | 0.927 | 0.942 | 0.998 | 0.678 | 0.984 | n/a |
