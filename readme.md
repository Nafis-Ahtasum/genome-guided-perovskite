# Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers

Code and datasets for our preprint:
**"Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules"**
Nafis Ahtasum, Sohanur Rahman Sohan, Md. Mostaq Ahmed Himel, Md. Zahid Hassan, Muhammad Harussani Moklis, Masud Rana Rashel, Hasan Jamil, AKM Kamrul Islam, Mouhaydine Tlemcani
arXiv link: https://arxiv.org/abs/2605.22887



### What's this project about?

Lead-based perovskites are great for solar cells, but lead toxicity is a big issue. Double perovskites (A2BB'X6) are a solid lead-free alternative, but finding ones that are actually stable and work as solar absorbers is like finding a needle in a haystack. Running DFT on every possible formula takes way too much compute time on our cluster. 

So we put together this ML screening pipeline. We trained surrogate models on 1,221 DFT calculations and used them to screen an unverified library of 13,088 lead-free compositions. Then we ran full DFT on the best hits to see if they actually hold up.

<p align="center">
  <img src="fig%203.png" alt="Workflow" width="85%"/>
</p>



### Key notes & results

* We generated 13,088 charge-balanced formulas (both A2B+B'3+X6 and A2B2+B'2+X6 types).
* For stability classification, we tuned our Decision Tree specifically for high recall (Recall = 0.831, ROC-AUC = 0.93). We did this on purpose: in screening, a false positive just costs an extra DFT check, but a false negative throws away a good material completely.
* For band gaps, our tuned XGBoost regressor hits R2 = 0.9317 and RMSE = 0.5144 eV.
* We ended up with 5 DFT-confirmed, phase-stable semiconductors (all Ehull <= 0 meV/atom): Cs2CdSnBr6, Cs2CdSnI6, Cs2KGaI6, Rb2SnMnBr6, and Cs2AgAlBr6.



### The 13 Descriptors ("Stability Genome")

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

### The 5 DFT-Validated Hits

| Compound | Space Group | DFT Bandgap (eV) | m_e* (m0) | m_h* (m0) | eps_0 | Target Role |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Cs2CdSnBr6** | P1 | 1.794 | 0.087 | 0.197 | 5.14 | PV absorber with light carrier masses |
| **Cs2CdSnI6** | Fm-3m | 1.132 | 0.73 | 1.06 | 8.16 | Near-ideal PV bandgap & strong dielectric screening |
| **Cs2KGaI6** | P1 | 0.696 | 0.95 | 1.24 | 4.58 | Narrow-gap / Near-IR sensor |
| **Rb2SnMnBr6** | P1 | 1.237 | 0.18 | 2.10 | 6.15 | Asymmetric transport / spin-active device |
| **Cs2AgAlBr6** | P1 | 1.096 | 0.17 | 1.06 | 4.57 | Auxiliary optoelectronic / window layer |



### Main Takeaways / Design Rules

* Geometric formability is a strict gate: if a compound has a bad tolerance factor (outside 0.80 <= t <= 1.10 or tau >= 4.18), trying to optimize bandgaps is pointless.
* Stronger B-X and B'-X bond dissociation energies help phase stability and also directly boost near-edge optical absorption.
* Softer halides (like I-) boost polarizability and dielectric constant (e.g. eps_0 = 8.16 in Cs2CdSnI6), which is great for screening excitons.

