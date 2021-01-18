# SMILES strings notes

The *Simplified Molecular-Input Line-Entry System* (SMILES) is a *specification in the form of a line notation for describing the structure of chemical species using short ASCII strings*. SMILES strings can be imported by most molecule editors for conversion back into two-dimensional drawings or three-dimensional models of the molecules. 

**Typically, a number of equally valid SMILES strings can be written for a molecule.** For example, CCO, OCC and C(O)C all specify the structure of ethanol. 

The original SMILES specification was initiated in the 1980s. It has since been modified and extended. In 2007, an open standard called OpenSMILES was developed in the open-source chemistry community. There are also other linear notations. e.g, the Wiswesser line notation (WLN), ROSDAL, and SYBYL Line Notation (SLN).

**The terms *canonical* and *isomeric* SMILES can lead to some confusion. The terms describe different attributes of SMILES strings and are not mutually exclusive.**

## Cannonicalization
Algorithms have been developed to generate the same SMILES string for a given molecule; of the many possible strings, these algorithms choose only one of them. This SMILES is unique for each structure, *although dependent on the canonicalization algorithm used to generate it*, and is termed the canonical SMILES. 

These algorithms first convert the SMILES to an internal representation of the molecular structure; an algorithm then examines that structure and produces a unique SMILES string. Various algorithms for generating canonical SMILES have been developed and include those by Daylight Chemical Information Systems, OpenEye Scientific Software, MEDIT, Chemical Computing Group, MolSoft LLC, and the Chemistry Development Kit. 
A common application of canonical SMILES is indexing and ensuring uniqueness of molecules in a database.

The original paper that described the CANGEN[2] algorithm claimed to generate unique SMILES strings for graphs representing molecules, but the algorithm fails for a number of simple cases (e.g. cuneane, 1,2-dicyclopropylethane) and cannot be considered a correct method for representing a graph canonically. There is currently no systematic comparison across commercial software to test if such flaws exist in those packages.

## Isomerization
SMILES notation allows the specification of configuration at tetrahedral centers, and double bond geometry. These are structural features that cannot be specified by connectivity alone, and therefore SMILES which encode this information are termed isomeric SMILES. A notable feature of these rules is that they allow rigorous partial specification of chirality. The term isomeric SMILES is also applied to SMILES in which isomers are specified.

*Source: [Wikipedia](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system)*