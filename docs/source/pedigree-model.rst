########################
Pedigree Model
########################

.. toctree::
   :maxdepth: 1

Overview
========

To support the interoperability of family health history data within and between existing standards (such as HL7 FHIR and Phenopackets), the GA4GH Clinical and PhenoTips Data Capture Workstream developed the Pedigree Conceptual Model.

The Pedigree Conceptual Model defines core classes and their properties, and is based on the [Recommended Common Data Set for Family Health History](https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit).

Compatible standards provide an implementation guide for capturing and representing pedigree data in a manner that is compatible with this model. Current compatible standards are:
* HL7 FHIR
* GA4GH Phenopacket


Pedigree
------------

- ID - Required
- Index patients (proband, consultand, first person tested positive for a particular condition/variant)
- Condition(s) being investigated in the family
- Completion status
- Language
- Narrative
- Date collected/updated


Individual
------------

- Individual id - required
- Sex at birth - required
- Gender
- Name
- DOB
- Age / Age Range / Estimated Age
- REA - suggested list of concepts from HANCESTRO
- Deceased
- Disease/condition: code, onset, contributed to death
- Affected
- other risk-relevant observations


Relationship
------------
- individual - required
- relative - required
- relationship - required, coded using KIN terms


Kinship Ontology (KIN)
========

The Kinship Ontology (KIN) is a family relations ontology developed as part of the Global Alliance for Genomics and Health Pedigree Standard project. It allows using an OWL reasoner to automatically validate a family history graph and infer new relations.

The latest version of the ontology can always be found at: http://purl.org/ga4gh/kin.owl.

The Ontology is open-source and managed in this GitHub repo: https://github.com/GA4GH-Pedigree-Standard/family_history_terminology



Examples
========

The following examples are illustrated with [Cypher](https://neo4j.com/developer/cypher/)-like descriptions for readibility.


Trio
----

```
(Mother)-[isBiologicalMotherOf]->(Child)
(Father)-[isBiologicalFatherOf]->(Child)
```

Twins
-----

Minimum, e.g.:

```
(Mother)-[isBiologicalMotherOf]->(Twin1)
(Father)-[isBiologicalFatherOf]->(Twin1)
(Twin1)-[isMonozygoticTwinOf]->(Twin2)
```


Fully-qualified:

```
(Mother)-[isBiologicalMotherOf]->(Twin1)
(Father)-[isBiologicalFatherOf]->(Twin1)
(Mother)-[isBiologicalMotherOf]->(Twin2)
(Father)-[isBiologicalFatherOf]->(Twin2)
(Twin1)-[isMonozygoticTwinOf]->(Twin2)
(Twin2)-[isMonozygoticTwinOf]->(Twin1)
```


Adoption
-----

```
(Mother)-[isAdoptiveParentOf]->(Child)
(BiologicalMother)-[isBiologicalMotherOf]->(Child)
(Father)-[isBiologicalFatherOf]->(Twin1)
```


IVF
---

```
(Father)-[isSpermDonorOf]->(Child)
(Surrogate)-[isGestationalCarrierOf]->(Child)
(Mother)-[isOvumDonorOf]->(Child)
```

More complex family history
---

TODO
- show, e.g. second degree relative with history of breast cancer



Compatible standards
========


Phenopackets
------------

A Phenopacket implementation guide is currently underway. At the moment, an aligned implementation is defined on a branch of the Phenopacket repository: https://github.com/phenopackets/phenopacket-schema/blob/6262c6d4837ffad0bed20ecf80eb1264a80ded57/src/main/proto/pedigree.proto


HL7 FHIR
------------

The FHIR Implementation Guide is here: https://github.com/GA4GH-Pedigree-Standard/pedigree-fhir-ig


Design motivations
=========

Design motivation
- avoid overlap with other standards (fhir, phenopacket)
- focus on relationship
- graphical model, bringing relationships as top-level entities
- allow for the synthesizing of patient-reported family history data, such as comes out of family history questionnaires and EHR records (and can be represented with the FamilyMemberHistoryResource), and support this information through to risk models
- provide a standard interface for validation
- facilitate conversion among existing standards for pedigree data


Relationships between individuals are standardized using concepts from the newly developed Kinship Ontology.
To allow existing workflows and tools to gracefully add interoperability with this standard, we developed an open-source pedigree data interoperability library, pedigree-tools.
