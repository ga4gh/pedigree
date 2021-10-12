# Pedigree conceptual model

## Motivation

Lack of interoperability of pedigree data between systems results in a substantial double-entry burden for already-overloaded nurses, genetic counselors, and support staff, and introduces the potential for serious errors.

# Classes

## Pedigree

A clinical `Pedigree` is curated selection of information about a family, including the individuals, relationships between them, and relevant health conditions.

### properties

| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| proband | ID | optional | id of Individual that is the index case for the family, usually the first person referred to genetics or tested for the condition being investigated |
| date | Date | optional | the date the pedigree was collected or last updated, as ISO full or partial date, i.e. YYYY, YYYY-MM, or YYYY-MM-DD |
| reason | Concept | optional | the reason for pedigree collection, especially a health condition of focus being investigated in the family; if any Individual has the `affected` property defined, it refers to this condition |

## Individual

### properties

| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| id | ID | required | logical id |
| name | string | optional | name of the individual |
| identifiers | list of Identifier | optional | external identifiers for the individual |
| sex | Concept | required | Sex assigned at birth, values: `male`, `female`, `other`, `unknown`; See: https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions and https://www.hl7.org/fhir/valueset-administrative-gender.html |
| gender | Concept | optional | Presumed or reported gender identity, values: `male`, `female`, `non-binary`, `non-disclosed`, `trans`, ...; See: https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions |
| lifeStatus | Concept | recommended | The presumed/accepted life status of the individual as of the pedigree collection date; one of: `alive`, `deceased`, `unborn` |
| affected | boolean | optional | whether or not the individual is affected by the condition being investigated in this pedigree (`Pedigree.reason`) |

## Relationship

### properties
| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| subject | ID | required | identifier of the subject Individual; equivalent to the Biolink "Subject" and similar to the FHIR "Player" and |
| relationship | Concept | required | the relationship the `subject` has to the `relative` (e.g., if the `subject` is the `relative`'s mother, then relation would be `isBiologicalMother`); terms should come from the [KIN terminology](https://github.com/GA4GH-Pedigree-Standard/family_history_terminology) |
| relative | ID | required | identifier of the relative Individual; equivalent to the Biolink "Object" and similar to the FHIR "Scoping Individual" |


## Identifier
This is an identifier for the individual in another system.

### properties
| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| id | string | required | an external identifier such as a medical record number, participant id, or insurance number;  URI/CURIE is preferred |
| label | string | optional | a human-readable label for the identifier |


## Concept
This is a reference to a concept in an ontology/terminology/valueset.

### properties
| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| id | string | required | a CURIE associated with the concept (e.g., `HP:0000118`) |
| label | string | optional | a human-readable label for the concept (e.g., `"Phenotypic abnormality"`) |


## Implementation guidance
Properties to link instances of the classes are intentionally omitted in order to make the model agnostic to either a tabular or a hierarchical representation.


# Supporting formats

## ID
This is a **string** identifier for internal cross-referencing of individuals between the components of a Pedigree.


# TODO
- add Event class for collecting birth, death, conditions, and attribution/provenance for this data
- linking with other terminologies:
  - request that ECO add new term, subclass of ECO_0006151, for family-member assertion (versus self-reported), to distinguish ECO:0006154, in which a patient is giving a statement about themselves, or sibling of ECO_0006153 since it's not self-reported, it's from a family member, for now, can use ECO:0006151 for it
  - request that Biolink add a IndividualOrganismToIndividualOrganismAssociation class
  - request that Phenopacket Disease allow status to represent carrier/asymptomatic/presymptomatic
