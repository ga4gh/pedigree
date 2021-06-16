# GA4GH Pedigree Standard

## What is the Pedigree Standard?

The GA4GH Pedigree Standard allows for the computable exchange of family health history as well as representation of larger, more complex families. The collection of specific clinical or genetic data is outside the scope of this deliverable, and would instead be handled by other formats and references to individuals within the pedigree representation.

## How did the Pedigree Standard come about?

The need for high quality, unambiguous, computable pedigree and family information is critical for scaling genomic analysis to larger, complex families. Pedigree data is currently represented in heterogeneous formats that frequently result in the use of lowest-common-denominator formats (e.g., PED) or custom JSON formats for data transfer. The HL7 FHIR standard core data models do not support pedigrees, but there is a draft extension to support genomic pedigrees that should be evaluated and potentially extended by the GA4GH. Standardizing the way systems represent family structure will allow patients to share this information more easily between healthcare systems and help software tools to use this information to improve genome analysis and diagnosis. 

We asked our stakeholders our about their use of family health history and pedigree data - How are you using it? How is it stored? What do you wish you could do with your data that you currently can't? The results of the survey can be [found here](https://docs.google.com/presentation/d/17r-WyVEpl57i0wxnJcgTkmuWclQovMRKkgqJrkWC4Yo/edit?usp=sharing). A significant percentage of respondents were using a non-computable or interoperable format, and there was no common tool or format with they intede to import or export data. Importantly, 57% of respondents were experiencing challenges with standardization, including lack of computability and integration with analysis tools, and inability to represent complex families and share data easily.

## Why not PED?

The [PED format](https://zzz.bwh.harvard.edu/plink/data.shtml) is a simple text file with 6 columns - IDs, a binary sex field, the phenotype (singular) and SNP genotypes. You can represent a basic parent-child trio, and that may cover a lot of use cases. However, you can't represent twins, things like adoption or donors, pregnancy, vital status, multiple phenotypes and data provenance. All of this type of data is important for genetic counseling and risk assessments where richer representations of relationships are valuable.

The GA4GH Pedigree Standard will natively incorporate PED to enable interoperability with legacy tools.

## The GA4GH Pedigree Model

![pedgraphmodel](https://user-images.githubusercontent.com/48133386/122280389-24ff9a00-ceb7-11eb-8ae1-b7374615f74e.png)
1. Superset of 6-column PED format
2. Optional genderless relationship vocabulary distinguishes biological and social relationships
3. Graph structure allows specifying arbitrary relationships
4. Easy-to-use within the context of other standards such as FHIR or Phenopackets
5. Simplifies converting a genetic family history from one proband to another

The GA4GH Pedigree Standard is a graph-based model, a directed graph where nodes are the individuals and the arrows or edges represent relationships between pairs of those indiviudals. This is distinct from node information that corresponds to each individual separately. Separating these two things is important for extensibility and building out to more complex families, and for including more data elements than if you had something more indiviual-centric like PED.

## Blue Sky Architechture

![pedecosystem](https://user-images.githubusercontent.com/48133386/122281412-3bf2bc00-ceb8-11eb-9aed-9b4a931383f5.png)

## Links of Interest
- [Use case collection](https://docs.google.com/document/d/1i__95wmm3EpVytRD2gngFAXPhUajK2knWOtuHT9r8W8/edit#)
- [Draft conceptual model](https://github.com/GA4GH-Pedigree-Standard/pedigree/blob/master/model.md)
- [Draft minimal dataset](https://docs.google.com/document/d/1UAtSLBEQ_7ePRLvDPRpoFpiXnl6VQEJXL2eQByEmfGY/edit)
- [Draft FHIR IG](https://github.com/GA4GH-Pedigree-Standard/pedigree-fhir-ig)
- [Meeting Minutes](https://docs.google.com/document/d/12gw2BBIPVaWxUNQx2qiVVIt7W0zVOHON_2Ts9yc9fWY/edit?usp=sharing)
