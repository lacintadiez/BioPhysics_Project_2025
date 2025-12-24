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

### **Step 4 — PyMOL images**


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

# **Step 5 : Build Mutated RBD Structures**

In this step, the mutations of SARS-CoV-2 RBD variants (Alpha, Beta, Delta) were modeled using **PyMOL** to generate structures with replaced side chains for energy analysis.

## Known RBD Mutations by Variant

- **Alpha (B.1.1.7):** N501Y, A570D, P681H
- **Beta (B.1.351):** K417N, E484K, N501Y
- **Delta (B.1.617.2):** L452R, T478K, P681R

## Mutations Used in This Step

Only the mutations present in our dataset were modeled:

- **Alpha:** N501Y → Asparagine (N) replaced by Tyrosine (Y), introducing a larger aromatic side chain that may affect local stability and ACE2 binding.
- **Beta:** K417N → Lysine (K) to Asparagine (N), changing a positively charged residue to a polar uncharged residue.
- **Beta:** E484K → Glutamic acid (E) to Lysine (K), changing a negatively charged residue to a positive one, potentially affecting ACE2 and antibody interactions.
- **Beta:** N501Y → Asparagine (N) to Tyrosine (Y), same as in Alpha.
- **Delta:** T478K → Threonine (T) to Lysine (K), introducing a positive charge in the binding interface.

## Procedure in PyMOL

1. Load the wild-type RBD structure.
2. Use the **Mutagenesis Wizard** to replace each residue with the corresponding mutant, selecting the optimal rotamer.

**Note:** Other characteristic mutations of these variants (A570D, P681H/R, L452R) were not included because they are not present in the analyzed dataset. Alternative tools like `biobb_structure_checking` could automate this process.

---
## 🛠️ Tools Used

* Python 3 + Biopython
* Custom Python scripts for contact detection, energy decomposition, alanine scanning
* **biobb_structure_checking** for structure preparation
* **NACCESS** for ASA calculations
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
