# Pedigree conceptual model

## Motivation

Lack of interoperability of pedigree data between systems results in a substantial double-entry burden for already-overloaded nurses, genetic counselors, and support staff, and introduces the potential for serious errors.

# Classes

![image](https://user-images.githubusercontent.com/4251264/105404643-34c0ce00-5bf8-11eb-8e3b-2ac0821f16c8.png)

## Pedigree

A `Pedigree` is collection of selected information about a family, including the individuals, relationships between them, and relevant health conditions.

### properties

| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| proband | ID | optional | id of Individual that is the index case for the family |
| consultant | ID | optional | id of Individual that is the focus of the current analysis |
| collectedAt | Date | optional | the date the pedigree was collected, as ISO full or partial date, i.e. YYYY, YYYY-MM, or YYYY-MM-DD |
| reasonCollected | Concept | optional | the health condition being investigated in the family; if any Individual has the `affected` property defined, it refers to this condition |

## Individual

### properties

| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| id | ID | required | logical id |
| name | string | optional | name of the individual |
| identifiers | list of string | optional | external identifiers for the individual |
| sex | Concept | optional | Sex for Clinical Use, values: `male`, `female`, `other`, `unknown`; See: https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions and https://www.hl7.org/fhir/valueset-administrative-gender.html |
| gender | Concept | optional | Presumed or reported gender identity, values: `male`, `female`, `non-binary`, `non-disclosed`, `trans`, ...; See: https://confluence.hl7.org/display/VOC/Gender+Harmony+Context+Definitions |
| lifeStatus | Concept | recommended | The presumed/accepted life status of the individual as of the pedigree collection date; one of: `alive`, `deceased`, `unborn` |
| affected | boolean | not recommended | whether or not the individual is affected by the condition being investigated in this pedigree; included for PED backwards compatibility |

## Relationship

### properties
| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| id | ID | required | logical id |
| individual | ID | required | identifier of the subject Individual; equivalent to the FHIR "Scoping Individual" and the Biolink "Object" |
| relative | ID | required | identifier of the relative Individual; equivalent to the FHIR "Player" and the Biolink "Subject" |
| relation | Concept | required | the relationship between `individual` and `relative`; if the relationship is directed, it is the relationship that the relative has to the individual (e.g., individual is the child, relative is the mother, then relation is parent); terms should come from the relationship terminology, below |

## Event

### properties
| Field | Type | Status | Definition |
| --- | --- | --- | --- |
| type | string | required | the type of event, may be a known event type |
| date | Date/Age/Range | optional | the date when the Event is purposed to have happened |
| value | string | optional | a value associated with the event, which varies depending on the `type` |

### Known Event types
| type | Description | Value |
| --- | --- | --- |
| birth | A person's birth | The outcome of the pregnancy, a sublass of [Pregnancy Outcome](http://www.ontobee.org/ontology/NCIT?iri=http://purl.obolibrary.org/obo/NCIT_C90491), e.g.: preterm, multiple, stillbirth, abortion, termination |
| death | A person's death | The cause of death as a concept URI (e.g., a NCIT term for the cancer type) |
| alive | A person is reported to be alive.  | N/A |
| condition | A condition affecting the person  | A concept URI for the condition (e.g., an HPO term) |
| diagnosis | A diagnosis received by the person  | A concept URI for the diagnosis (e.g., an OMIM term) |

## Relationship Terminology

See: https://docs.google.com/spreadsheets/d/1V5i-pbOPgg9_F52lYkQpqYbk3u4MIAc9TNw8aglvLL4/edit?usp=sharing

## Implementation guidance
Properties to link instances of the classes are intentionally omitted in order to make the model agnostic to either a tabular or a hierarchical representation.

# Supporting formats

## Concept
For simplicity, this is just a string URI/CURIE

# TODO
- add attribution/provenance for events
- linking with other terminologies:
  - request that ECO add new term, subclass of ECO_0006151, for family-member assertion (versus self-reported), to distinguish ECO:0006154, in which a patient is giving a statement about themselves, or sibling of ECO_0006153 since it's not self-reported, it's from a family member, for now, can use ECO:0006151 for it
  - request that Biolink add a IndividualOrganismToIndividualOrganismAssociation class
  - request that Phenopacket Disease allow status to represent carrier/asymptomatic/presymptomatic
