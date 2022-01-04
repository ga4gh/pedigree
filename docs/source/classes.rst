#######
Classes
#######

Pedigree
========

A clinical **Pedigree** is curated selection of information about a family, including the individuals, relationships between them, and relevant health conditions.

.. list-table:: Properties
   :widths: 15 15 15 55
   :header-rows: 1

   * - Field
     - Type
     - Status
     - Definition
   * - identifiers
     - array of ``Identifier``
     - optional
     - External identifiers for the family
   * - date
     - Date
     - optional
     - The date the pedigree was collected or last updated, as ISO full or partial date, *i.e.* ``YYYY``, ``YYYY-MM``, or ``YYYY-MM-DD``
   * - proband
     - ID
     - optional
     - ID of ``Individual`` that is the index case for the family, usually the first person referred to genetics or tested for the condition being investigated
   * - reason
     - ``Concept``
     - optional
     - The reason for pedigree collection, especially a health condition of focus being investigated in the family; if any ``Individual`` has the ``affected`` property defined, it refers to this condition.
   * - individuals
     - array of ``Individual``
     - optional
     - Collection of the individuals who are the members of this pedigree
   * - relationships
     - array of ``Relationship``
     - optional
     - Collection of relationships between the individuals who are the members of this pedigree

Individual
==========

.. list-table:: Properties
   :widths: 15 15 15 55
   :header-rows: 1

   * - Field
     - Type
     - Status
     - Definition
   * - id
     - ID
     - required
     - Logical ID
   * - name
     - string
     - optional
     - Name of the individual
   * - identifiers
     - array of ``Identifier``
     - optional
     - External identifiers for the individual
   * - sex
     - ``Concept``
     - required
     - Sex assigned at birth, values: ``male``, ``female``, ``other``, ``unknown``; See `Gender Harmony Context Definitions <https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions>`_ and `FHIR AdministrativeGender Value Set <http://hl7.org/fhir/ValueSet/administrative-gender>`_.
   * - gender
     - ``Concept``
     - optional
     - Presumed or reported gender identity, values: ``male``, ``female``, ``non-binary``, ``non-disclosed``, ``trans``, etc.; See `Gender Harmony Context Definitions <https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions>`_.
   * - lifeStatus
     - ``Concept``
     - recommended
     - The presumed/accepted life status of the individual as of the pedigree collection date, values: ``alive``, ``deceased``, ``unborn``
   * - affected
     - boolean
     - optional
     - Whether or not the individual is affected by the condition being investigated in this pedigree (``Pedigree.reason``)

Relationship
============

.. list-table:: Properties
   :widths: 15 15 15 55
   :header-rows: 1

   * - Field
     - Type
     - Status
     - Definition
   * - individual
     - ID
     - required
     - Identifier of the subject ``Individual``; equivalent to the Biolink "subject" and similar to the FHIR "Player"
   * - relation
     - ``Concept``
     - required
     - The relationship the ``individual`` has to the ``relative`` (*e.g.*, if the ``individual`` is the ``relative``'s biological mother, then relation could be ``isBiologicalMotherOf`` ``[KIN:027]``); terms should come from the `KIN Ontology <http://purl.org/ga4gh/kin.owl>`_.
   * - relative
     - ID
     - required
     - Identifier of the relative ``Individual``; equivalent to the Biolink "object" and similar to the FHIR "Scoping Individual"

Identifier
==========

This is an identifier for the individual in another system.

.. list-table:: Properties
   :widths: 15 15 15 55
   :header-rows: 1

   * - Field
     - Type
     - Status
     - Definition
   * - id
     - string
     - required
     - An external identifier such as a medical record number, participant ID, or insurance number; URI/CURIE is preferred.
   * - label
     - string
     - optional
     - A human-readable label for the identifier

Concept
=======

This is a reference to a concept in an ontology/terminology/valueset.

.. list-table:: Properties
   :widths: 15 15 15 55
   :header-rows: 1

   * - Field
     - Type
     - Status
     - Definition
   * - id
     - string
     - required
     - A CURIE associated with the concept (*e.g.*, ``HP:0000118``)
   * - label
     - string
     - optional
     - A human-readable label for the concept (*e.g.*, ``"Phenotypic abnormality"``)
