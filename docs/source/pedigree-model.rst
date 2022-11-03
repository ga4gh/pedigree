########################
Conceptual Model
########################

.. toctree::
   :maxdepth: 1

Overview
========

To support the interoperability of family health history data within and between existing standards (such as HL7 FHIR and Phenopackets), the GA4GH Clinical and PhenoTips Data Capture Workstream developed the Pedigree Conceptual Model.

The Pedigree Conceptual Model defines core concepts and their properties, and is based on `A Recommendation for The Common Data Set for Family Health History <https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit>`_.



Concepts
========

The diagram below shows an overview of the pedigree concepts. Lines between concepts indicate composition.

.. figure:: images/classes.png
   :alt: Overview of concepts


Individual
----------

The **Individual** concept represents an individual person or patient who is a member of the pedigree being investigated.

.. list-table::
   :header-rows: 1

   * - Field
     - Multiplicity
     - Description
   * - id
     - 1..1
     - External identifier for the individual
   * - sex
     - 1..1
     - Sex assigned at birth
   * - karyotypicSex
     - 0..1
     - The chromosomal sex of the individual; See Phenopacket `KaryotypicSex <https://phenopacket-schema.readthedocs.io/en/latest/karyotypicsex.html>`_.
   * - gender
     - 0..1
     - Presumed or reported gender identity
   * - name
     - 0..1
     - Name of the individual
   * - dateOfBirth
     - 0..1
     - Birth date of the individual, can be just birth year in most cases
   * - age
     - 0..1
     - Age of the individual, can be either Age, Estimated Age (or Ontology Class), Age Range, and/or Gestational Age; See also `Phenopackets' TimeElement <https://phenopacket-schema.readthedocs.io/en/latest/time-element.html#rsttimeelement>`_.
   * - populationDescriptors
     - 0..*
     - Information about the individual's ancestry, ethnicity, race, tribe, etc.,; terms from the `Human Ancestry Ontology (HANCESTRO) <https://www.ebi.ac.uk/ols/ontologies/hancestro>`_ are recommended, but freetext must be supported
   * - deceased
     - 0..1
     - The presumed/accepted life status of the individual as of the pedigree collection date
   * - affected
     - 0..1
     - Whether or not the individual is affected


Relationship
------------

The *Relationship* concept represents the relationship that one individual has to another individual.

.. list-table::
   :header-rows: 1

   * - Field
     - Multiplicity
     - Description
   * - individual
     - 1..1
     - Identifier of the subject ``Individual``; equivalent to the Biolink "subject"
   * - relation
     - 1..1
     - The relationship the ``individual`` has to the ``relative`` (*e.g.*, if the ``individual`` is the ``relative``'s biological mother, then relation could be ``isBiologicalMotherOf`` ``[KIN:027]``); terms should come from the `KIN Ontology <http://purl.org/ga4gh/kin.owl>`_.
   * - relative
     - 1..1
     - Identifier of the relative ``Individual``; equivalent to the Biolink "object"


Pedigree
--------

A **Pedigree** is a set of individuals and the relationships between them.

.. list-table::
   :header-rows: 1

   * - Field
     - Multiplicity
     - Description
   * - id
     - 1..1
     - External identifier for the family being investigated
   * - indexPatients
     - 0..*
     - Identified ``Individual`` in the family of a health condition of focus being investigated: ``Proband``, ``Consultand``, ``First Person Tested Positive``
   * - individuals
     - 0..*
     - Collection of ``Individual`` who are the members of this pedigree
   * - relationships
     - 0..*
     - Collection of ``Relationship`` between the ``individuals`` who are the members of this pedigree
   * - status
     - 0..1
     - Status of the pedigree resource collection
   * - narrative
     - 0..1
     - Summary of the pedigree resource for human interpretation
   * - date
     - 0..1
     - The date the pedigree was collected or last updated, as ISO full or partial date, *i.e.* ``YYYY``, ``YYYY-MM``, or ``YYYY-MM-DD``




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
