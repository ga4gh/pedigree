# GA4GH Pedigree Standard

The GA4GH Pedigree Standard allows for the computable exchange of family health history as well as representation of larger, more complex families. The collection of specific clinical or genetic data is outside the scope of this deliverable, and would instead be handled by other formats and references to individuals within the pedigree representation. Standardizing the way systems represent family structure will allow patients to share this information more easily between healthcare systems and help software tools use this information to improve genome analysis and diagnosis.

This has been developed as part of the [GA4GH Clinical Phenotype Data Capture Workstream](https://ga4gh-cp.github.io/).

## Documentation
The core documentation can be found at https://pedigree.readthedocs.io/.
Links of Interest:
- [Use case collection](https://docs.google.com/document/d/1i__95wmm3EpVytRD2gngFAXPhUajK2knWOtuHT9r8W8/edit#)
- [Conceptual model](https://pedigree.readthedocs.io/en/latest/pedigree-model.html)
- [Protobuf schema (v1)](src/main/proto/ga4gh/pedigree/v1/)
- [Common dataset](https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit?usp=sharing)
- [Draft FHIR IG](https://github.com/GA4GH-Pedigree-Standard/pedigree-fhir-ig)
- [KIN Ontology](https://github.com/GA4GH-Pedigree-Standard/family_history_terminology)
- [Pedigree Tools](https://github.com/GA4GH-Pedigree-Standard/pedigree-tools)
- [Pedigree Validator](https://github.com/GA4GH-Pedigree-Standard/pedigree-validator)
- [Meeting Minutes](https://docs.google.com/document/d/12gw2BBIPVaWxUNQx2qiVVIt7W0zVOHON_2Ts9yc9fWY/edit?usp=sharing)

## Versioning
The model uses semantic versioning. See https://semver.org for details.

Current version: **v1 (draft)**

### Summary of changes from v0.1 → v1

**Individual**
- `sex` → `sex_assigned_at_birth` (now optional; added categorical values)
- `gender` → `gender_identity` (added categorical values)
- `dateOfBirth` → `date_of_birth` (snake_case rename)
- `populationDescriptors` → `population_descriptors` (snake_case rename)
- `karyotypicSex` removed (represented in linked genotypic data)
- `affected` removed (represented in linked phenotypic data)
- `egg_parent` and `sperm_parent` added (gamete-provider shorthand)

**ExternalIdentifier** (new)
- Links an Individual to identifiers in external systems (EHR, Phenopackets, etc.)

**Relationship**
- `relation` (single KIN term) replaced by:
  - `biological_relationship` (0..*) — KIN biological subset
  - `social_relationship` (0..*) — KIN social subset
- `twin_group` added (monozygotic / dizygotic)
- `consanguinity` and `consanguinity_note` added

**Pedigree**
- `indexPatients` → `index_patients` (snake_case rename)

## Mailing List / Google Group
The GA4GH Pedigree Subgroup has a Google Group/mailing list for those interested in contributing to development of the standard or following along with progress. Please reach out to lindsay.smith@ga4gh.org for more information.
