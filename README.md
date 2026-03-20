# Generation and Filtering of Drug-like Molecules

This work implements the generation and filtering of drug-like molecules targeting the **Epidermal Growth Factor Receptor (EGFR)** using a dynamic, feature-aligned diffusion framework guided by expert knowledge (ExpDiff model). The goal is to generate structurally valid molecules with desirable pharmaceutical properties and identify top candidates for further investigation.  

<p align="center">
  <img src="/paper/ExpDiff_framework_scheme.png" width="650" height="600">
</p>

**Last Update:** March, 20th, 2026  

## Contents

- **ExpDiff_research.ipynb**: Main notebook performing molecule generation, property calculation, and filtering (Section 4).
- **results/:** Stores generated molecule images, CSV files with properties, and top candidate structures.
- **paper/:** Contains research article and ExpDiff framework illustration.
- **src/:** Contains the core implementation of the diffusion model and utility scripts and origin notebooks including all steps (Section 1 - 3 of the **ExpDiff_research.ipynb**) leading up to the final experiments.


---

## Problem Statement

The main objective of this section (Section 4) is to address the following challenge in computational drug design:

**Designing molecules that bind effectively to a biologically relevant target while satisfying pharmaceutical property constraints.**

Key issues include:

1. **Drug-likeness and synthesizability** – generated molecules should have high QED scores and feasible synthetic accessibility (SA).
2. **Physicochemical constraints** – molecular weight, LogP, HBD/HBA counts, and rotatable bonds must lie within drug-like ranges.
3. **Safety and toxicity** – molecules should avoid structural alerts or potential toxic substructures.  

EGFR was chosen as a target because it is a clinically relevant receptor tyrosine kinase involved in cancer progression, widely studied with abundant known inhibitors, providing a reliable reference space for generative model evaluation.

---

## Approach

The workflow implemented in `ExpDiff_research.ipynb` includes:

1. **Molecule Generation**  
   - Uses a pre-trained 3D feature-aligned diffusion model to sample candidate ligands for EGFR.
   - Initially generates a larger set of molecules to ensure diversity.

2. **Property Computation**. Calculates key molecular descriptors for each molecule:
     - Molecular Weight (MW)
     - Lipophilicity (LogP)
     - QED score
     - Hydrogen bond donors (HBD) and acceptors (HBA)
     - Rotatable bonds
     - Number of rings
     - Synthetic Accessibility (SA) score
     - Toxicity flag (boolean heuristic)

3. **Filtering Strategy**. Multi-step filtering to select the most promising candidates:
     - QED > 0.5  
     - LogP < 5  
     - MW < 600  
     - HBD ≤ 5, HBA ≤ 10  
     - Rotatable bonds ≤ 10  
     - SA score < 8  
     - Non-toxic molecules only
  Reduces the dataset to a top 5–10 candidate molecules.

4. **Visualization**  
   - Generates grid images of top molecules using RDKit.
   - Provides visual inspection of molecular scaffolds and pharmacophore elements.

---

## Results

<p align="center">
  <img src="/results/egfr_top_molecules.png" width="900" height="450">
</p>


- **All generated molecules:** 10+ candidates with diverse scaffolds and calculated properties.  
- **Top candidates (after filtering):** 5 molecules with optimal QED (0.51–0.59), balanced LogP (3.4–4.3), low SA (~1.2–1.3), and non-toxic.  
- **Outputs stored in `results/`:**
  - `egfr_top_molecules.png` – grid image of top molecules.
  - `egfr_top_candidates.csv` – CSV with properties of top candidates.
