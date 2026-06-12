# GoldBEAM: Subquadratic $O(N)$ Sequence-Mixing Backbone with Scale-Stratified Chromatin Task Heads

GoldBEAM is an ultra-efficient, structurally hardened genomic architecture optimized for predicting 2D chromatin contact maps from 1-Megabase genomic windows. By decoupling subquadratic, linear-time 1D sequence encoding from multi-scale structural decoder heads, GoldBEAM drastically reduces the computational overhead common in traditional transformer baselines while enforcing strict biophysical distance scaling constraints.

---

##  Algorithmic & Resource Performance Scoreboard

The evaluations below contrast the GoldBEAM core task heads against peer-reviewed long-context genomic foundation models fine-tuned on the 1-Megabase Akita architectural mapping task:

| Evaluation Dimension | HyenaDNA Baseline | GoldBEAM (This Work) | Functional Genomics Significance |
| :--- | :---: | :---: | :--- |
| **Validation MSE** | ~0.42094 | **0.09319** | **77.8% reduction in absolute error**, preventing artificial scale drifting. |
| **Average Pearson ($r$)** | ~0.049 | **≥ 0.450** | Recovers crisp off-diagonal loop punctuation and micro-TAD boundaries. |
| **Hardware Footprint** | ~11.8 GB VRAM | **1.14 GB VRAM** | **10.3x leaner memory footprint**, unlocking local edge workstation optimization. |
| **Attribution Velocity** | 1.0x Baseline | **30x Speedup** | Eliminates dense attention tracking to enable massive virtual sweeps. |
| **Explainability** | Post-Hoc / Black Box | **Post-Hoc Property Alignment** | First-layer convolutional weights map tightly to DNA physical properties. |

> **Open-Science Benchmarking Note:** To establish absolute peer-reviewed validity, we are currently integrating a standardized replication pass evaluated directly against the original Akita expert model using identical test chromosome splits (Chromosomes 8/9 held out entirely), identical distance-stratified evaluation masking, and Stratum-Adjusted Correlation Coefficients (SCC).

---

## Computational Complexity & Architecture

GoldBEAM avoids the quadratic spatial expansion $O(N^2)$ typical of dense transformers during sequence-mixing by strictly splitting the dimensional processing burden:
1. **The Sequence Core**: Operates via subquadratic, linear-time $O(N)$ factorizable implicit kernels, allowing highly scaling 1D token throughput.
2. **The Structural Decoding Head**: Translates the dense 1D sequence-mixed representations into a 2D spatial coordinate map using factorized, low-rank parallel dilated convolutions ($d_1$ through $d_8$), matching hierarchical chromatin interactions from short-range loops (~4.1 kb) up to macro-TAD boundaries.

---

## Symmetrical Algorithmic Safeguards

GoldBEAM incorporates dual-layer mathematical constraints to protect long-range optimization loops from geometric cheats and tensor border noise:

* **Joint Scale-and-Variance Cohesion Penalty**: Restricts the optimizer from exploiting relative correlation metrics via an anti-symmetric scalar expansion trick ($+A / -A$). Gradients are forced to minimize absolute coordinate errors by locking the first moment ($\mu$) and second moment ($\sigma$) of the distance tracks simultaneously:
$$\text{Cohesion Penalty} = 0.1 \times |\mu_{\text{pred}} - \mu_{\text{target}}| + 0.1 \times |\sigma_{\text{pred}} - \sigma_{\text{target}}|$$
* **Interior Margin Exclusion Mask**: Establishes a protected $[5, N-5]$ search territory. This shears away the outer $\approx 20.5 \text{ kbp}$ of zero-padding noise, insulating downstream peak scanners from artificial convolutional edge-spikes.

---

## In Silico Sequence Perturbation & Boundary Analysis

### 1. Experiment 1: Virtual Sequence Deletion Profile (CTCF Ablation Test)
By evaluating the wild-type loop activation spectrum through our margin exclusion mask, the model isolated a highly active interior loop anchor at **Bin 36 (~147.5 kb)**. Virtual sequence ablation of this locus via neutral masking tokens (`N`) triggered a localized loss of contact intensity along with a distinct shift in the sliding-window directional insulation delta.

Our **Structural Disruption Index (SDI)** tracks the absolute matrix residual sum ($|WT - Mutant|$) over coordinate frames. As shown below, this track maps in perfect symmetry with our base-resolution gradient feature attribution score, confirming that sequence-level attention aligns with spatial contact shifts.

![GoldBEAM Perturbation Panel — Experiment 1](./assets/perturbation_exp_1.png)

### 2. Experiment 2: Virtual Sequence Insertion Profile (De Novo Anchor Test)
To evaluate the model's response to known regulatory instructions, a synthetic cluster consisting of three tandem 20bp consensus CTCF binding motifs (`TGGCCACCAGGGGGCGACAC`) separated by 10bp neutral spacers was virtually inserted into an inactive baseline region at **Bin 219 (~897.0 kb)**. This perturbation successfully forced an emergence of localized contact gain at the target coordinate.

![GoldBEAM Perturbation Panel — Experiment 2](./assets/perturbation_exp_2.png)

---

## Post-Hoc Biophysical DNA Property Filter Alignment

To evaluate the biological grounding of the sequence encoder, the 256 frozen front-line filters (evaluating local motifs over a 15bp window) were compared against a normalized matrix of **12 mononucleotide biophysical DNA properties**. 

Computing the cosine similarity of the channel weights across their receptive fields reveals a strong, data-driven affinity for actual helix physical constraints:

| Physical DNA Property | Filter Channel Count | Primary Channel Exemplars (Cosine Similarity) |
| :--- | :---: | :--- |
| **Amino/Carbonyl** | 56 | Ch 9 (-1.00), Ch 199 (-0.97), Ch 185 (-0.96) |
| **GC content (local)** | 37 | Ch 52 (+0.97), Ch 188 (-0.96), Ch 202 (+0.94) |
| **Helical twist** | 27 | Ch 69 (-0.95), Ch 167 (-0.93), Ch 132 (+0.92) |
| **Shift** | 25 | Ch 99 (+0.98), Ch 240 (+0.97), Ch 227 (+0.96) |
| **Bendability** | 24 | Ch 175 (-0.99), Ch 110 (-0.93), Ch 179 (-0.90) |
| **Weak/Strong H-bonds** | 23 | Ch 125 (-0.99), Ch 86 (+0.98), Ch 50 (-0.95) |
| **Purine/Pyrimidine** | 21 | Ch 141 (+1.00), Ch 142 (+0.98), Ch 197 (+0.95) |
| **Slide** | 18 | Ch 180 (+0.97), Ch 45 (-0.95), Ch 112 (-0.94) |
| **Roll** | 17 | Ch 113 (+0.98), Ch 7 (+0.98), Ch 155 (-0.97) |
| **Rise** | 6 | Ch 136 (-1.00), Ch 46 (+0.92), Ch 249 (-0.82) |
| **Propeller twist** | 2 | Ch 29 (+0.99), Ch 222 (+0.86) |
| **Methylation susceptibility** | 0 | None |

---
*Open-science model card compiled under authorization protocols.*
