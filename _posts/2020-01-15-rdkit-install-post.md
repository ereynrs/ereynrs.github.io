---
layout: post
title:  How to install RDKit on Windows 10
---

# How to install RDKit on Windows 10

## Step 1: 
### Installing Anaconda
**Note:** Anaconda is not a requirement for RDKit installation , but it eases it a lot!
* Install Anaconda from [here](https://repo.anaconda.com/archive/Anaconda3-2020.11-Windows-x86_64.exe)

## Step 2: 
### Installing RDKit
* Start Anaconda Prompt
* Run the following commands to create the rdkitenv and install the rdkit and dependencies on it.
```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
conda create -c conda-forge -n rdkitenv rdkit 
```
* Run the following commands to activate the rdkit env
```bash
conda activate rdkitenv
```
* Start Python and run this chuck of code to check the installation
```python
import rdkit
rdkit.__version__
```
```
‘2020.09.3’
```

## Bonus track step:
### Transforming Nintedanib SMILES code to InChI Key.
* Run this chunk of Python code
```python
from rdkit import Chem
smiles="COC(=O)c1ccc2c(c1)NC(=O)/C2=C(\\Nc1ccc(N(C)C(=O)CN2CCN(C)CC2)cc1)c1ccccc1"
mol = Chem.MolFromSmiles(smiles)
Chem.MolToInchiKey(mol)
```
```
'XZXHXSATPCNXJR-ZIADKAODSA-N'
```
