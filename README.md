# RGTFormer: Categorical Gated Transformer and Relational Graph Convolutional Network for Predicting Mutation-Associated Multi-Drug Resistance in *Mycobacterium tuberculosis*

RGTFormer: A novel deep learning model fusing Relational Graph Convolutional Networks (RGCN) + Categorical Gated Transformer for predicting mutation-driven multi-drug resistance in Mycobacterium tuberculosis. Achieves 98.67% accuracy on independent test set. Covers rpoB, katG, inhA, pncA, gyrA, gyrB genes.

<img src="assets/banner.jpg" alt="RGTFormer Banner" width="100%"/>

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

## Highlights

- First model to fuse **relational mutation graphs** (RGCN) with **categorical sequence learning** (Transformer) for TB.
- Interpretable attention-based fusion of structural + genomic features.
- Tested on **753 curated mutations** across **6 resistance genes**: `rpoB`, `katG`, `inhA`, `pncA`, `gyrA`, `gyrB`.
- Ready for clinical deployment — lightweight & generalizable.

---

## Model Architecture

<img src="figures/architecture.jpg" alt="RGTFormer Architecture" width="100%"/>

**Two parallel branches**:
- **RGCN**: Captures relational dependencies between mutations (physicochemical + co-occurrence edges)
- **CGT**: Gated attention over categorical features (residue type, secondary structure)

Outputs fused via learnable attention → binary classification (Resistant / Susceptible)

---

## Dataset Summary (Table I)

| Drug              | Gene  | TBDReaMDB | GMTV | Total | Resistant | Susceptible |
|-------------------|-------|-----------|------|-------|-----------|-------------|
| Rifampin          | rpoB  | 134       | 198  | 114   | 50        | 64          |
| Isoniazid         | inhA  | 13        | 30   | 27    | 10        | 17          |
| Isoniazid         | katG  | 273       | 83   | 250   | 135       | 115         |
| Pyrazinamide      | pncA  | 278       | 137  | 241   | 139       | 102         |
| Fluoroquinolones  | gyrA  | 17        | 112  | 72    | 31        | 41          |
| Fluoroquinolones  | gyrB  | 18        | 72   | 49    | 28        | 21          |

*Source: TBDReaMDB + GMTV databases*

---

## Figures

| Figure | Description |
|--------|-----------|
| <img src="figures/workflow.jpg" width="300"/> | **Fig 1.** Overall workflow: data → features → RGTFormer → prediction |
| <img src="figures/architecture.jpg" width="300"/> | **Fig 2.** Detailed RGTFormer architecture (RGCN + CGT + fusion) |
| <img src="figures/cgt.jpg" width="300"/> | **Fig 3.** Categorical Gated Transformer (CGT) module internals |
| <img src="figures/rgc.jpg" width="300"/> | **Fig 4.** RGCN module internals |

---

## Citation

```bibtex
@article{joshi2025rgtformer,
  title={RGTFormer: Categorical Gated Transformer and Relational Graph Convolutional Network for Predicting Mutation-Associated Multi-Drug Resistance in Mycobacterium tuberculosis},
  author={Joshi, Rakesh Chandra and Dereddy, Hitesh Reddy and Mukhopadhyay, Sandip and Burget, Radim and Dutta, Malay Kishore},
  year={2025},
  publisher={IEEE/ACM Transactions on Computational Biology and Bioinformatics (under review)}
}
