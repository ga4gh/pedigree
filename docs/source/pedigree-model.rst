########################
Pedigree Model
########################

.. toctree::
   :maxdepth: 1

Overview
========

To support the interoperability of family health history data within and between existing standards (such as HL7 FHIR and Phenopackets), the GA4GH Clinical and PhenoTips Data Capture Workstream developed the Pedigree Conceptual Model.

The Pedigree Conceptual Model defines core classes and their properties, and is based on the `A Recommendation for The Common Data Set for Family Health History <https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit>`_.



Key Concepts
=============

The model defines three core classes:

Pedigree: A collection of information about related individuals and relationships between them.

Individual: A person or entity.

Relationship: A relationship that one individual has with a related individual.


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

A basic family trio consists of one male parent, one female parent, and a proband child. This would be represented as a Pedigree with three Individuals and two parent-child Relationships:

As a Phenopacket `GA4GHPedigree` message:

.. code-block:: yaml
  id: FAM1
  narrative: A Phenopacket GA4GHPedigree of a trio with an affected child
  date: 2022-06-23
  individuals:
    - individual:
        id: MOTHER
        sex: FEMALE
      name: My Mother
      raceEthnicityAncestry:
        - id: HANCESTRO:0316
          label: Amish
    - individual:
        id: FATHER
        sex: MALE
    - individual:
        id: CHILD
        sex: UNKNOWN
      affected: true
  relationships:
    - individual: MOTHER
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: CHILD
    - individual: FATHER
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: CHILD
  indexPatients:
    - id: CHILD
      type: Proband


Twins
----------

The relationship between twins (TWIN1 and TWIN2) can be represented by adding another Individual, parent-child relationships and a twin Relationship to the Pedigree:

.. code-block:: yaml

  id: FAM2
  narrative: A Phenopacket GA4GHPedigree of a couple with identical twins
  date: 2022-06-23
  individuals:
    - individual:
        id: MOTHER
        sex: FEMALE
    - individual:
        id: FATHER
        sex: MALE
    - individual:
        id: TWIN1
        sex: UNKNOWN
    - individual:
        id: TWIN2
        sex: UNKNOWN
  relationships:
    - individual: MOTHER
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: TWIN1
    - individual: FATHER
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: TWIN1
    - individual: TWIN1
      relationship: 
        id: KIN:010
        label: isMonozygoticMultipleBirthSiblingOf
      relative: TWIN2


The parent-child relationships for TWIN2 are not strictly necessary.
Because the `isMonozygoticTwinOf` relationship is symmetric, it would be equally valid to have said that TWIN2 isMonozygoticTwinOf TWIN1.


Adoption
----------


.. code-block:: yaml


  id: FAM3
  narrative: A Phenopacket GA4GHPedigree of a child with an adoptive mother
  date: 2022-06-23
  individuals:
    - individual:
        id: MOTHER
        sex: FEMALE
    - individual:
        id: BIOLOGICAL_MOTHER
        sex: FEMALE
    - individual:
        id: FATHER
        sex: MALE
    - individual:
        id: CHILD
        sex: UNKNOWN
  relationships:
    - individual: MOTHER
      relationship:
        id: KIN:022
        label: isAdoptiveParentOf
      relative: CHILD
    - individual: BIOLOGICAL_MOTHER
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: CHILD
    - individual: FATHER
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: CHILD


IVF
-----

.. code-block:: yaml

  id: FAM4
  narrative: A Phenopacket GA4GHPedigree of a child with an egg donor, gestational carrier, and biological father
  date: 2022-06-23
  individuals:
    - individual:
        id: MOTHER
        sex: FEMALE
    - individual:
        id: SURROGATE
        sex: FEMALE
    - individual:
        id: FATHER
        sex: MALE
    - individual:
        id: CHILD
        sex: UNKNOWN
  relationships:
    - individual: MOTHER
      relationship:
        id: KIN:038
        label: isOvumDonorOf
      relative: CHILD
    - individual: SURROGATE
      relationship:
        id: KIN:005
        label: isGestationalCarrierOf
      relative: CHILD
    - individual: FATHER
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: CHILD


Complete cancer family
---------------------------

.. figure:: images/classic-brca1-pedigree.jpeg
   :alt: BRCA1 pedigree example

   Example BRCA1 pedigree. Source: https://visualsonline.cancer.gov/details.cfm?imageid=10436


.. code-block:: yaml

  id: FAM5
  narrative: A Phenopacket GA4GHPedigree of a classic BRCA1 pedigree
  date: 2022-06-23
  individuals:
    - id: 1
      phenopacket:
        subject:
          sex: MALE
          vital_status: DECEASED
    - id: 2
      phenopacket:
        subject:
          sex: FEMALE
          vital_status: DECEASED
    - id: 3
      phenopacket:
        subject:
          sex: MALE
          vital_status: DECEASED
    - id: 4
      phenopacket:
        subject:
          sex: FEMALE
          vital_status: DECEASED
          diseases:
            - term:
                id: 
                label: Ovarian cancer
              onset:
                age: P49Y
    - id: 5
      phenopacket:
        subject:
          sex: FEMALE
    - id: 6
      phenopacket:
        subject:
          sex: FEMALE
    - id: 7
      phenopacket:
        subject:
          sex: MALE
    - id: 8
      phenopacket:
        subject:
          sex: FEMALE
          diseases:
            - term:
                id: 
                label: Breast cancer
              onset:
                age: P42Y
    - id: 9
      phenopacket:
        subject:
          sex: MALE
    - id: 10
      phenopacket:
        subject:
          sex: FEMALE
    - id: 11
      phenopacket:
        subject:
          sex: FEMALE
          diseases:
            - term:
                id: 
                label: Ovarian cancer
              onset:
                age: P53Y
    - id: 12
      phenopacket:
        subject:
          sex: FEMALE
    - id: 13
      phenopacket:
        subject:
          sex: MALE
    - id: 14
      phenopacket:
        subject:
          sex: FEMALE
    - id: 15
      phenopacket:
        subject:
          sex: FEMALE
          diseases:
            - term:
                id: 
                label: Breast cancer
              onset:
                age: P38Y
  relationships:
    - individual: 1
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 5
    - individual: 2
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 5
    - individual: 1
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 6
    - individual: 2
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 6
    - individual: 1
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 7
    - individual: 2
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 7
    - individual: 3
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 8
    - individual: 4
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 8
    - individual: 3
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 9
    - individual: 4
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 9
    - individual: 3
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 11
    - individual: 4
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 11
    - individual: 3
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 12
    - individual: 4
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 12
    - individual: 7
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 13
    - individual: 8
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 13
    - individual: 9
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 14
    - individual: 10
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 14
    - individual: 7
      relationship: 
        id: KIN:028
        label: isBiologicalFatherOf
      relative: 15
    - individual: 8
      relationship:
        id: KIN:027
        label: isBiologicalMotherOf
      relative: 15
  indexPatients:
    - id: 14
      type: Proband



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
