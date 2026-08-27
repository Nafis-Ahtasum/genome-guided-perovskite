# Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/)
[![arXiv:2605.22887](https://img.shields.io/badge/arXiv-2605.22887-b31b1b.svg)](https://arxiv.org/abs/2605.22887)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official repository for the research preprint:

> **Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules**  
> Nafis Ahtasum $^{1,2,*}$, Sohanur Rahman Sohan $^{1,3}$, Md. Mostaq Ahmed Himel $^{1,2}$, Md. Zahid Hassan $^{3}$, Muhammad Harussani Moklis $^{1,4,5}$, Masud Rana Rashel $^{6,7}$, Hasan Jamil $^{8}$, AKM Kamrul Islam $^{9}$, Mouhaydine Tlemcani $^{6}$  
> *arXiv preprint arXiv:2605.22887* (2026). [https://arxiv.org/abs/2605.22887](https://arxiv.org/abs/2605.22887)



### Author Affiliations
$^{1}$ Center for Material, Climate and Energy, Research and Analysis Institute, RAI Initiative Ltd, Dhaka, Bangladesh  
$^{2}$ Department of Apparel Engineering, Bangladesh University of Textiles, Dhaka-1208, Bangladesh  
$^{3}$ Department of Textile Engineering Management, Bangladesh University of Textiles, Dhaka-1208, Bangladesh  
$^{4}$ Department of Chemical and Environmental Engineering, Faculty of Engineering, Universiti Putra Malaysia, Serdang 43400, Selangor, Malaysia  
$^{5}$ Energy Science and Engineering, Department of Transdisciplinary Science and Engineering, Institute of Science Tokyo, 2-12-1, Ookayama, Meguro-ku, Tokyo 152-8550, Japan  
$^{6}$ Instrumentation and Control Laboratory, Center for sci-tech Research in Earth System and Energy, University of Évora, Portugal  
$^{7}$ Department of Computer Science and Engineering, Daffodil International University, Bangladesh  
$^{8}$ Department of Computer Science, University of Idaho, 875 Perimeter Drive, Moscow, ID 83844, USA  
$^{9}$ Computational Science and Engineering, North Carolina A&T State University, Greensboro, North Carolina, USA  




## 📌 Overview

This repository implements an interpretable, genome-guided machine learning and Density Functional Theory (DFT) inverse-design framework to screen and discover phase-stable, lead-free all-inorganic double perovskites ($A_2BB'X_6$).

Starting from a combinatorial library of **13,088 charge-balanced, lead-free compositions**, the workflow integrates geometric formability gating, a 4-cluster "Stability Genome" descriptor set, evolutionary-optimized surrogate models (Decision Tree & XGBoost), SHAP explainability, and comprehensive first-principles DFT phenotype closure.

<p align="center">
  <img src="fig%203.png" alt="Genome-Guided Discovery Workflow" width="85%"/>
</p>
<p align="center"><em>Figure 1: Five-stage screening workflow combining chemical-space construction, descriptor-genome encoding, evolutionary ML surrogate modeling, multi-objective inverse-design screening, and DFT phenotype closure.</em></p>



## 🎯 Key Highlights

* **Combinatorial Realization Space:** 13,088 unique charge-balanced formulas enumerated across heterovalent ($A_2B^+B'^{3+}X_6$) and homovalent ($A_2B^{2+}B'^{2+}X_6$) oxidation families.
* **Recall-Optimized Stability Surrogate:** An evolutionary algorithm (EA)-tuned Decision Tree classifier achieving **ROC-AUC = 0.93** and a stable-class recall of **Recall = 0.831** to prevent premature loss of promising metastable phases ($E_{\text{hull}} \le 25\text{ meV/atom}$).
* **Screening-Grade Bandgap Regressor:** An EA-tuned XGBoost regressor predicting scalar GGA-PBE electronic bandgaps with **$R^2 = 0.9317$** and **$\text{RMSE} = 0.5144\text{ eV}$**.
* **Interpretable Stability Genome:** Descriptors grouped into 4 distinct physical categories—Packing, Bonding, Polarization, and Electronic Identity—to decode multi-axial property trade-offs.
* **DFT-Validated Endpoints:** Staged constraint stacking narrows 13,088 compositions down to **5 phase-stable semiconductors**: $\text{Rb}_2\text{SnMnBr}_6$, $\text{Cs}_2\text{CdSnBr}_6$, $\text{Cs}_2\text{CdSnI}_6$, $\text{Cs}_2\text{KGaI}_6$, and $\text{Cs}_2\text{AgAlBr}_6$ ($E_{\text{hull}} \le 0\text{ meV/atom}$).
* **Full Optical & Transport Suite:** Complete evaluation of carrier effective masses, deformation-potential mobilities, complex dielectric functions ($\epsilon_0 = 4.57\text{--}8.16$), optical absorption ($\alpha_{\text{peak}} \approx 10^5\text{ cm}^{-1}$), energy-loss plasmon fingerprints, and frequency-dependent optical conductivities.



## 🧬 Stability-Genome Descriptor Architecture

Each $A_2BB'X_6$ composition is represented using a compact, 13-feature unrelaxed stability genome designed to avoid data leakage while preserving mechanistic interpretability:

| Functional Role | Gene Cluster | Descriptors | Sites | Physical & Target Role |
| :--- | :--- | :--- | :--- | :--- |
| **Structural Existence** | **Packing (Formability)** | Ionic radii ($R_A, R_B, R_{B'}, R_X$), Tolerance factors ($t, \mu, \tau$) | A, B, B', X | Determines ionic size matching, cage filling, octahedral fit, and lattice distortion. |
| **Thermodynamic Stability** | **Bonding (Framework Cohesion)** | Bond dissociation energies ($BE_{A-X}, BE_{B-X}, BE_{B'-X}$), Electronegativity ($\chi_{B}, \chi_{B'}, \chi_X$), Formation enthalpies ($E_A, E_B, E_{B'}, E_X$) | A, B, B', X | Controls metal-halide bond strength, network cohesion, bond polarity, and elemental stability conditioning. |
| **Optoelectronic Function** | **Polarization (Electronic Response)** | First ionization energy ($I_A$), Electron affinity ($A_X$), Polarizability ($P_A, P_B, P_{B'}, P_X$) | A, B, B', X | Dictates cation ionization, anion charge accommodation, dielectric constant ($\epsilon_0$), and lattice softness. |
| **Optoelectronic Function** | **Electronic Identity** | Valence electron counts ($V_A, V_B, V_{B'}, V_X$), Atomic numbers ($N_A, N_B, N_{B'}$) | A, B, B', X | Regulates orbital filling, relativistic effects, periodic trends, and band-edge dispersion. |



## 🧪 Staged  Screening Funnel

<p align="center">
  <img src="figures/screening_funnel.png" alt="Screening Funnel and Landscape" width="85%"/>
</p>
<p align="center"><em>Figure 2: Multi-stage candidate reduction funnel from 13,088 compositions to 5 phase-stable semiconductors.</em></p>

1. **Combinatorial Library:** 13,088 unique charge-balanced $A_2BB'X_6$ compositions.
2. **Phase I (ML Stability Classifier):** Filters for predicted thermodynamic stability $\rightarrow$ **5,354 candidates (40.91%)**.
3. **Phase II (Geometric Gating):** $0.80 \le t \le 1.10$, $0.414 \le \mu \le 0.732$, $\tau < 4.18$ $\rightarrow$ **733 candidates (5.60%)**.
4. **Phase III (Bandgap Window):** Application-targeted window ($1.1\text{--}1.6\text{ eV}$) $\rightarrow$ **162 candidates (1.24%)**.
5. **Phase IV (Revised Tolerance):** Tightened structural filter ($\tau < 4.18$) $\rightarrow$ **51 candidates (0.39%)**.
6. **Phase V (Deployability Filter):** Removal of toxic (Pb, As, Be) and scarce noble elements $\rightarrow$ **24 candidates (0.18%)**.
7. **Phase VI (Targeted DFT Closure):** Ground-state validation $\rightarrow$ **5 phase-stable semiconductors (0.04%)**.


## 📊 Summary of DFT-Validated Candidates

<p align="center">
  <img src="figures/application_map.png" alt="Application-Oriented Candidate Map" width="75%"/>
</p>
<p align="center"><em>Figure 3: Application-oriented portfolio map showing PBE bandgap ($E_g$), near-edge absorption $\alpha(E_g + 0.5\text{ eV})$, and static dielectric constant ($\epsilon_0$).</em></p>

| Compound | Space Group | $a$ (Å) | $E_g^{\text{DFT}}$ (eV) | $m_e^*$ ($m_0$) | $m_h^*$ ($m_0$) | $\epsilon_0$ | $\alpha(E_g + 0.5\text{ eV})$ ($\text{cm}^{-1}$) | Target Application |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **$\text{Cs}_2\text{CdSnBr}_6$** | $P1$ | 11.488 | 1.794 | 0.087 | 0.197 | 5.14 | $1.5 \times 10^5$ | Promising PV Absorber / Ultralight Carriers |
| **$\text{Cs}_2\text{CdSnI}_6$** | $Fm\bar{3}m$ | 8.238 | 1.132 | 0.73 | 1.06 | 8.16 | $1.8 \times 10^5$ | Ideal PV Absorber / High Dielectric Screening |
| **$\text{Cs}_2\text{KGaI}_6$** | $P1$ | 12.453 | 0.696 | 0.95 | 1.24 | 4.58 | $1.1 \times 10^5$ | Near-IR Detector / Narrow-Gap Semiconductor |
| **$\text{Rb}_2\text{SnMnBr}_6$** | $P1$ | 12.400 | 1.237 | 0.18 | 2.10 | 6.15 | $1.2 \times 10^5$ | Asymmetric-Transport / Spin-Active Device |
| **$\text{Cs}_2\text{AgAlBr}_6$** | $P1$ | 11.000 | 1.096 | 0.17 | 1.06 | 4.57 | $1.3 \times 10^5$ | Auxiliary Optoelectronic / Window Layer |



## 🛠️ DFT-Validated Design Rules

Genotype-phenotype coupling analysis reveals three hierarchical design heuristics:

1. **Formability Gate (Packing Genes):** Tolerance and octahedral factors ($t, \mu, \tau$) set the absolute steric boundary. If a candidate falls outside the structural manifold ($0.80 \le t \le 1.10$, $\tau < 4.18$), downstream electronic and optical optimization fails.
2. **Framework Stabilization (Bonding Genes):** Within the formable manifold, maximizing $B\text{--}X$ and $B'\text{--}X$ bond dissociation energies enhances 0 K phase stability while directly boosting above-onset absorption $\alpha(E_g + 0.5\text{ eV})$ and optical conductivity $\sigma_1(\omega)$ via covalent metal-halide hybridization.
3. **Optoelectronic Response Tuning (Polarization Genes):** Static dielectric constant ($\epsilon_0$) and exciton screening are governed by atomic polarizabilities and soft halide sublattices (e.g., $\text{I}^-$ yielding $\epsilon_0 \approx 8.16$ in $\text{Cs}_2\text{CdSnI}_6$). Delocalized $s\text{--}p$ bands (Sn, Ga) promote low effective masses, whereas localized $3d$ states (Mn) induce carrier asymmetry.

