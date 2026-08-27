# Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/)
[![arXiv:2605.22887](https://img.shields.io/badge/arXiv-2605.22887-b31b1b.svg)](https://arxiv.org/abs/2605.22887)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official repository for the preprint:

> **Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules**  
> Nafis Ahtasum, Sohanur Rahman Sohan, Md Mostaq Ahmed Himel, Md Zahid Hassan, Muhammad Harussani Moklis, Md Rafiul Alam Roni  
> *arXiv preprint arXiv:2605.22887* (2026). [https://arxiv.org/abs/2605.22887](https://arxiv.org/abs/2605.22887)

---

## 📌 Overview

This repository implements a high-throughput, quantitative chemical-genome screening pipeline to discover and evaluate stable, lead-free halide double perovskites ($A_2BB'X_6$) for advanced optoelectronic, sensing, and photovoltaic applications. 

Starting from an unverified combinatorial space of **13,088 charge-balanced, lead-free compositions**, the workflow integrates geometric formability gating, 6-family chemical-genome descriptor encoding, evolutionary-algorithm-optimized machine learning surrogates, explainable AI (SHAP), and first-principles Density Functional Theory (DFT) phenotype closure.

<p align="center">
  <img src="figures/workflow.png" alt="Genome-Guided Discovery Workflow" width="85%"/>
</p>
<p align="center"><em>Figure 1: Hierarchical discovery framework combining chemical-space construction, halide-aware formability filtering, chemical-genome descriptor encoding, evolutionary ML surrogate modeling, multi-objective screening, and DFT phenotype closure.</em></p>

---

## 🎯 Key Highlights

* **13,088-Compound Library:** Combinatorial enumeration across homovalent ($A_2B^{2+}B'^{2+}X_6$) and heterovalent ($A_2B^+B'^{3+}X_6$) oxidation families.
* **Recall-Prioritized Stability Classifier:** An evolutionary-tuned Decision Tree stability surrogate engineered for high stable-class recall (**Recall = 0.831**) to protect promising metastable phases ($\Delta E_{\text{hull}} \le 25\text{ meV/atom}$) from premature pruning.
* **Bandgap Surrogate Regression:** A GA-optimized XGBoost regressor predicting scalar GGA-PBE electronic bandgaps with **$R^2 = 0.9317$** and **$\text{RMSE} = 0.5144\text{ eV}$**.
* **XAI Design Heuristics:** SHAP feature attributions across 6 descriptor families that translate complex surrogate matrices into transferable mixing rules.
* **7 DFT-Validated Endpoints:** Candidate space reduction down to 7 stable endpoints: $\text{Cs}_2\text{SnGeBr}_6$, $\text{Rb}_2\text{TeCuBr}_6$, $\text{Cs}_2\text{NiBaI}_6$, $\text{Cs}_2\text{AgInCl}_6$, $\text{K}_2\text{BePdF}_6$, $\text{K}_2\text{MnCdCl}_6$, and $\text{Cs}_2\text{GeSrBr}_6$.
* **Suppressed Excitonic Drag:** Isolation of the post-transition-metal anomaly **$\text{Cs}_2\text{SnGeBr}_6$** ($E_g = 1.112\text{ eV}$, $\mu^* = 0.045\,m_0$, $\epsilon_1(0) = 7.54$), collapsing the exciton binding energy to **$\Delta E_{\text{xb}} = -10.76\text{ meV}$** (~90% reduction relative to $\text{Cs}_2\text{AgBiBr}_6$).

---

## 🧬 Chemical-Genome Descriptor Architecture

Compositions are mapped into a 6-family unrelaxed "Chemical Genome" ($G \in \mathbb{R}^{M \times 6}$) derived purely from elemental and unrelaxed precursor states to prevent data leakage:

| Gene Family | Representative Descriptors | Physical Meaning / Target Role |
| :--- | :--- | :--- |
| **Geometric Packing** | Ionic radii ($R_A, R_B, R_{B'}, R_X$), Tolerance factors ($t, \mu, \tau$) | Steric size compatibility, cage filling, octahedral packing feasibility |
| **Framework Cohesion** | Bond dissociation energies ($BE_{M-X}$), Atomic formation enthalpies ($\Delta H_f$) | Metal-halide bonding robustness, framework persistence, phase stability |
| **Polarity & Covalency** | Electronegativity contrasts ($\Delta\chi_{B-X}, \Delta\chi_{B'-X}, \chi_X$) | Band-edge orbital hybridization, optical transition and absorption strength |
| **Charge Transfer** | Ionization energy ($I_A$), Electron affinities, Redox balance | Cation ionization tendency, charge accommodation, redox plausibility |
| **Polarization & Screening** | Atomic polarizabilities ($\alpha$), Halide softness | Static dielectric response, Fröhlich polaron shielding, exciton attenuation |
| **Electronic Identity** | Valence electron counts ($V_M$), Atomic numbers ($N_M$) | Dispersive $s\text{--}p$ band edges vs. localized $d$-state transport bottlenecks |

<p align="center">
  <img src="figures/shap_summary.png" alt="SHAP Genome Decoding" width="90%"/>
</p>
<p align="center"><em>Figure 2: SHAP feature attribution distributions for stability classification and bandgap regression, showing 1D and 2D partial dependence response curves.</em></p>

---

## 🧪 Multi-Stage Screening Funnel

<p align="center">
  <img src="figures/screening_funnel.png" alt="Screening Funnel and Landscape" width="85%"/>
</p>
<p align="center"><em>Figure 3: Sequential reduction across the screening funnel and the resulting stability-function landscape.</em></p>

1. **Initial Chemical Space:** 13,088 charge-balanced formulas.
2. **Stage I (ML Stability Classifier):** Retains predicted stable/metastable phases $\rightarrow$ **5,354 candidates (40.91%)**.
3. **Stage II (Halide-Aware Geometry):** Enforces $0.80 \le t \le 1.10$, $\mu \ge 0.414$, $\tau < 4.18$ $\rightarrow$ **733 candidates (5.60%)**.
4. **Stage III (Bandgap Targeting):** Gated by optoelectronic application windows $\rightarrow$ **113 candidates (0.86%)**.
5. **Stage IV & V (DFT Phenotype Closure):** First-principles electronic and thermodynamic verification $\rightarrow$ **7 prioritized endpoints (0.05%)**.

---

## 📊 Summary of Validated Candidates

<p align="center">
  <img src="figures/multi_phenotype_map.png" alt="Multi-Phenotype Performance Map" width="90%"/>
</p>
<p align="center"><em>Figure 4: Multi-phenotype performance map comparing normalized electronic, transport, dielectric, and optical absorption scores of the 7 DFT-validated lead-free double perovskites.</em></p>

| Compound | Target Role | $E_g^{\text{DFT}}$ (eV) | Type | $\mu^*$ ($m_0$) | $\epsilon_1(0)$ | $\Delta E_{\text{xb}}$ (meV) | $\Delta H_{\text{decomp}}$ (meV/atom) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **$\text{Cs}_2\text{SnGeBr}_6$** | Broadband Solar Absorber | 1.112 | Direct | 0.045 | 7.54 | -10.76 | +25.0 |
| **$\text{Rb}_2\text{TeCuBr}_6$** | Ambipolar Absorber / NIR | 0.742 | Direct | 0.185 | 13.27 | -14.28 | +21.0 |
| **$\text{Cs}_2\text{NiBaI}_6$** | Narrow-Gap / High Polarizability | 0.656 | Indirect | 0.167 | 15.28 | -6.43 | +26.0 |
| **$\text{Cs}_2\text{AgInCl}_6$** | Electron-Selective / Window | 1.419 | Direct | 0.078 | 3.46 | -88.54 | +27.0 |
| **$\text{K}_2\text{BePdF}_6$** | High Dielectric Polarizability | 0.876 | Direct | 0.280 | 11.27 | -29.98 | +34.0 |
| **$\text{K}_2\text{MnCdCl}_6$** | Stable Reference Semiconductor | 1.060 | Direct | 0.182 | 5.84 | -72.63 | +28.0 |
| **$\text{Cs}_2\text{GeSrBr}_6$** | UV-Selective / Transparent | 3.434 | Direct | 0.114 | 3.71 | -112.50 | +31.0 |

---

## 📂 Repository Layout

```text
genome-guided-perovskite/
├── data/
│   ├── raw/
│   │   └── enumerated_13088_compositions.csv
│   └── processed/
│       ├── chemical_genome_descriptors.csv
│       └── reference_dataset_1221.csv
├── dft_calculations/
│   ├── structures_cif/             # Relaxed crystal structure CIF files
│   ├── band_structures_pdos/       # Electronic band structures and PDOS data
│   ├── optical_spectra/            # Complex dielectric functions & absorption spectra
│   └── aimd_trajectories/          # 300 K NVT ab initio molecular dynamics logs
├── figures/
│   ├── workflow.png
│   ├── shap_summary.png
│   ├── screening_funnel.png
│   ├── multi_phenotype_map.png
│   └── origin_projects/            # OriginPro project files (.opju / .opj)
├── notebooks/
│   ├── 01_chemical_space_enumeration.ipynb
│   ├── 02_genome_descriptor_engineering.ipynb
│   ├── 03_evolutionary_surrogate_training.ipynb
│   └── 04_shap_interpretability_decoding.ipynb
├── src/
│   ├── __init__.py
│   ├── enumeration.py              # Combinatorial space builder
│   ├── formability.py              # Geometric formability calculator (t, μ, τ)
│   ├── descriptors.py              # 6-family chemical-genome constructor
│   ├── models.py                   # EA hyperparameter optimization & surrogate trainers
│   └── xai_shap.py                 # SHAP explainability engine
├── requirements.txt
├── environment.yml
├── LICENSE
└── README.md
