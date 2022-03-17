########################
Pedigree Model
########################

.. toctree::
   :maxdepth: 1

Overview
========

To support the interoperability of family health history data within and between existing standards (such as HL7 FHIR and Phenopackets), the GA4GH Clinical and PhenoTips Data Capture Workstream developed the Pedigree Conceptual Model.

The Pedigree Conceptual Model defines core classes and their properties, and is based on the `The Recommended Common Data Set for Family Health History <https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit>`_.



Key Concepts
=============

The model defines three core classes:


Individual
------------

A person or entity.

- Individual id - required
- Sex at birth - required
- Gender
- Name
- DOB
- Age / Age Range / Estimated Age / Gestational Age
- REA - Concept - suggested list of concepts from HANCESTRO
- Deceased
- Disease/condition: code, onset, contributed to death
- Affected: Y/N/?
  - for backwards-compatibility with PED
- Other risk-relevant observations


Relationship
------------

A relationship that one individual has with another relative.

- individual - required
- relationship - required, coded using KIN terms
- relative - required


Pedigree
------------

A collection of information about related individuals and relationships between them.

- ID - Required
- Index patients (proband, consultand, first person tested positive for a particular condition/variant)
  - Individual
  - Type enum: Proband, Consultand, First Person Tested Positive
- Completion status
- Language
- Narrative
- Date collected/updated



Direction of Relationships
==========================

A Relationship defines a relationship between one individual and another, such as `isBiologicalMotherOf` or `isTwinOf`. Only one of the two directions needs to be specified, and it does not matter which.

Symmetric relationships are those where both individuals share the same relationship with one another. These include: `isTwinOf` and `isPartnerOf`.

Non-symmetric relationships are those where the relationship that individual X has to individual Y is not the same as the relationship that individual Y has to individual X. For example, if individual X has relationship `isBiologicalParentOf` to individual Y, then individual Y has relationship `isBiologicalChildOf` individual X.

Because of this inherent flexibility in the way that relationships can be described, we define the notion of a **minimum standard form** for describing a pedigree. A pedigree in minimum standard form:
1. Has explicit parent-child relationships between all parents and their offspring, and they are directed downwards, with the parent as the individual and the child as the relative.
2. Has sibling relationships only when this is not implied by having shared parents, and in the event of multiple siblings, all sibling relationships are defined relative to the same individual
3. Defines all twin relationships relative to the same individual
4. Has partnership relationships only when this is not implied by having shared children
5. Has extended relative relationships only when this is not implied by the previously-defined relationships, and they are directed downwards, with the ancestor as the individual and the descendant as the relative.



Compatible standards
====================

Compatible standards provide an implementation guide for capturing and representing pedigree data in a manner that is compatible with this model.

The representation of each core data element within each standard is summarized in :doc:`classes`.

The current list of compatible standards are:

Phenopackets
  A Phenopacket implementation guide is currently underway. At the moment, an aligned implementation is defined on a branch of the Phenopacket repository: https://github.com/phenopackets/phenopacket-schema/blob/6262c6d4837ffad0bed20ecf80eb1264a80ded57/src/main/proto/pedigree.proto

HL7 FHIR
  The FHIR Implementation Guide is here: https://github.com/GA4GH-Pedigree-Standard/pedigree-fhir-ig


Examples
===========

The following examples demonstrate the way in which pedigrees of various complexity can be represented using the pedigree model.

The precise representation within the context of one of the standards, such as FHIR or Phenopacket.


Basic Trio
------------

A basic family trio consists of one male parent, one female parent, and a child. This would be represented as a Pedigree with three Individuals and two parent-child Relationships:

.. code-block:: yaml

  individuals:
    -
      id: MOTHER
      sex: FEMALE
    -
      id: FATHER
      sex: MALE
    -
      id: CHILD
      sex: UNKNOWN
  relationships:
    -
      individual: MOTHER
      relationship: isBiologicalMotherOf
      relative: CHILD
    -
      individual: FATHER
      relationship: isBiologicalFatherOf
      relative: CHILD



Twins
----------

The relationship between twins (TWIN1 and TWIN2) can be represented by adding another Individual, parent-child relationships and a twin Relationship to the Pedigree:

.. code-block:: yaml

  individuals:
    -
      id: MOTHER
      sex: FEMALE
    -
      id: FATHER
      sex: MALE
    -
      id: TWIN1
      sex: UNKNOWN
    -
      id: TWIN2
      sex: UNKNOWN
  relationships:
    -
      individual: MOTHER
      relationship: isBiologicalMotherOf
      relative: CHILD
    -
      individual: FATHER
      relationship: isBiologicalFatherOf
      relative: CHILD
    -
      individual: TWIN1
      relationship: isMonozygoticTwinOf
      relative: TWIN2


The parent-child relationships for TWIN2 are not strictly necessary.
Because the `isMonozygoticTwinOf` relationship is symmetric, it would be equally valid to have said that TWIN2 isMonozygoticTwinOf TWIN1.


Adoption
----------


.. code-block:: yaml

  individuals:
    -
      id: MOTHER
      sex: FEMALE
    -
      id: BIOLOGICAL_MOTHER
      sex: FEMALE
    -
      id: FATHER
      sex: MALE
    -
      id: CHILD
      sex: UNKNOWN
  relationships:
    -
      individual: MOTHER
      relationship: isAdoptiveParentOf
      relative: CHILD
    -
      individual: BIOLOGICAL_MOTHER
      relationship: isBiologicalMotherOf
      relative: CHILD
    -
      individual: FATHER
      relationship: isBiologicalFatherOf
      relative: CHILD



IVF
-----

.. code-block:: yaml

  individuals:
    -
      id: MOTHER
      sex: FEMALE
    -
      id: SURROGATE
      sex: FEMALE
    -
      id: FATHER
      sex: MALE
    -
      id: CHILD
      sex: UNKNOWN
  relationships:
    -
      individual: MOTHER
      relationship: isOvumDonorOf
      relative: CHILD
    -
      individual: SURROGATE
      relationship: isGestationalCarrierOf
      relative: CHILD
    -
      individual: FATHER
      relationship: isBiologicalFatherOf
      relative: CHILD


Complete cancer family
---------------------------

.. figure:: images/classic-brca1-pedigree.jpeg
   :alt: BRCA1 pedigree example

   Example BRCA1 pedigree. Source: https://visualsonline.cancer.gov/details.cfm?imageid=10436


.. code-block:: yaml

  index:
    -
      id: 14
      type: proband
  individuals:
    -
      id: 1
      sex: MALE
      deceased: true
    -
      id: 2
      sex: FEMALE
      deceased: true
    -
      id: 3
      sex: MALE
      deceased: true
    -
      id: 4
      sex: FEMALE
      deceased: true
      attributes:
        -
          term: Ovarian cancer
          ageAtDiagnosis: 49 yrs
    -
      id: 5
      sex: FEMALE
    -
      id: 6
      sex: FEMALE
    -
      id: 7
      sex: MALE
    -
      id: 8
      sex: FEMALE
      attributes:
        -
          term: Breast cancer
          ageAtDiagnosis: 42 yrs
    -
      id: 9
      sex: MALE
    -
      id: 10
      sex: FEMALE
    -
      id: 11
      sex: FEMALE
      attributes:
        -
          term: Ovarian cancer
          ageAtDiagnosis: 53 yrs
    -
      id: 12
      sex: FEMALE
    -
      id: 13
      sex: MALE
    -
      id: 14
      sex: FEMALE
    -
      id: 15
      sex: FEMALE
      attributes:
        -
          term: Breast cancer
          ageAtDiagnosis: 38 yrs
  relationships:
    -
      individual: 1
      relationship: isBiologicalFatherOf
      relative: 5
    -
      individual: 2
      relationship: isBiologicalMotherOf
      relative: 5
    -
      individual: 1
      relationship: isBiologicalFatherOf
      relative: 6
    -
      individual: 2
      relationship: isBiologicalMotherOf
      relative: 6
    -
      individual: 1
      relationship: isBiologicalFatherOf
      relative: 7
    -
      individual: 2
      relationship: isBiologicalMotherOf
      relative: 7
    -
      individual: 3
      relationship: isBiologicalFatherOf
      relative: 8
    -
      individual: 4
      relationship: isBiologicalMotherOf
      relative: 8
    -
      individual: 3
      relationship: isBiologicalFatherOf
      relative: 9
    -
      individual: 4
      relationship: isBiologicalMotherOf
      relative: 9
    -
      individual: 3
      relationship: isBiologicalFatherOf
      relative: 11
    -
      individual: 4
      relationship: isBiologicalMotherOf
      relative: 11
    -
      individual: 3
      relationship: isBiologicalFatherOf
      relative: 12
    -
      individual: 4
      relationship: isBiologicalMotherOf
      relative: 12
    -
      individual: 7
      relationship: isBiologicalFatherOf
      relative: 13
    -
      individual: 8
      relationship: isBiologicalMotherOf
      relative: 13
    -
      individual: 9
      relationship: isBiologicalFatherOf
      relative: 14
    -
      individual: 10
      relationship: isBiologicalMotherOf
      relative: 14
    -
      individual: 7
      relationship: isBiologicalFatherOf
      relative: 15
    -
      individual: 8
      relationship: isBiologicalMotherOf
      relative: 15



Design motivations
==================

Design motivation:

- avoid overlap with other standards (fhir, phenopacket)
- focus on relationship
- graphical model, bringing relationships as top-level entities
- allow for the synthesizing of patient-reported family history data, such as comes out of family history questionnaires and EHR records (and can be represented with the FamilyMemberHistoryResource), and support this information through to risk models
- provide a standard interface for validation
- facilitate conversion among existing standards for pedigree data


Relationships between individuals are standardized using concepts from the newly developed Kinship Ontology.
To allow existing workflows and tools to gracefully add interoperability with this standard, we developed an open-source pedigree data interoperability library, pedigree-tools.
