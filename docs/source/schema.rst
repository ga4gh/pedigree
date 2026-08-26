###########################
Protobuf Schema (v1)
###########################

.. toctree::
   :maxdepth: 1


Overview
========

The GA4GH Pedigree Standard v1 provides a formal `Protocol Buffers (proto3) <https://developers.google.com/protocol-buffers/docs/proto3>`_ schema. This schema:

- Serves as the canonical, language-agnostic definition of the Pedigree data model
- Enables code generation for multiple programming languages (Java, Python, Go, C++, etc.)
- Can be serialized to JSON or binary wire format for compact, efficient data exchange
- Mirrors the namespace conventions used by `Phenopackets v2 <https://github.com/ga4gh/phenopacket-schema>`_

The schema is located at ``src/main/proto/ga4gh/pedigree/v1/`` in this repository.

Package namespace: ``org.ga4gh.pedigree.v1``


Files
=====

``pedigree.proto``
------------------

Defines the top-level ``Pedigree`` message — the primary exchange unit.

Key messages:

- ``Pedigree`` — contains ``id``, ``index_patients``, ``individuals``, ``relationships``, ``status``, ``narrative``, ``date``
- ``PedigreeStatus`` enum — ``ACTIVE``, ``COMPLETED``, ``RETIRED``


``individual.proto``
--------------------

Defines the ``Individual`` and ``ExternalIdentifier`` messages.

Key messages:

- ``Individual`` — all demographic fields plus ``egg_parent_id`` / ``sperm_parent_id`` and an embedded list of ``ExternalIdentifier``
- ``ExternalIdentifier`` — links an individual to a record in an external system (EHR, Phenopacket store, etc.)
- ``SexAssignedAtBirth`` enum — ``ASSIGNED_MALE``, ``ASSIGNED_FEMALE``, ``NOT_ASSIGNED_AT_BIRTH``
- ``GenderIdentity`` enum — ``MAN``, ``WOMAN``, ``NONBINARY_GENDER_DIVERSE``, ``OTHER_GENDER``


``relationship.proto``
----------------------

Defines the ``Relationship`` message.

Key messages:

- ``Relationship`` — links two individuals via ``biological_relationship`` (KIN biological subset), ``social_relationship`` (KIN social subset), ``twin_group``, and ``consanguinity`` fields
- ``TwinType`` enum — ``MONOZYGOTIC``, ``DIZYGOTIC``


``base.proto``
--------------

Defines shared types used across all messages.

Key messages:

- ``OntologyClass`` — a ``{id, label}`` pair for coded terms (e.g., KIN terms, HANCESTRO terms)
- ``TimeElement`` — flexible time representation (timestamp, Age, AgeRange, GestationalAge, or OntologyClass); mirrors `Phenopackets' TimeElement <https://phenopacket-schema.readthedocs.io/en/latest/time-element.html>`_
- ``Age``, ``AgeRange``, ``GestationalAge``


Using the Schema
================

Compile the proto files with the `protoc <https://github.com/protocolbuffers/protobuf>`_ compiler or a build plugin for your language. For example, to generate Python classes::

    protoc \
      --proto_path=src/main/proto \
      --python_out=generated/ \
      ga4gh/pedigree/v1/pedigree.proto \
      ga4gh/pedigree/v1/individual.proto \
      ga4gh/pedigree/v1/relationship.proto \
      ga4gh/pedigree/v1/base.proto

JSON serialization follows the standard `proto3 JSON mapping <https://developers.google.com/protocol-buffers/docs/proto3#json>`_. Field names in JSON use lowerCamelCase by convention (e.g., ``indexPatients``, ``sexAssignedAtBirth``).


Relationship to Phenopackets
============================

The Pedigree v1 proto schema is designed to sit alongside Phenopackets v2. Individuals in a ``Pedigree`` can be linked to their corresponding ``Phenopacket`` or FHIR ``Patient`` records via ``ExternalIdentifier``. The ``TimeElement`` and ``OntologyClass`` base types are intentionally compatible with their Phenopackets equivalents to facilitate joint use.
