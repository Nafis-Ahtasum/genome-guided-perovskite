# Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers

Code and datasets for our preprint:
**"Genome-Guided Interpretable Screening of Phase-Stable, Lead-Free Double Perovskite Absorbers for All-Inorganic Semiconductors, Sensors, and Photovoltaics with DFT-Validated Design Rules"**
Nafis Ahtasum, Sohanur Rahman Sohan, Md. Mostaq Ahmed Himel, Md. Zahid Hassan, Muhammad Harussani Moklis, Masud Rana Rashel, Hasan Jamil, AKM Kamrul Islam, Mouhaydine Tlemcani[cite: 2]
arXiv link: https://arxiv.org/abs/2605.22887[cite: 2]

---

### What's this project about?

Lead-based perovskites are great for solar cells, but lead toxicity is a big issue[cite: 2]. Double perovskites (A2BB'X6) are a solid lead-free alternative, but finding ones that are actually stable and work as solar absorbers is like finding a needle in a haystack[cite: 2]. Running DFT on every possible formula takes way too much compute time on our cluster[cite: 2]. 

So we put together this ML screening pipeline[cite: 2]. We trained surrogate models on 1,221 DFT calculations and used them to screen an unverified library of 13,088 lead-free compositions[cite: 2]. Then we ran full DFT on the best hits to see if they actually hold up[cite: 2].

<p align="center">
  <img src="fig%203.png" alt="Workflow" width="85%"/>
</p>

---

### Key notes & results

* We generated 13,088 charge-balanced formulas (both A2B+B'3+X6 and A2B2+B'2+X6 types)[cite: 2].
* For stability classification, we tuned our Decision Tree specifically for high recall (Recall = 0.831, ROC-AUC = 0.93)[cite: 2]. We did this on purpose: in screening, a false positive just costs an extra DFT check, but a false negative throws away a good material completely[cite: 2].
* For band gaps, our tuned XGBoost regressor hits R2 = 0.9317 and RMSE = 0.5144 eV[cite: 2].
* We ended up with 5 DFT-confirmed, phase-stable semiconductors (all Ehull <= 0 meV/atom): Cs2CdSnBr6, Cs2CdSnI6, Cs2KGaI6, Rb2SnMnBr6, and Cs2AgAlBr6[cite: 2].

---

### The 13 Descriptors ("Stability Genome")

We didn't want any data leakage, so all features are calculated from basic periodic table properties and precursor data without needing relaxed DFT crystal structures[cite: 2]:

* **Packing:** Shannon ionic radii (RA, RB, RB', RX) and tolerance factors (t, mu, tau) to check if the crystal packing makes sense[cite: 2].
* **Bonding:** Bond dissociation energies (BE_M-X), Pauling electronegativities, and formation enthalpies to check framework cohesion[cite: 2].
* **Polarization:** Ionization energy (I_A), electron affinity (A_X), and polarizabilities (P_A, P_B, P_B', P_X) to track dielectric screening[cite: 2].
* **Electronic identity:** Valence electron counts and atomic numbers for band-edge dispersion[cite: 2].

---

### The Screening Funnel

We filtered the 13k compositions in stages[cite: 2]:

1. Initial pool: 13,088 charge-balanced lead-free compositions[cite: 2]
2. ML Stability check: keeps predicted stable/metastable phases -> 5,354 left[cite: 2]
3. Geometric filters: 0.80 <= t <= 1.10, 0.414 <= mu <= 0.732, tau < 4.18 -> 733 left[cite: 2]
4. Bandgap targeting: picking candidates in the 1.1 - 1.6 eV range -> 162 left[cite: 2]
5. Stricter structural filter (tau < 4.18) -> 51 left[cite: 2]
6. Practical filters: dropped toxic stuff (Pb, As, Be) and expensive noble metals -> 24 left[cite: 2]
7. DFT validation: full relaxations & electronic structure calculations -> 5 phase-stable semiconductors confirmed[cite: 2]



### The 5 DFT-Validated Hits

| Compound | Space Group | DFT Bandgap (eV) | m_e* (m0) | m_h* (m0) | eps_0 | Target Role |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Cs2CdSnBr6**[cite: 2] | P1[cite: 2] | 1.794[cite: 2] | 0.087[cite: 2] | 0.197[cite: 2] | 5.14[cite: 2] | PV absorber with light carrier masses[cite: 2] |
| **Cs2CdSnI6**[cite: 2] | Fm-3m[cite: 2] | 1.132[cite: 2] | 0.73[cite: 2] | 1.06[cite: 2] | 8.16[cite: 2] | Near-ideal PV bandgap & strong dielectric screening[cite: 2] |
| **Cs2KGaI6**[cite: 2] | P1[cite: 2] | 0.696[cite: 2] | 0.95[cite: 2] | 1.24[cite: 2] | 4.58[cite: 2] | Narrow-gap / Near-IR sensor[cite: 2] |
| **Rb2SnMnBr6**[cite: 2] | P1[cite: 2] | 1.237[cite: 2] | 0.18[cite: 2] | 2.10[cite: 2] | 6.15[cite: 2] | Asymmetric transport / spin-active device[cite: 2] |
| **Cs2AgAlBr6**[cite: 2] | P1[cite: 2] | 1.096[cite: 2] | 0.17[cite: 2] | 1.06[cite: 2] | 4.57[cite: 2] | Auxiliary optoelectronic / window layer[cite: 2] |



### Main Takeaways / Design Rules

* Geometric formability is a strict gate: if a compound has a bad tolerance factor (outside 0.80 <= t <= 1.10 or tau >= 4.18), trying to optimize bandgaps is pointless[cite: 2].
* Stronger B-X and B'-X bond dissociation energies help phase stability and also directly boost near-edge optical absorption[cite: 2].
* Softer halides (like I-) boost polarizability and dielectric constant (e.g. eps_0 = 8.16 in Cs2CdSnI6), which is great for screening excitons[cite: 2].

