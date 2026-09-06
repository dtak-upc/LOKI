# Relationship Clustering

This report compares Qwen and LOKI on Relationship Clustering rather than the old pair-label proxy surface.

- LOKI uses its stored cluster artifacts: raw pair-cluster purity / oracle-pair F1 plus cluster-label metrics from the batch resume-state.
- Qwen is reconstructed as synthetic predicted clusters by grouping GT-matched predicted pairs into one bucket per predicted relation type within each admission.
- Primary comparator: raw pair-cluster purity and raw oracle-pair F1. Cluster-label metrics are secondary for prompts because the synthetic cluster identity is derived from the predicted relation type itself.
- This is still conditional on GT-matched predicted pairs; it is a clustering-task comparison, not full end-to-end retrieval evaluation.

## Summary

| System | Scope | Admissions | GT-matched pairs | Clusters | Raw purity | Raw oracle P/R/F1 | Cluster-label macro P/R/F1 | Corpus cluster-label P/R/F1 | Cluster accuracy |
| --- | --- | ---: | ---: | ---: | ---: | --- | --- | --- | ---: |
| LOKI+GPT-OSS 20B | full | 378 | 2730 | 2018 | 0.996 | 0.982 / 0.486 / 0.627 | 0.755 / 0.755 / 0.742 | 0.84 / 0.84 / 0.84 | 0.84 |
| LOKI+Qwen-3.6 | full | 380 | 2903 | 2113 | 0.995 | 0.982 / 0.515 / 0.652 | 0.75 / 0.732 / 0.726 | 0.848 / 0.807 / 0.827 | 0.807 |
| Qwen-3.7 | full | 381 | 4149 | 548 | 0.998 | 0.997 / 0.717 / 0.817 | 0.966 / 0.963 / 0.964 | 0.958 / 0.958 / 0.958 | 0.958 |
| LOKI+GPT-OSS 20B | matched_to_Qwen-3.7 | 377 | 2726 | 2014 | 0.996 | 0.982 / 0.487 / 0.627 | 0.755 / 0.756 / 0.743 | 0.841 / 0.841 / 0.841 | 0.841 |
| Qwen-3.7 | matched_to_LOKI+GPT-OSS 20B | 377 | 4117 | 544 | 0.998 | 0.997 / 0.716 / 0.817 | 0.966 / 0.963 / 0.964 | 0.958 / 0.958 / 0.958 | 0.958 |
| LOKI+Qwen-3.6 | matched_to_Qwen-3.7 | 379 | 2899 | 2109 | 0.995 | 0.982 / 0.516 / 0.653 | 0.75 / 0.733 / 0.727 | 0.848 / 0.807 / 0.827 | 0.807 |
| Qwen-3.7 | matched_to_LOKI+Qwen-3.6 | 379 | 4127 | 546 | 0.998 | 0.997 / 0.716 / 0.817 | 0.966 / 0.963 / 0.964 | 0.958 / 0.958 / 0.958 | 0.958 |
| Qwen3.6-Local | full | 381 | 3103 | 523 | 0.998 | 0.993 / 0.54 / 0.678 | 0.932 / 0.929 / 0.93 | 0.92 / 0.92 / 0.92 | 0.92 |
| LOKI+GPT-OSS 20B | matched_to_Qwen3.6-Local | 377 | 2726 | 2014 | 0.996 | 0.982 / 0.487 / 0.627 | 0.755 / 0.756 / 0.743 | 0.841 / 0.841 / 0.841 | 0.841 |
| Qwen3.6-Local | matched_to_LOKI+GPT-OSS 20B | 377 | 3078 | 519 | 0.998 | 0.993 / 0.539 / 0.677 | 0.931 / 0.928 / 0.929 | 0.919 / 0.919 / 0.919 | 0.919 |
| LOKI+Qwen-3.6 | matched_to_Qwen3.6-Local | 379 | 2899 | 2109 | 0.995 | 0.982 / 0.516 / 0.653 | 0.75 / 0.733 / 0.727 | 0.848 / 0.807 / 0.827 | 0.807 |
| Qwen3.6-Local | matched_to_LOKI+Qwen-3.6 | 379 | 3085 | 521 | 0.998 | 0.993 / 0.539 / 0.677 | 0.931 / 0.928 / 0.929 | 0.919 / 0.919 / 0.919 | 0.919 |
