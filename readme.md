# Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/)
[![arXiv:2605.22887](https://img.shields.io/badge/arXiv-2605.22887-b31b1b.svg)](https://arxiv.org/abs/2605.22887)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official code, datasets, and DFT calculations for our research preprint:

> **Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules**  
> Nafis Ahtasum $^{1,2,*}$, Sohanur Rahman Sohan $^{1,3}$, Md. Mostaq Ahmed Himel $^{1,2}$, Md. Zahid Hassan $^{3}$, Muhammad Harussani Moklis $^{1,4,5}$, Masud Rana Rashel $^{6,7}$, Hasan Jamil $^{8}$, AKM Kamrul Islam $^{9}$, Mouhaydine Tlemcani $^{6}$  
> *arXiv:2605.22887* (2026). [Read the paper on arXiv](https://arxiv.org/abs/2605.22887)







### What's this project about?

Lead-based perovskites are great for solar cells, but lead toxicity is a big issue. Double perovskites (A2BB'X6) are a solid lead-free alternative, but finding ones that are actually stable and work as solar absorbers is like finding a needle in a haystack. Running DFT on every possible formula takes way too much compute time on our cluster. 

So we put together this ML screening pipeline. We trained surrogate models on 1,221 DFT calculations and used them to screen an unverified library of 13,088 lead-free compositions. Then we ran full DFT on the best hits to see if they actually hold up.

<p align="center">
  <img src="fig%203.png" alt="Workflow" width="85%"/>
</p>



### Key notes & results

* We generated 13,088 charge-balanced formulas (both A2B+B'3+X6 and A2B2+B'2+X6 types).
* For stability classification, we tuned our Decision Tree specifically for high recall . We did this on purpose: in screening, a false positive just costs an extra DFT check, but a false negative throws away a good material completely.
* For band gaps,  tuned XGBoost regressor .
* We ended up with 5 DFT-confirmed, phase-stable semiconductors (all Ehull <= 0 meV/atom): Cs2CdSnBr6, Cs2CdSnI6, Cs2KGaI6, Rb2SnMnBr6, and Cs2AgAlBr6.

### Validation Through DFT Calculation

To close the loop and ensure that the machine-learning-prioritized candidates exhibit real physical and optoelectronic functionality, we performed comprehensive first-principles DFT validation on the final shortlisted compounds:

* **Structural Optimization & Phase Purity:** Full geometry relaxations were run with DMol³ using the spin-polarized GGA-PBE functional and a DNP basis set. Simulated powder XRD patterns show sharp diffraction peaks and characteristic (111) superlattice reflections, confirming long-range rock-salt ordering of the B/B' sublattices without phase degradation.
* **Thermodynamic Ground States:** All 5 validated compounds reside directly on or very close to the convex hull ($E_{\text{hull}} \le 0\text{ meV/atom}$), verifying 0 K thermodynamic phase stability.
* **Electronic Dispersion & PDOS:** Band structure and projected density of states calculations confirm semiconducting behavior with valence bands dominated by halide $p$-states and conduction band minimums formed by metal $s/p$-states.
* **Carrier Transport Properties:** Evaluated carrier effective masses and phonon-limited mobilities using the 3D Bardeen-Shockley deformation potential model, identifying ultralight electron masses in systems like $\text{Cs}_2\text{CdSnBr}_6$ ($m_e^* = 0.087\,m_0$).
* **Full Optical Phenotype Suite:** Calculated complex dielectric functions $\epsilon(\omega)$, high optical absorption coefficients ($\alpha_{\text{peak}} \approx 10^5\text{ cm}^{-1}$), refractive indices $n(\omega)$, reflectivity $R(\omega)$, plasmon loss functions $L(\omega)$, and optical conductivities $\sigma(\omega)$ using CASTEP.



### The 13 Descriptors ("Genome")

We didn't want any data leakage, so all features are calculated from basic periodic table properties and precursor data without needing relaxed DFT crystal structures:

* **Packing:** Shannon ionic radii (RA, RB, RB', RX) and tolerance factors (t, mu, tau) to check if the crystal packing makes sense.
* **Bonding:** Bond dissociation energies (BE_M-X), Pauling electronegativities, and formation enthalpies to check framework cohesion.
* **Polarization:** Ionization energy (I_A), electron affinity (A_X), and polarizabilities (P_A, P_B, P_B', P_X) to track dielectric screening.
* **Electronic identity:** Valence electron counts and atomic numbers for band-edge dispersion.



### The Screening Funnel

We filtered the 13k compositions in stages:

1. Initial pool: 13,088 charge-balanced lead-free compositions
2. ML Stability check: keeps predicted stable/metastable phases -> 5,354 left
3. Geometric filters: 0.80 <= t <= 1.10, 0.414 <= mu <= 0.732, tau < 4.18 -> 733 left
4. Bandgap targeting: picking candidates in the 1.1 - 1.6 eV range -> 162 left
5. Stricter structural filter (tau < 4.18) -> 51 left
6. Practical filters: dropped toxic stuff (Pb, As, Be) and expensive noble metals -> 24 left
7. DFT validation: full relaxations & electronic structure calculations -> 5 phase-stable semiconductors confirme





### Design Rules

* Geometric formability is a strict gate: if a compound has a bad tolerance factor (outside 0.80 <= t <= 1.10 or tau >= 4.18), trying to optimize bandgaps is pointless.
* Stronger B-X and B'-X bond dissociation energies help phase stability and also directly boost near-edge optical absorption.
* Softer halides (like I-) boost polarizability and dielectric constant (e.g. eps_0 = 8.16 in Cs2CdSnI6), which is great for screening excitons.

