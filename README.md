# RGTFormer: Categorical Gated Transformer and Relational Graph Convolutional Network for Predicting Mutation-Associated Multi-Drug Resistance in *Mycobacterium tuberculosis*

RGTFormer: A novel deep learning model fusing Relational Graph Convolutional Networks (RGCN) + Categorical Gated Transformer for predicting mutation-driven multi-drug resistance in Mycobacterium tuberculosis. Achieves 98.67% accuracy on independent test set. Covers rpoB, katG, inhA, pncA, gyrA, gyrB genes.

**Authors**: Rakesh Chandra Joshi¹, Hitesh Reddy Dereddy², Sandip Mukhopadhyay³, Radim Burget⁴, Malay Kishore Dutta¹*  
<sup>1</sup>Amity University Noida, India • <sup>2</sup>Amity University Noida • <sup>3</sup>ICMR-NIRTR, Kolkata • <sup>4</sup>Brno University of Technology, Czech Republic  
*Corresponding: malaykishoredutta@gmail.com

---

## Abstract

Tuberculosis (TB) remains a critical global health concern due to multi-drug-resistant strains. We present **RGTFormer**, a novel deep learning architecture combining a **Categorical Gated Transformer (CGT)** with a **Relational Graph Convolutional Network (RGCN)** to predict mutation-driven drug resistance in *Mycobacterium tuberculosis*.

**Key Results**  
- **98.67%** accuracy on independent test set  
- **97.15%** average accuracy (10-fold CV)  
- Outperforms CNNs, GNNs, and classical ML baselines  

---

## Key Contributions

1. First model fusing **relational mutation graphs (RGCN)** + **categorical sequence learning (Transformer)** for TB resistance.
2. Novel **Categorical Gated Transformer (CGT)** with GLU + soft attention masking.
3. Multi-gene, multi-drug prediction across **6 genes**: `rpoB`, `katG`, `inhA`, `pncA`, `gyrA`, `gyrB`.
4. Biologically grounded labeling using **ΔΔG** (thermodynamic stability).

---

## Dataset Composition (Table I)

| Drug              | Gene  | TBDReaMDB | GMTV | Final Variations | Resistant | Susceptible |
|-------------------|-------|-----------|------|------------------|-----------|-------------|
| Rifampin          | rpoB  | 134       | 198  | 114              | 50        | 64          |
| Isoniazid         | inhA  | 13        | 30   | 27               | 10        | 17          |
| Isoniazid         | katG  | 273       | 83   | 250              | 135       | 115         |
| Pyrazinamide      | pncA  | 278       | 137  | 241              | 139       | 102         |
| Fluoroquinolones  | gyrA  | 17        | 112  | 72               | 31        | 41          |
| Fluoroquinolones  | gyrB  | 18        | 72   | 49               | 28        | 21          |
| **Total**         |       |           |      | **753**          | **393**   | **360**     |

---

## Numerical Features (6 total)

| Feature                    | Description |
|----------------------------|-----------|
| Molecular Weight           | Mass of side chain |
| Van der Waals Volume       | Spatial size |
| Polarity                   | Ability to form H-bonds |
| Isoelectric Point (pI)     | Net charge pH |
| Hydrophobicity             | Preference for hydrophobic core |
| Normalized ASA             | Solvent exposure |

All computed as **Δ(mutant – wild-type)** and z-score normalized.

---

## Categorical Features (3 total)

| Feature                | Categories |
|------------------------|----------|
| Residue Type (WT/Mut)  | Hydrophobic, Aromatic, Polar, Charged |
| Secondary Structure    | Helix, Sheet, Turn, Coil |

Label-encoded for CGT module.

---

## Overall Workflow (Figure 1)

<img src="figures/workflow.jpg" alt="Figure 1: Overall Workflow" width="100%"/>

---

## RGTFormer Architecture (Figure 2)

<img src="figures/architecture.jpg" alt="Figure 2: RGTFormer Architecture" width="100%"/>

**Two parallel branches** → **Attention Fusion** → **Classification**

---

## Categorical Gated Transformer (CGT) Module (Figure 3)

<img src="figures/cgt.jpg" alt="Figure 3: CGT Module" width="100%"/>

### CGT Equations

$$
z_t = \text{ReLU}(W_z x_{\text{cat}} + b_z), \quad \text{GLU}(z_t, g_t) = z_t \cdot \sigma(g_t) \quad (1)
$$

$$
m_t = \text{Softmax}(W_m x_{\text{cat}} + b_m) \quad (2)
$$

$$
x_{\text{masked},t} = x_{\text{cat}} \odot m_t \quad (3)
$$

$$
h_{\text{CGT}} = \sum_{j=1}^{N} \gamma_j h_j, \quad \sum \gamma_j = 1 \quad (4)
$$

---

## Performance on Independent Test Set (Table II)

| Model                | Accuracy | Precision | Recall | F1-Score |
|----------------------|----------|-----------|--------|----------|
| **RGTFormer (Ours)** | **98.67%** | **98.71%** | **98.62%** | **98.66%** |
| RGCN only            | 94.32%   | 94.10%    | 94.55% | 94.32%   |
| CGT only             | 93.87%   | 93.65%    | 94.10% | 93.87%   |
| CNN                  | 92.10%   | 91.88%    | 92.33% | 92.10%   |
| GAT                  | 91.45%   | 91.20%    | 91.70% | 91.45%   |
| SVM                  | 85.33%   | 85.10%    | 85.55% | 85.32%   |
| Random Forest        | 84.97%   | 84.75%    | 85.20% | 84.97%   |

---

## Per-Gene Performance (10-fold CV) (Table III)

| Gene  | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| rpoB  | 99.12%   | 99.00%    | 99.25% | 99.12%   |
| katG  | 98.80%   | 98.70%    | 98.90% | 98.80%   |
| inhA  | 96.30%   | 96.00%    | 96.60% | 96.30%   |
| pncA  | 97.51%   | 97.40%    | 97.62% | 97.51%   |
| gyrA  | 98.61%   | 98.50%    | 98.72% | 98.61%   |
| gyrB  | 97.96%   | 97.80%    | 98.12% | 97.96%   |
| **Avg**| **97.15%**|           |        |          |

---

## Ablation Study (Table IV)

| Variant                        | Accuracy |
|--------------------------------|----------|
| Full RGTFormer                 | **98.67%** |
| w/o RGCN                       | 94.32% (-4.35%) |
| w/o CGT                        | 93.87% (-4.80%) |
| w/o Attention Fusion           | 95.46% (-3.21%) |
| w/o Categorical Encoding       | 93.55% (-5.12%) |
| w/o Graph Edges                | 91.79% (-6.88%) |

---

## Model Efficiency (Table V)

| Model         | Parameters | Training Time (10-fold) | Inference (per sample) |
|---------------|------------|--------------------------|------------------------|
| RGTFormer     | **420K**   | **18.2 min**             | **0.84 ms**            |
| CNN           | 1.2M       | 32.1 min                 | 1.67 ms                |
| GAT           | 890K       | 41.5 min                 | 2.31 ms                |

---
