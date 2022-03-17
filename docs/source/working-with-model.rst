################################################
Working with the Pedigree Model
################################################

.. toctree::
   :maxdepth: 1

Kinship Ontology (KIN)
========================

The Kinship Ontology (KIN) is a family relations ontology developed as part of the Global Alliance for Genomics and Health Pedigree Standard project. It allows using an OWL reasoner to automatically validate a family history graph and infer new relations.

The latest version of the ontology can be found at: http://purl.org/ga4gh/kin.owl.

The Ontology is open-source and managed in this GitHub repo: https://github.com/GA4GH-Pedigree-Standard/family_history_terminology


Pedigree Tools
========================

Pedigree-tools is a library for supporting the conversion of pedigree data between various file formats.

It can currently support importing from the following formats:

- GA4GH Pedigree
- PED/Linkage
- GEDCOM (Cyrillic)
- BOADICEA

In can currently export into the following formats:

- GA4GH Pedigree
- PED/Linkage

This tool is available at the following GitHub repository: https://github.com/GA4GH-Pedigree-Standard/pedigree-tools


Pedigree Validator
========================

This is a simple command line application that shows how validation of a FHIR pedigree file can be implemented using the HAPI FHIR libraries and the artifacts produced by the FHIR implementation guide.

It also shows how an OWL reasoner can be used to implement additional validation based on the KIN ontology.

The application is available at the following GitHub repository: https://github.com/GA4GH-Pedigree-Standard/pedigree-validator


Example Implementations
========================

The following systems have implemented the GA4GH Pedigree Standard:

FHIR implementations:

- `CSIRO Redcap Pedigree Plugin <https://github.com/aehrc/redcap_pedigree_editor>`_ (Open Source)
- `Open Pedigree <https://github.com/phenotips/open-pedigree>`_ (Open Source)

Phenopacket implementations:

- In progress...
