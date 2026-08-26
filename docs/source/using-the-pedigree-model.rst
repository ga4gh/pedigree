###########################
Using the Pedigree Standard
###########################

.. toctree::
   :maxdepth: 1


Compatible standards
====================

The GA4GH Pedigree Standard is a conceptual model and recommendations for transferring family history and pedigree data. It is not a standalone data format, but is intended to be implemented by compatible standards to facilitate the transfer and interoperability of this data.

Compatible standards provide an implementation guide for capturing and representing pedigree data in a manner that is compatible with this model.

The representation of each core concept within each standard is summarized in :doc:`pedigree-model`.

The current list of compatible standards are:

* Phenopackets
* HL7 FHIR


Phenopackets
------------

The GA4GH Pedigree Standard v1 defines its own canonical :doc:`proto3 schema <schema>` (``org.ga4gh.pedigree.v1``), which is designed to sit alongside Phenopackets v2. Individuals in a ``Pedigree`` can be linked to their corresponding Phenopacket records via ``ExternalIdentifier``. The ``TimeElement`` and ``OntologyClass`` base types are intentionally compatible with their Phenopackets equivalents.

For tools like Exomiser, it is possible to convert to PED format using pedigree-tools and ingest via a Phenopacket.

The Pedigree proto schema uses protobuf 3, the same exchange format used by Phenopackets. It is recommended to review the `Wikipedia page on Protobuf <https://en.wikipedia.org/wiki/Protocol_Buffers>`_ and `Google’s documentation <https://developers.google.com/protocol-buffers/>`_ for background. See :doc:`schema` for instructions on compiling the schema for your language.

Learn more about Phenopackets `here <https://phenopacket-schema.readthedocs.io/en/latest/index.html>`_.


HL7 FHIR
--------

**Note:** Our FHIR-based Implementation Guide of the GA4GH Pedigree conceptual model is under development. The website linked above states the Guide is a “Local Development Build (v0.1.0)”. As the development proceeds, all artifacts in the GA4GH Pedigree specification will be assigned a `"Maturity Level" <https://build.fhir.org/versions.html#maturity>`_. When completed, this IG will go through the HL7 balloting process to become part of the normative version of the FHIR standard.

`The Pedigree FHIR Implementation Guide <http://purl.org/ga4gh/pedigree-fhir-ig/index.html>`_

Fast Health Interoperability Resources (FHIR) is a loosely defined base model describing things in healthcare (e.g. Patient, Specimen) and how they relate to each other, developed by Health Level 7 (HL7). The FHIR specification is completely technology agnostic. Thus, it does not depend on programming languages or include things like relational database schemas. It is up to the implementers to decide how to implement the data model (i.e. relational database, nosql database, etc) and RESTful API.

To learn more about FHIR, we recommend you check out the following resources: `HL7.org <http://hl7.org/fhir/index.html>`_, `FHIR Basics <https://smilecdr.com/docs/tutorial_and_tour/fhir_basics.html#fhir-basics>`_, and this excellent `FHIR 101 Jupyter Notebook <https://github.com/NIH-NCPI/fhir-101>`_ developed by NIH Cloud-based Platform Interoperability (NCPI) Working Groups.



Tree-based vs. Relationship-based Fields
==========================================

The pedigree model provides two complementary mechanisms for encoding how individuals are connected. Understanding the distinction is important for producing consistent, interoperable pedigrees.

Tree-based fields
------------------

The **tree-based fields** are ``egg_parent`` and ``sperm_parent`` on the ``Individual`` concept. Each field holds the identifier of another individual in the pedigree:

- ``egg_parent`` — the individual who contributed the egg (genetic maternal parent).
- ``sperm_parent`` — the individual who contributed the sperm (genetic paternal parent).

Together these two fields encode the core biological parent-child tree directly on each individual. They are compact, unambiguous, and machine-traversable — a complete biological family tree can be represented with no ``Relationship`` records at all.

**Tree-based fields should be used whenever biological parentage is known.** They are the preferred representation for the biological parent-child graph.

Relationship-based fields
--------------------------

The **relationship-based fields** are ``biological_relationship`` and ``social_relationship`` on the ``Relationship`` concept. Each field holds one or more typed relationship terms drawn from the :doc:`KIN Ontology <kin>`:

- ``biological_relationship`` — biological relationships beyond simple parentage: gestational carriers, sperm or egg donors, mitochondrial donors, biological siblings, grandparents, cousins, etc.
- ``social_relationship`` — social or legal relationships: adoptive parents, foster parents, step-parents, step-siblings, partners, etc.

Relationship-based fields express relationships that the tree-based fields cannot: non-parental biological relationships, social/legal relationships, extended-family relationships, and cases where multiple relationship types apply simultaneously to the same pair of individuals (e.g., a gestational carrier who later adopts the child).

Preference and Precedence
--------------------------

#. **Prefer tree-based fields for biological parentage.** Set ``egg_parent`` and ``sperm_parent`` whenever the biological parents are known. This is the minimal, canonical representation of the biological tree.

#. **Use relationship-based fields in addition or as an alternative** when tree-based fields are insufficient (e.g., to record a gestational carrier, a social relationship, or an extended-family link).

#. **The two representations must be consistent.** If both ``egg_parent`` on an ``Individual`` and a corresponding ``biological_relationship`` on a ``Relationship`` are present for the same pair, they must agree. **When in doubt, the tree-based field takes precedence.**


Direction of Relationships
==========================

A ``Relationship`` is directed: the ``individual`` field holds the subject and the ``relative`` field holds the object. For a given KIN term, this determines the direction of the assertion (e.g., ``individual: A, relative: B, biological_relationship: isBiologicalParentOf`` means A is a parent of B, not the reverse).

Symmetric relationships — twin relationships (``twin_group``) and partner relationships — have no meaningful direction.

For non-symmetric relationships, the following priority order is recommended:

1. **Proband-ascending (highest priority).** Relationships connecting the proband to their ancestors should be expressed in the ascending direction, with the proband as ``individual`` and the ancestor as ``relative`` (e.g., ``individual: proband, relative: mother, biological_relationship: isBiologicalChildOf``). This matches the most common clinical data collection flow, where a patient reports their own relatives.

2. **Downward / ancestor-first (default).** For all other relationships — or when no proband is defined — express them in the downward direction, with the ancestor as ``individual`` and the descendant as ``relative`` (e.g., ``individual: grandmother, relative: proband, biological_relationship: isBiologicalGrandparentOf``).

3. **Consistent within the pedigree (minimum requirement).** If neither of the above can be applied uniformly, at minimum ensure all non-symmetric relationships within a single ``Pedigree`` are expressed in the same direction (either all ascending or all descending). Mixed directions within a pedigree should be avoided as they complicate traversal and validation.

Because of this inherent flexibility in the way that relationships can be described, there is no single canonical representation for a particular pedigree. However, pedigrees can be represented in a **reduced form**, in which implied relationships are excluded. A pedigree in reduced form:

1. Has explicit parent-child relationships between all parents and their offspring, directed according to the priority order above.
2. Has sibling relationships only when not implied by shared parents; in the event of multiple siblings, all sibling relationships are defined relative to the same individual.
3. Defines all twin relationships (via ``twin_group``) relative to the same individual.
4. Has partnership relationships only when not implied by shared children.
5. Has extended relative relationships only when not implied by the previously-defined relationships, directed according to the priority order above.

Biological vs. Social Relationships
-------------------------------------

The ``biological_relationship`` and ``social_relationship`` fields can each hold multiple values, allowing the model to simultaneously represent distinct relationship types for the same pair of individuals. For example, a gestational carrier (``biological_relationship: isGestationalCarrierOf``) who later formally adopts the child (``social_relationship: isAdoptiveParentOf``) can be expressed on a single ``Relationship`` record without conflating the two.

See :doc:`kin` for guidance on which KIN terms belong in each field.

Consanguinity
-------------

Consanguinity between any two individuals can be flagged directly on a ``Relationship`` using the ``consanguinity`` field (boolean) and the optional ``consanguinity_note`` for free-text detail. This avoids the need to infer consanguinity from the graph topology alone.




Pedigree Regulatory & Ethics Disclaimer
=======================================

This model has been designed for use in clinical and research settings. The model may be implemented differently depending on the use cases and setting within which it will be used. While a stand alone regulatory and ethics review has been performed on the model itself, an independent regulatory and ethics review by the implementer may be required depending on the context of use to consider specific issues such as privacy, confidentiality and/or data security and ensure that the model’s implementation and usage is in compliance with applicable legislation and ethical requirements in their jurisdiction. Given that this model is designed to represent family health history data, information which carries potential for personal identification, it is the duty of the implementer to address these risks in the implementation and use of this model. When used in clinical research settings please refer to the Global Alliance for Genomics and Health Policy on `Clinically Actionable Genomic Research Results <https://www.ga4gh.org/wp-content/uploads/GA4GH-Policy-RoR.pdf>`_ for guidance in managing the return of results.
