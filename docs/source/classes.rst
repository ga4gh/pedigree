#######
Classes
#######

The diagram below shows an overview of the pedigree classes. Lines between classes indicate composition.

.. figure:: images/classes.png
   :alt: Overview of classes

Individual
==========

The subject of a **Pedigree** is represented by an **Individual** class. This class intends to represent an individual person or patient who is a member of the pedigree being investigated.  

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
     - Age of the individual
   * - raceEthnicityAncestry
     - 0..1
     - Race, Ethnicity, or Ancestry of the individual
   * - deceased
     - 0..1
     - The presumed/accepted life status of the individual as of the pedigree collection date
   * - affected
     - 0..1
     - Whether or not the individual is affected by the condition being investigated in this pedigree (``Pedigree.reason``)

Relationship
============

The *Relationship* class defines the family relations in a pedigree.

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
========

A clinical **Pedigree** is curated selection of information about a family, including the individuals, relationships between them, and relevant health conditions.

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
   * - reason
     - 0..1
     - The reason for pedigree collection, a health condition of focus being investigated in the family; if any ``Individual`` has the ``affected`` property defined, it refers to this condition.
   * - status
     - 0..1
     - Status of the pedigree resource collection
   * - narrative
     - 0..1
     - Summary of the pedigree resource for human interpretation
   * - date
     - 0..1
     - The date the pedigree was collected or last updated, as ISO full or partial date, *i.e.* ``YYYY``, ``YYYY-MM``, or ``YYYY-MM-DD``
