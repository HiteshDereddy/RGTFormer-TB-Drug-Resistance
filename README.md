# RGTFormer: Categorical Gated Transformer + Relational Graph Convolutional Network  
### Predicting Mutation-Associated Multi-Drug Resistance in *Mycobacterium tuberculosis*

**Authors:**  
Rakesh Chandra Joshi¹, Hitesh Reddy Dereddy², Sandip Mukhopadhyay³, Radim Burget⁴, Malay Kishore Dutta¹*  
¹ Amity Centre for Artificial Intelligence, Amity University, Noida, India  
² Department of Artificial Intelligence, Amity School of Engineering & Technology, Amity University, Noida, India  
³ ICMR–National Institute for Research in Bacterial Infections, Kolkata, India  
⁴ Brno University of Technology, Czech Republic  
*Corresponding Author: malaykishoredutta@gmail.com*  

---

## 🧬 Overview
**RGTFormer** integrates a **Categorical Gated Transformer (CGT)** with a **Relational Graph Convolutional Network (RGCN)** to predict mutation-associated resistance to first- and second-line anti-tuberculosis (TB) drugs.  
It learns from both **sequence-based categorical** and **structure-based numerical** mutation descriptors to model multi-gene, multi-drug resistance with interpretability and efficiency.

---

## 📚 Abstract
Tuberculosis (TB), caused by *Mycobacterium tuberculosis*, remains a global health threat, worsened by multi-drug-resistant TB (MDR-TB). Resistance often stems from single-nucleotide mutations in key genes.  
RGTFormer fuses RGCN-based relational reasoning and Transformer-based categorical attention to predict resistance from mutation profiles.  
Evaluated on 753 curated mutations across six genes (*rpoB, katG, inhA, pncA, gyrA, gyrB*), the model achieved:

- **Independent-test accuracy:** 98.67 %  
- **10-fold CV accuracy:** 97.15 %  
- **Precision:** 100 % **Recall:** 97.37 % **F1:** 98.67 %

---

## 🧠 Key Contributions
- 🧩 **Hybrid architecture** combining relational (RGCN) and categorical (CGT) feature learning.  
- 🔬 Handles heterogeneous genomic features — physicochemical + residue/structure.  
- 🧮 Attention-based fusion ensures interpretability and robustness.  
- 🧫 Multi-gene, multi-drug generalization across six resistance genes.  
- 📈 Outperforms all classical ML and deep-learning baselines.  
- 💡 SHAP analysis reveals biologically meaningful feature contributions.

---

## 🧩 Dataset
| Gene | Drug | Variants | Resistant | Susceptible | Source |
|------|------|-----------|-----------|--------------|---------|
| rpoB | Rifampicin | 114 | 50 | 64 | TBDReaMDB + GMTV |
| katG | Isoniazid | 250 | 135 | 115 | TBDReaMDB + GMTV |
| inhA | Isoniazid | 27 | 10 | 17 | TBDReaMDB + GMTV |
| pncA | Pyrazinamide | 241 | 139 | 102 | TBDReaMDB + GMTV |
| gyrA | Fluoroquinolones | 72 | 31 | 41 | TBDReaMDB + GMTV |
| gyrB | Fluoroquinolones | 49 | 28 | 21 | TBDReaMDB + GMTV |

**Total:** 753 mutations **Labels:** ΔΔG < 0 → Resistant; ΔΔG ≥ 0 → Susceptible  

---

## ⚙️ Feature Engineering
**Numerical (z-score normalized mutant–wild-type differences):**
1. Molecular weight 2. Van der Waals volume 3. Polarity  
4. Isoelectric point 5. Hydrophobicity 6. Normalized ASA  

**Categorical:**
- Residue type (0 charged / 1 polar / 2 aromatic / 3 hydrophobic)  
- Secondary structure (1 helix / 2 sheet / 3 coil / 4 turn)

---

## 🧮 Methodology

### 🔗 Workflow
![Fig 1 – Workflow](figures/workflow.jpg)

### 🧠 Model Architecture
RGTFormer = RGCN + CGT + Attention Fusion  
![Fig 2 – Architecture](figures/architecture.jpg)

**RGCN layer**
\begin{equation*}
h_i^{(l+1)} = \sigma\!\left(\sum_{r\in R}\sum_{j\in N_i^r}\!\tfrac{1}{c_{i,r}}W_r^{(l)}h_j^{(l)}\right)
\end{equation*}

![Fig 3 – RGCN](figures/rgc.jpg)

**CGT (Gated Transformer)**
\[
z=ReLU(Wx+b),\quad GLU(z,g)=z\!\cdot\!\sigma(g)
\]
\[m=Softmax(Wx+b),\quad x'_i=x_i\odot m \]
![Fig 4 – CGT](figures/cgt.jpg)

**Fusion + Classification**
\[
h=\alpha h_{RGCN}+\beta h_{CGT},\;\alpha+\beta=1,\quad
y=Softmax(Wh+b)
\]

**Training:** Adam (β₁ = 0.9, β₂ = 0.999), 100 epochs, BCE loss, PyTorch on NVIDIA A100 (40 GB)

---

## 🧪 Experimental Results
| Variant | GNN Dim | Layers | LR | CGT Dim | Accuracy % | Precision % | Recall % | F1 % |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|
| V1 | 64 | 3 | 0.001 | 64 | 97.35 | 98.65 | 96.05 | 97.33 |
| **V2 (Best)** | **128** | **4** | **0.0005** | **128** | **98.67** | **100.0** | **97.37** | **98.67** |
| V3 | 32 | 2 | 0.002 | 32 | 94.70 | 97.22 | 92.11 | 94.59 |

### Ablation & Baselines
| Model | 10-Fold Acc % | Test Acc % | Prec | Rec | F1 |
|:--|--:|--:|--:|--:|--:|
| **RGTFormer Full** | **97.15** | **98.67** | 1.00 | 0.97 | 0.99 |
| RGCN only | 94.57 | 96.02 | 0.97 | 0.95 | 0.96 |
| CGT only | 67.81 | 68.87 | 0.69 | 0.68 | 0.69 |
| GCN + Vanilla Transformer | 65.36 | 66.89 | 0.72 | 0.55 | 0.63 |

### Classical ML Comparison
| SVM | RF | Extra Trees | XGBoost | KNN | MLP (Complex) | ANN | **RGTFormer** |
|--|--|--|--|--|--|--|--|
| 90.07 | 93.38 | 92.72 | 95.36 | 95.36 | 94.70 | 94.04 | **98.67** |

**ROC AUC = 0.9923 (149 / 151 correct)**  
| Confusion Matrix | ROC Curve |
|:--:|:--:|
| ![CM](results/cm.jpg) | ![ROC](results/roc.jpg) |

---

## 🧩 Explainability – SHAP
![Fig 6 – SHAP Importance](results/shap.jpg)  
Hydrophobicity ≫ Molecular Weight ≫ Isoelectric Point ≫ ASA as key features.

---

## 📊 Appendix Summary
| Feature | Mean | Std | Min | Max |
|--|--:|--:|--:|--:|
| Mol Weight | −0.0013 | 0.2989 | −1 | 0.77 |
| Volume | 0.005 | 0.2917 | −1 | 0.8 |
| Polarity | −0.0277 | 0.549 | −1 | 1 |
| pI | 0.0232 | 0.2956 | −0.71 | 0.87 |
| Hydrophobicity | −0.0456 | 0.3268 | −0.92 | 0.92 |
| Norm ASA | 0.4797 | 0.2721 | 0 | 1 |


### Correlation Matrix
![Corr](results/crm.jpg)

---

## 📚 Comparison with Existing Studies
| Study | Method | Drugs | Accuracy | Remarks |
|--|--|--|--:|--|
| Hadikurniawati 2023 | Classical ML | RIF INH PZA EMB | ~99 | No graph features |
| CRyPTIC 2022 | ML on WGS | 13 | >95 | Low interpretability |
| Jamal 2020 | ML + Docking | 4 | ~85 | High runtime |
| Bhaskar 2023 | CNN on CT + Genomics | – | 97.27 | No mutation-level |
| **RGTFormer** | CGT + RGCN | 6 | **98.67** | Interpretable + efficient |

---

## 💡 Key Takeaways
- Hybrid graph-transformer captures both relational + contextual dependencies.  
- Mutation-level reasoning enhances interpretability.  
- SHAP reveals biologically coherent physicochemical drivers.  
- Extensible to other antimicrobial resistance tasks.

---

## 🧰 Implementation
**Environment**
```bash
Python >=3.10
PyTorch >=2.1
scikit-learn pandas numpy shap matplotlib networkx
