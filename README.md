# 🧬 Energy Analysis Project (2025–26)

**Spike RBD–ACE2 Interface Energy Analysis**

---

## 📌 Objective

Evaluate the relative energetic contribution of protein–protein interface residues in the SARS-CoV-2 Spike RBD–ACE2 complex, using structure **6M0J**.

Specifically, we analyze:

* Which residues form the interface
* Per-residue contribution to binding energy
* Impact of mutations at interface positions
* Alanine scanning to identify hotspots and stabilizers
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
pip install biopython numpy pandas matplotlib jupyter seaborn

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

### **Step 1 — Structure Preparation & Interface Identification**

1. Use **biobb_structure_checking** to clean the structure:

   * Remove waters, metals, ligands, and redundant hydrogens
   * Fix missing side chains, backbone, and amides
   * Add hydrogens and charges

2. Visual inspection in PyMOL or Chimera for verification.

3. Define a contact distance threshold (closest atom–atom distance + 1–2 Å).

4. Use Python scripts to:

   * Read the cleaned PDB
   * Compute interatomic distances
   * Identify interface residues for chains A and E

**Output:** Cleaned PDB (`6m0j_fixed_step1.pdb`) and list of interface residues.

---

### **Step 2 — Compute Energetic Contributions**

#### Step 2.1 — Imports and Setup

* Parse cleaned PDB and PDBQT files.
* Assign atom types, charges, and vdW parameters.
* Run NACCESS to compute ASA for complex and isolated chains.

#### Step 2.2 — Energy Functions

* Mehler–Solmajer dielectric for electrostatics.
* Lennard-Jones potential for van der Waals.
* Solvation energies from ASA × solvation parameter.

#### Step 2.3 — Interface Detection

* `NeighborSearch` with cutoff (5 Å) to detect contacting residues.

#### Step 2.4 — Atom–Atom Interaction

* Compute cross-chain pairwise energies, avoiding double-counting.

#### Step 2.5 — Per-Residue Contributions

* Calculate:

  * Electrostatics (E)
  * van der Waals (V)
  * Solvation ΔG
  * Total ΔG_res per residue

#### Step 2.6 — Plotting and Classification

* Stacked bar plots of per-residue energy components.
* Classify interface residues as hotspots (strong contributors) or stabilizers:

  * Electrostatics (salt bridges)
  * vdW (hydrophobic packing, aromatic stacking)
  * Solvation effects

**Output:**

* Total ΔG of interface (~ −55 kcal/mol, vdW-dominated)
* Per-residue energy table and plots
* Hotspot classification

---

### **Step 3 — Alanine Scanning and Variant Analysis**

* For each interface residue:

  * Mutate to alanine, keeping backbone and CB atom
  * Recompute interaction energies
  * Calculate ΔΔG = WT − Ala

* Identify key stabilizers and hotspots.

* Analyze known SARS-CoV-2 RBD variants:

  * Introduce mutations individually
  * Recompute ΔΔG and compare to WT and alanine scan

**Output:**

* ΔΔG profile for all interface residues
* Variant impact analysis plots and tables

---

### **Step 4 — FoldX Comparison**

* Repeat Steps 2–3 using FoldX.
* Compare ΔΔG predictions between methods.
* Evaluate agreement and discuss potential sources of differences.

---

## ⚙️ Structure Preparation Pipeline

Using **biobb_structure_checking**:

1. Input: 6M0J PDB
2. Select the biological assembly
3. Remove heteroatoms (waters, metals, ligands)
4. Add:

   * Missing side chains
   * Hydrogen atoms
   * Atomic charges
5. Clean and verify structure with backup file

> **Note:** PDBQT generation is optional; reference PDBQT files are provided for downstream analyses to maintain consistency.

**Output:** Cleaned PDB (`6m0j_fixed_step1.pdb`) ready for energy analysis.

---

## 🛠️ Tools Used

* Python 3 + Biopython
* Custom Python scripts for contact detection, energy decomposition, alanine scanning
* **biobb_structure_checking** for structure preparation
* **NACCESS** for ASA calculations
* **FoldX** for comparison
* PyMOL for visualization
* Matplotlib/Seaborn for plotting

---

## 📤 Expected Outputs

* Cleaned and verified PDB files
* List of interface residues per chain
* Per-residue interaction energy tables
* Alanine scanning ΔΔG profiles
* Variant impact ΔΔG analysis
* Classification of hotspots and stabilizers
* Comparison plots and tables with FoldX predictions

---
