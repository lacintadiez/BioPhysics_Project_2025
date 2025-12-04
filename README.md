# 🧬 Energy Analysis Project (2025–26)

### **Spike RBD–ACE2 Interface Energy Analysis**

## 📌 **Objective**

Evaluate the **relative energetic contribution of protein–protein interface residues** in the SARS-CoV-2 Spike RBD–ACE2 complex, using structure **6M0J**.

Specifically, we analyze:

1. **Which residues form the interface**
2. **Per-residue contribution to binding energy**
3. **Impact of mutations at interface positions**
4. **Comparison with FoldX predictions**

---

## 🧠 **Background**

Large-scale sequencing projects generate vast numbers of protein variants. Many proteins function as **complexes**, and small sequence changes can affect:

* Binding affinity
* Stability
* Regulation
* Viral infectivity (in the case of SARS-CoV-2)

The Spike RBD–ACE2 interaction is a key step in the SARS-CoV-2 infection mechanism, targeted by most vaccines and neutralizing antibodies.

This project explores how **each interface residue** contributes to the interaction energy, and how mutations might disrupt or enhance binding.

---

## ⚙️ **Setup Instructions**

### 1. Clone the repository

```bash
git clone https://github.com/lacintadiez/BioPhysics_Project_2025.git
cd BioPhysics_Project_2025
```

---

### 2. Create and activate a Conda environment

```bash
conda create -n biophysics_project python=3.10 -y
conda activate biophysics_project
```

---

### 3. Install required Python packages

```bash
# Core Python libraries
pip install biopython numpy pandas matplotlib jupyter

# Structure checking library
conda install -c bioconda biobb_structure_checking
```

---

### 4. Install ipykernel for Jupyter
```bash
conda install ipykernel
# or
pip install ipykernel
```
#### Then register the environment with Jupyter:
```bash
python -m ipykernel install --user --name biophysics_project --display-name "Python (biophysics_project)"
```

---
### 5. Install and test NACCESS

1. Navigate to the NACCESS folder:

```bash
cd Energy_analysis_project/NACCESS/NACCESS
chmod +x naccess
```

2. Test the executable:

```bash
./naccess -h
```

You should see a message like:

```
usage: you must supply a pdb format file
```
---

### 6. Download PDB structure

```bash
wget https://files.rcsb.org/download/6M0J.pdb
```
---

## 📁 **Input Data**

* **PDB structure:** `6M0J` (provided by the command line above)
* Contains the SARS-CoV-2 Spike **Receptor Binding Domain (RBD)** bound to **ACE2**.

---

# 🔬 **Project Workflow**

## **Step 1 — Identify Interface Residues**

1. Visually inspect the structure using **PyMOL**
2. Define a cutoff distance for contacts:

   * Use the closest atom–atom distance observed
   * Add **1–2 Å** to include adjacent residues
3. Write a **Python script** that:

   * Reads the PDB
   * Computes interatomic distances
   * Outputs a list of **interface residues** for each chain

---

## **Step 2 — Compute Energetic Contributions**

### **A. Per-residue interaction energy**

Estimate the energetic contribution of each interface residue to the complex stability (ΔG contribution).

### **B. Alanine scanning (in silico)**

For each interface residue:

1. Mutate to **Alanine**
2. Recalculate complex interaction energy
3. Compute ΔΔG = ΔG<sub>mutant</sub> – ΔG<sub>wildtype</sub>

---

## **Step 3 — Analyze Spike Variants**

1. Identify known SARS-CoV-2 Spike mutations located in the RBD
2. Introduce mutations into the structure (one at a time)
3. Recompute the interaction energy
4. Compare the energetic impact with WT and Ala-scan results

---

## **Step 4 — FoldX Comparison**

Repeat steps using **FoldX** ([https://foldxsuite.crg.eu/](https://foldxsuite.crg.eu/)) and evaluate:

* Agreement between methods
* Differences in predicted ΔΔG
* Possible sources of error or bias

---

## ⚙️ **Structure Preparation Pipeline**

Using **biobb_structure_checking**:

1. Use the provided 6M0J structure
2. Select the correct biological assembly
3. Remove heteroatoms
4. Add:

   * Missing side chains
   * Hydrogen atoms
   * Atomic charges
5. Generate a **prepared PDBQT** file (CMIP-compatible)

---

## 🛠️ **Tools Used**

* **Python 3**
* **Biopython** or custom scripts for contact detection
* **biobb_structure_checking** for model preparation
* **NACCESS** for ASA calculations
* **FoldX** for comparison
* **PyMOL** for visualization

---

## 📤 **Expected Outputs**

* List of interface residues (per chain)
* Per-residue interaction energy
* Alanine-scanning ΔΔG profile
* Variant impact analysis (ΔΔG)
* Comparison plots between methods
* Summary table of energetically important interface residues

---
