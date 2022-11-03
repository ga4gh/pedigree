########################
Using the Pedigree Model
########################

.. toctree::
   :maxdepth: 1


Compatible standards
====================

The GA4GH Pedigree standard is a conceptual model and recommendations for transferring family history and pedigree data. It is not a standalone data format, but is intended to be implemented by compatible standards to facilitate the transfer and interoperability of this data.

Compatible standards provide an implementation guide for capturing and representing pedigree data in a manner that is compatible with this model.

The representation of each core concept within each standard is summarized in :doc:`pedigree-model`.

The current list of compatible standards are:

* Phenopackets
* HL7 FHIR


Phenopackets
------------

`The Phenopackets “Implementation Guide” <https://github.com/phenopackets/phenopacket-schema/blob/pedigree/src/main/proto/ga4gh/pedigree/v1/pedigree.proto>`_ - an implementation of the GA4GH pedigree spec which is partly composed of phenopacket-schema messages. It is not ‘part’ of the Phenopackets spec, but sits in its own org.ga4gh.pedigree namespace.

For tools like Exomiser, it is possible to convert to PED format using pedigree-tools and ingest via a Phenopacket.

Phenopackets schema uses protobuf, an exchange format developed in 2008 by Google. It is recommended to review the `Wikipedia page on Protobuf <https://en.wikipedia.org/wiki/Protocol_Buffers>`_ and to `Google’s documentation <https://developers.google.com/protocol-buffers/>`_ for details. This page intends to get curious readers who are unfamiliar with protobuf up to speed with the main aspects of this technology, but it is not necessary to understand protobuf to use the phenopacket or pedigree schemas.

Learn more about the Phenopackets `here <https://phenopacket-schema.readthedocs.io/en/latest/index.html>`_.


HL7 FHIR
--------

`The Pedigree FHIR Implementation Guide <http://purl.org/ga4gh/pedigree-fhir-ig/index.html>`_.

Fast Health Interoperability Resources (FHIR) is a loosely defined base model describing things in healthcare (e.g. Patient, Specimen) and how they relate to each other, developed by Health Level 7 (HL7). The FHIR specification is completely technology agnostic. Thus, it does not depend on programming languages or include things like relational database schemas. It is up to the implementers to decide how to implement the data model (i.e. relational database, nosql database, etc) and RESTful API.

To learn more about FHIR, we recommend you check out the following resources: `HL7.org <http://hl7.org/fhir/index.html>`_, `FHIR Basics <https://smilecdr.com/docs/tutorial_and_tour/fhir_basics.html#fhir-basics>`_, and this excellent `FHIR 101 Jupyter Notebook <https://github.com/NIH-NCPI/fhir-101>`_ developed by NIH Cloud-based Platform Interoperability (NCPI) Working Groups.



Direction of Relationships
==========================

A Relationship defines a relationship between one individual and another, such as `isBiologicalMotherOf` or `isTwinOf`. Only one of the two directions needs to be specified, and it does not matter which.

Symmetric relationships are those where both individuals share the same relationship with one another. These include: `isTwinOf` and `isPartnerOf`.

Non-symmetric relationships are those where the relationship that individual X has to individual Y is not the same as the relationship that individual Y has to individual X. For example, if individual X has relationship `isBiologicalParentOf` to individual Y, then individual Y has relationship `isBiologicalChildOf` individual X.

Because of this inherent flexibility in the way that relationships can be described, there is no single representation for a particular pedigree. However, pedigrees can be represented in a **reduced form**, in which implied relationships are excluded. A pedigree in reduced form:

1. Has explicit parent-child relationships between all parents and their offspring, and they are directed downwards, with the parent as the individual and the child as the relative.
2. Has sibling relationships only when this is not implied by having shared parents, and in the event of multiple siblings, all sibling relationships are defined relative to the same individual
3. Defines all twin relationships relative to the same individual
4. Has partnership relationships only when this is not implied by having shared children
5. Has extended relative relationships only when this is not implied by the previously-defined relationships, and they are directed downwards, with the ancestor as the individual and the descendant as the relative




Pedigree Regulatory & Ethics Disclaimer
=======================================

This model has been designed for use in clinical and research settings. The model may be implemented differently depending on the use cases and setting within which it will be used. While a stand alone regulatory and ethics review has been performed on the model itself, an independent regulatory and ethics review by the implementer may be required depending on the context of use to consider specific issues such as privacy, confidentiality and/or data security and ensure that the model’s implementation and usage is in compliance with applicable legislation and ethical requirements in their jurisdiction. Given that this model is designed to represent family health history data, information which carries potential for personal identification, it is the duty of the implementer to address these risks in the implementation and use of this model. When used in clinical research settings please refer to the Global Alliance for Genomics and Health Policy on `Clinically Actionable Genomic Research Results <https://www.ga4gh.org/wp-content/uploads/GA4GH-Policy-RoR.pdf>`_ for guidance in managing the return of results.
