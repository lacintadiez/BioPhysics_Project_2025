
# 🧬 Energy Analysis Project (2025–26)

**Spike RBD–ACE2 Interface Energy Analysis**

---

## 📌 Objective

Evaluate the relative energetic contribution of protein–protein interface residues in the SARS-CoV-2 Spike RBD–ACE2 complex, using structure **6M0J**.

Specifically, we analyze:

* Which residues form the interface  
* Per-residue contribution to binding energy  
* Impact of mutations at interface positions  
* Comparison with FoldX predictions  

---

## 🧠 Background

Large-scale sequencing projects generate many protein variants. Many proteins function as complexes, and small sequence changes can affect:

* Binding affinity  
* Stability  
* Regulation  
* Viral infectivity (for SARS-CoV-2)  

The Spike RBD–ACE2 interaction is a key step in viral infection, targeted by vaccines and neutralizing antibodies. This project explores how interface residues contribute to interaction energy, and how mutations may disrupt or enhance binding.

---

## ⚙️ Setup Instructions

1. **Clone the repository**

```bash
git clone https://github.com/lacintadiez/BioPhysics_Project_2025.git
cd BioPhysics_Project_2025
```

2. **Create and activate a Conda environment**

```bash
conda create -n biophysics_project python=3.10 -y
conda activate biophysics_project
```

3. **Install required Python packages**

```bash
# Core libraries
pip install biopython numpy pandas matplotlib jupyter

# Structure checking library
conda install -c bioconda biobb_structure_checking
```

4. **Install ipykernel for Jupyter**

```bash
conda install ipykernel
# or
pip install ipykernel
```

Then register the environment with Jupyter:

```bash
python -m ipykernel install --user --name biophysics_project --display-name "Python (biophysics_project)"
```

5. **Install and test NACCESS**

```bash
cd Energy_analysis_project/NACCESS/NACCESS
chmod +x naccess
./naccess -h
```

You should see a message like:

```
usage: you must supply a pdb format file
```

6. **Download PDB structure**

```bash
wget https://files.rcsb.org/download/6M0J.pdb
```

---

## 📁 Input Data

* **PDB structure:** 6M0J  
* Contains the SARS-CoV-2 Spike Receptor Binding Domain (RBD) bound to ACE2.

---

## 🔬 Project Workflow

### **Step 1 — Identify Interface Residues**

1. Visually inspect the structure in PyMOL or Chimera.  
2. Define a contact distance threshold (closest atom–atom distance + 1–2 Å).  
3. Use Python scripts to:  
   * Read the PDB  
   * Compute interatomic distances  
   * Output interface residues for each chain  

**Output:** List of interface residues.

---

### **Step 2 — Compute Energetic Contributions**

#### Step 2.1 — Imports and Setup
* Parse cleaned PDB and PDBQT files.  
* Attach charges, atom types, vdW parameters.  
* Run NACCESS to compute ASA for complex and isolated chains.  

#### Step 2.2 — Energy Functions
* Implement Mehler–Solmajer dielectric.  
* Define electrostatics and Lennard-Jones vdW functions.  

#### Step 2.3 — Interface Detection
* Use `NeighborSearch` with cutoff (5 Å) to identify contacting residues.  

#### Step 2.4 — Atom–Atom Interaction
* Compute cross-chain pair energies with single-pass loop (no double-counting).  

#### Step 2.5 — Solvation Definition
* Compute solvation energies from ASA × fsrf for complex and isolated chains.  

#### Step 2.6 — Per-Residue Contributions
* Calculate E, V, solvation difference, and ΔG_res for each interface residue.  

#### Step 2.7 — Table of Most Relevant Interactions
* Output top residues ranked by |ΔG_res| with detailed breakdown.  

#### Step 2.8 — Plotting Results
* Generate stacked bar plots showing per-residue contributions (Electrostatics, vdW, Solvation).  

#### Step 2.9 — Classification of Interactions
* Identify hotspots by type:  
  * Electrostatic (salt bridges)  
  * vdW (hydrophobic packing, aromatic stacking)  
  * Solvation (burial/exposure effects)  

**Output:**  
* Total ΔG of interface (≈ −55 kcal/mol, vdW-dominated).  
* Per-residue table and plots.  
* Hotspot classification.

---

### **Step 3 — Analyze Spike Variants**

* Identify known SARS-CoV-2 RBD mutations.  
* Introduce mutations into the structure, one at a time.  
* Recompute interaction energy.  
* Compare with WT and Ala-scan results.  

---

### **Step 4 — FoldX Comparison**

* Repeat Steps 2–3 using FoldX.  
* Compare ΔΔG predictions between methods.  
* Assess agreement, differences, and potential sources of error.  

---

## ⚙️ Structure Preparation Pipeline

Using **biobb_structure_checking**:

1. Use the provided 6M0J structure.  
2. Select the correct biological assembly.  
3. Remove heteroatoms (water, metals, ligands, hydrogens).  
4. Add:  
   * Missing side chains  
   * Hydrogen atoms  
   * Atomic charges  

> **Note:** PDBQT generation is attempted, but due to software limitations, we use the **provided reference files** for downstream analyses to ensure consistency.

**Output:** Cleaned PDB

---

## 🛠️ Tools Used

* Python 3 + Biopython  
* Custom Python scripts for contact detection and energy decomposition  
* biobb_structure_checking for structure preparation  
* NACCESS for accessible surface area (ASA) calculations  
* FoldX for comparison  
* PyMOL for visualization  

---

## 📤 Expected Outputs

* List of interface residues (per chain)  
* Per-residue interaction energy table and plots  
* Alanine scanning ΔΔG profile  
* Variant impact analysis (ΔΔG)  
* Comparison plots between methods  
* Summary table of energetically important interface residues  

---



