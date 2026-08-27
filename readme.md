This project is genome guided perovskite discovery

# Backward Mapping from Target Optoelectronic Phenotypes to Chemical Genomes for Lead-Free Double Perovskite Discovery

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/)
[![Materials Today Physics](https://img.shields.io/badge/DOI-10.1016%2Fj.mtphys.2026.102180-blue.svg)](https://doi.org/10.1016/j.mtphys.2026.102180)
[![arXiv:2605.22887](https://img.shields.io/badge/arXiv-2605.22887-b31b1b.svg)](https://arxiv.org/abs/2605.22887)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official repository for the research paper:

> **Backward mapping from target optoelectronic phenotypes to chemical genomes for the discovery of lead-free double perovskites**  
> Nafis Ahtasum, Sohanur Rahman Sohan, Md Mostaq Ahmed Himel, Md Zahid Hassan, Muhammad Harussani Moklis, Md Rafiul Alam Roni  
> *Materials Today Physics*, 66 (2026) 102180. [https://doi.org/10.1016/j.mtphys.2026.102180](https://doi.org/10.1016/j.mtphys.2026.102180)



## 📌 Overview

Traditional materials discovery relies heavily on brute-force forward screening arrays that sequentially evaluate predefined compositional grids at massive computational cost. In this work, we invert the discovery trajectory by establishing a **chemical-genome-guided backward mapping framework**. 

Starting from an unverified combinatorial library of **13,088 charge-balanced, lead-free $A_2BB'X_6$ compositions**, we map the structural and optoelectronic landscape using evolutionary-algorithm-optimized machine learning surrogates, explainable AI (SHAP), and first-principles Density Functional Theory (DFT) phenotype closure.

<p align="center">
  <img src="figures/workflow.png" alt="Backward Mapping Discovery Workflow" width="85%"/>
</p>
<p align="center"><em>Figure 1: Five-stage backward-mapping discovery framework combining combinatorial space generation, 6-family chemical genome encoding, evolutionary ML surrogate modeling, multi-objective screening, and DFT phenotype closure.</em></p>



## 🎯 Key Highlights

* **Combinatorial Library:** Enumerated 13,088 unique charge-balanced Pb-free double perovskites across homovalent ($A_2B^{2+}B'^{2+}X_6$) and heterovalent ($A_2B^+B'^{3+}X_6$) families.
* **Recall-Prioritized Classification:** Built an evolutionary-tuned Decision Tree stability surrogate prioritized for high stable-class recall (**Recall = 0.831**) to minimize False Negatives (Type II errors) and preserve promising metastable phases ($\Delta E_{\text{hull}} \le 25\text{ meV/atom}$).
* **Accurate Bandgap Regression:** GA-optimized XGBoost regressor predicting scalar GGA-PBE bandgaps with **$R^2 = 0.9317$** and **$\text{RMSE} = 0.5144\text{ eV}$**.
* **Chemical Genome Decoding:** Utilized SHAP feature attributions across 6 descriptor families to convert complex surrogate matrices into transferable mixing rules.
* **Elite Discovered Endpoints:** Contracted the candidate space by >99.9% down to **7 DFT-validated endpoints**: $\text{Cs}_2\text{SnGeBr}_6$, $\text{Rb}_2\text{TeCuBr}_6$, $\text{Cs}_2\text{NiBaI}_6$, $\text{Cs}_2\text{AgInCl}_6$, $\text{K}_2\text{BePdF}_6$, $\text{K}_2\text{MnCdCl}_6$, and $\text{Cs}_2\text{GeSrBr}_6$.
* **Excitonic Drag Collapse:** Discovered the post-transition-metal anomaly **$\text{Cs}_2\text{SnGeBr}_6$** ($E_g = 1.112\text{ eV}$, $\mu^* = 0.045\,m_0$, $\epsilon_1(0) = 7.54$), which suppresses total exciton binding energy to **$\Delta E_{\text{xb}} = -10.76\text{ meV}$** (~90% reduction relative to the $\text{Cs}_2\text{AgBiBr}_6$ benchmark).



## 🧬 Chemical-Genome Descriptor Architecture

Compositions are mapped into a 6-family unrelaxed "Chemical Genome" ($G \in \mathbb{R}^{M \times 6}$) to completely eliminate data leakage from relaxed structures:

| Gene Family | Representative Descriptors | Physical Meaning / Target Role |
| :--- | :--- | :--- |
| **Geometric Packing** | Ionic radii ($R_A, R_B, R_{B'}, R_X$), Tolerance factors ($t, \mu, \tau$) | Steric fit, octahedral compatibility, structural formability gate |
| **Framework Cohesion** | Bond dissociation energies ($BE_{M-X}$), Atomic formation enthalpies ($\Delta H_f$) | Metal-halide bonding strength, lattice cohesion, convex-hull stability |
| **Polarity & Covalency** | Electronegativity differences ($\Delta\chi_{B-X}, \Delta\chi_{B'-X}, \chi_X$) | Metal-halide orbital hybridization, optical transition strength |
| **Charge Transfer** | Ionization energy ($I_A$), Electron affinities, Redox balance | Cation oxidation feasibility, absolute band-edge positioning |
| **Polarization & Screening** | Atomic polarizabilities ($\alpha$), Halide softness | Dielectric screening, Fröhlich polaron shielding, exciton binding |
| **Electronic Identity** | Valence electron counts ($V_M$), Atomic numbers ($N_M$) | Band edge dispersion, carrier effective mass ($\mu^*$), transport asymmetry |

<p align="center">
  <img src="figures/shap_summary.png" alt="SHAP Genome Decoding" width="90%"/>
</p>
<p align="center"><em>Figure 2: Mechanistic genome decoding showing SHAP summary distributions for stability classification and bandgap regression, alongside 1D/2D response partial dependencies.</em></p>

---

## 🧪 Inverse-Design Screening Funnel

<p align="center">
  <img src="figures/screening_funnel.png" alt="Inverse-Design Screening Funnel" width="85%"/>
</p>
<p align="center"><em>Figure 3: Candidate reduction trajectory across the staged inverse-design screening funnel and resulting stability-function landscape.</em></p>

The 13,088 initial compositions are narrowed down sequentially:
1. **Stage I (ML Stability Classifier):** Filters out non-viable configurations $\rightarrow$ **5,354 candidates (40.91%)**
2. **Stage II (Halide-Aware Geometry):** Enforces $0.80 \le t \le 1.10$, $\mu \ge 0.414$, $\tau < 4.18$ $\rightarrow$ **733 candidates (5.60%)**
3. **Stage III (Bandgap Targeting):** Gated by target optoelectronic role windows $\rightarrow$ **113 candidates (0.86%)**
4. **Stage IV & V (DFT Phenotype Closure & Validation):** Full first-principles closure $\rightarrow$ **7 prioritized endpoints (0.05%)**



## 📊 Summary of Discovered Candidates

<p align="center">
  <img src="figures/multi_phenotype_map.png" alt="Multi-Phenotype Performance Map" width="90%"/>
</p>
<p align="center"><em>Figure 4: Multi-phenotype performance map comparing normalized electronic, transport, dielectric, and optical absorption scores of the 7 DFT-validated lead-free double perovskites.</em></p>

| Compound | Target Device Role | $E_g^{\text{DFT}}$ (eV) | Type | $\mu^*$ ($m_0$) | $\epsilon_1(0)$ | $\Delta E_{\text{xb}}$ (meV) | $\Delta H_{\text{decomp}}$ (meV/atom) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **$\text{Cs}_2\text{SnGeBr}_6$** | Broadband Solar Absorber | 1.112 | Direct | 0.045 | 7.54 | -10.76 | +25.0 |
| **$\text{Rb}_2\text{TeCuBr}_6$** | Ambipolar Absorber / NIR | 0.742 | Direct | 0.185 | 13.27 | -14.28 | +21.0 |
| **$\text{Cs}_2\text{NiBaI}_6$** | Narrow-Gap / High Polarizability | 0.656 | Indirect | 0.167 | 15.28 | -6.43 | +26.0 |
| **$\text{Cs}_2\text{AgInCl}_6$** | Electron-Selective / Window | 1.419 | Direct | 0.078 | 3.46 | -88.54 | +27.0 |
| **$\text{K}_2\text{BePdF}_6$** | High Dielectric Polarizability | 0.876 | Direct | 0.280 | 11.27 | -29.98 | +34.0 |
| **$\text{K}_2\text{MnCdCl}_6$** | Stable Reference Semiconductor | 1.060 | Direct | 0.182 | 5.84 | -72.63 | +28.0 |
| **$\text{Cs}_2\text{GeSrBr}_6$** | UV-Selective / Transparent | 3.434 | Direct | 0.114 | 3.71 | -112.50 | +31.0 |



## 📂 Repository Structure


genome-guided-perovskite/
├── data/
│   ├── raw/
│   │   └── enumerated_13088_compositions.csv
│   └── processed/
│       ├── chemical_genome_descriptors.csv
│       └── reference_dataset_1221.csv
├── dft_calculations/
│   ├── structures_cif/             # Relaxed crystal structures (CIFs)
│   ├── band_structures_pdos/       # Electronic band dispersion & PDOS data
│   ├── optical_spectra/            # Dielectric functions, absorption, reflectivity
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
│   ├── enumeration.py              # Pymatgen combinatorial space builder
│   ├── formability.py              # Geometric gates (t, μ, τ calculation)
│   ├── descriptors.py              # 6-family chemical genome constructor
│   ├── models.py                   # EA hyperparameter optimization & ML pipelines
│   └── xai_shap.py                 # SHAP explainability engine
├── requirements.txt
├── environment.yml
├── LICENSE
└── README.md
