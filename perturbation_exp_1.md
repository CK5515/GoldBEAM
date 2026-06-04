# Evaluation Suite: In Silico Chromatin Architecture Perturbation

This directory tracks real-time progress, biological validation milestones, and interpretability metrics for GoldBEAM's linear-time genomic foundation architecture.

## Experiment 1: In Silico Deletion (CTCF Knockout)

### Objective
To verify whether the model utilizes causal structural sequence syntax to map 3D chromatin folding, rather than over-indexing on broad regional textures. 

### Methodology
* **Target:** Locus Bin 107 (~438.3 kb), natively identified as the highest-intensity loop anchor within the 1-Megabase validation sequence.
* **Perturbation:** A virtual 10-kilobase segment centered at this peak was masked out using neutral tokens (`N`), completely arresting localized sequence-based signal propagation.

### Observations & Causal Validation
* **Contact Matrix Collapse:** The Difference Map reveals a pronounced, coordinate-specific loss of contact (visualized as the localized blue crosshair), demonstrating structural insulation collapse upon motif disruption.
* **Activation Drop:** The Multi-Scale Contact Head's Loop Dilation ($d_1$) spectrum shows a precipitous drop down to ~0.2, mapping directly to the disrupted insulator region.
* **Gradient Attribution:** Backpropagated attribution mechanics yield a highly isolated, high-magnitude nucleotide saliency spike precisely over the disrupted locus, confirming exact attribution to structural sequence elements.

---
*Note: Architectural code, factorizable kernel weights, and training hyperparameters remain closed-source and proprietary to protect filed IP.*
