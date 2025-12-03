# BioPhysics_Project_2025
Spike RBD–ACE2 Protein-Protein Interface Energy Analysis (Exercise 2025-26)

## 📌 Objective
Evaluate the relative contribution of interface residues to the interaction energy
in the SARS-CoV-2 Spike RBD–ACE2 complex (PDB: 6m0j).  
Steps include interface detection, energy decomposition, Ala-scanning, variant analysis, and comparison with FoldX.

---

## ⚙️ Setup Instructions

### 1. Clone the original BioPhysics repo
git clone https://github.com/lacintadiez/BioPhysics_Project_2025.git
cd BioPhysics_Project_2025

### 2. Create and Activate the Conda Environment
conda create -n biophysics_project python=3.10 -y
conda activate biophysics_project

### 3. Install required Python Packages
#### Core dependencies
pip install biopython numpy pandas matplotlib jupyter
#### Structure preparation library
conda install -c bioconda biobb_structure_checking


