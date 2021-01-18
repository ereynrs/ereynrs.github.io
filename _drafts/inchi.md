# InChI and InChI Keys

## InChI

The IUPAC International Chemical Identifier *is a textual identifier for chemical substances, designed to provide a standard way to encode molecular information and to facilitate the search for such information in databases and on the web*. InChIs can be seen as akin to a general and extremely formalized version of IUPAC names.

Initially developed by IUPAC (International Union of Pure and Applied Chemistry) and NIST (National Institute of Standards and Technology) from 2000 to 2005, the format and algorithms are non-proprietary. The continuing development of the standard has been supported since 2010 by the not-for-profit InChI Trust, of which IUPAC is a member.

The InChI algorithm converts input structural information into a unique InChI identifier in a three-step process: normalization (to remove redundant information), canonicalization (to generate a unique number label for each atom), and serialization (to give a string of characters).

In January 2009 the 1.02 version of the InChI software was released. This provided a means to generate so called *standard InChI*, which does not allow for user selectable options in dealing with the stereochemistry and tautomeric layers of the InChI string. 

The current software version is 1.06 and was released in December 2020. Prior to 1.04, the software was freely available under the open-source LGPL license, but it now uses a custom license called IUPAC-InChI Trust License.

### Format
Every InChI starts with the string ```InChI=``` followed by the version number, currently 1. If the InChI is standard, this is followed by the letter ```S``` for standard InChIs, which is a fully standardized InChI flavor maintaining the same level of attention to structure details and the same conventions for drawing perception. 

The remaining information is structured as a sequence of layers and sub-layers, with each layer providing one specific type of information. The layers and sub-layers are separated by the delimiter ```/``` and start with a characteristic prefix letter (except for the chemical formula sub-layer of the main layer).

### InChIs vs CAS
InChIs differ from the widely used CAS registry numbers in three respects:
1. they are freely usable and non-proprietary,
2. they can be computed from structural information and do not have to be assigned by some organization, and
3. most of the information in an InChI is human readable.

### InChIs vs SMILES
The InChIs
1. can express more information than the simpler SMILES notation, and
2. and differ from SMILES in that every structure has a unique InChI string, which is important in database applications.

## InChI Keys
The InChIKey, sometimes referred to as a hashed InChI, is a fixed length (27 character) condensed digital representation of the InChI that is not human-understandable. It is a hashed version of the full InChI (using the SHA-256 algorithm), and its [specification](https://web.archive.org/web/20071030202540/http://www.iupac.org/inchi/release102.html) was released in September 2007 in order to facilitate web searches for chemical compounds, since these were problematic with the full-length InChI. The full InChI turned out to be too lengthy for easy searching, and therefore the InChIKey was developed. 

**The adoption is not straightforward, and many databases show a discrepancy between the chemical structures and the InChI they contain, which is a problem for linking databases, as depicted in this [article](https://doi.org/10.1186%2F1758-2946-4-35).**

### InChI Keys resolvers
As the InChI cannot be reconstructed from the InChIKey, an InChIKey always needs to be linked to the original InChI to get back to the original structure. InChI Resolvers act as a lookup service to make these links, and prototype services are available from National Cancer Institute, the UniChem service at the European Bioinformatics Institute, and PubChem.

### Format
The InChIKey consists of three parts separated by hyphens, of 14, 10 and one character(s), respectively, like ```XXXXXXXXXXXXXX-YYYYYYYYFV-P```. The last two letters of the second part stand for:
* a single character indicating the kind of InChIKey (```S``` for standard and ```N``` for nonstandard), and
* a character indicating the version of InChI used (currently ```A``` for version 1). 

### InChI Keys vs InChis
However, there is a very small, but nonzero chance of two different molecules having the same InChIKey. **Unlike the InChI, the InChIKey is not unique: though collisions can be calculated to be very rare, [they happen](http://chem-bla-ics.blogspot.nl/2011/09/inchikey-collision-diy-copypastables.html).**

The probability for duplication of only the first 14 characters has been estimated as only one duplication in 75 databases each containing one billion unique structures. With all databases currently having below 50 million structures, such duplication appears unlikely at present. A recent [study](https://doi.org/10.1186%2F1758-2946-4-39) more extensively studies the collision rate finding that the experimental collision rate is in agreement with the theoretical expectations.







