###########################################
Introduction to the GA4GH Pedigree Standard
###########################################

.. toctree::
   :maxdepth: 1

What is the GA4GH Pedigree Standard
======

.. _reference 1:

The GA4GH Pedigree Standard allows for the computable exchange of family health history as well as representation of larger, more complex families. The collection of specific clinical or genetic data is outside the scope of this deliverable, and would instead be handled by other formats and references to individuals within the pedigree representation.

How did the Pedigree Standard Come About
======

The need for high quality, unambiguous, computable pedigree and family information is critical for scaling genomic analysis to larger, complex families. Pedigree data is currently represented in heterogeneous formats that frequently result in the use of lowest-common-denominator formats (e.g., PED) or custom JSON formats for data transfer. The HL7 FHIR standard core data models do not support pedigrees, but there is a draft extension to support genomic pedigrees that should be evaluated and potentially extended by the GA4GH. Standardizing the way systems represent family structure will allow patients to share this information more easily between healthcare systems and help software tools to use this information to improve genome analysis and diagnosis. 

We asked our stakeholders our about their use of family health history and pedigree data - How are you using it? How is it stored? What do you wish you could do with your data that you currently can't? The results of the survey can be `found here <https://docs.google.com/presentation/d/17r-WyVEpl57i0wxnJcgTkmuWclQovMRKkgqJrkWC4Yo/edit?usp=sharing>`_. A significant percentage of respondents were using a non-computable or non-interoperable format, and there was no common tool or format with they intede to import or export data. Importantly, 57% of respondents were experiencing challenges with standardization, including lack of computability and integration with analysis tools, and inability to represent complex families and share data easily.

Why PED is Not Enough
======

The `PED format <https://zzz.bwh.harvard.edu/plink/data.shtml>`_ is a simple text file with 6 columns - IDs, a binary sex field, the phenotype (singular) and SNP genotypes. You can represent a basic parent-child trio, and that may cover a lot of use cases. However, you can't represent twins, things like adoption or donors, pregnancy, vital status, multiple phenotypes and data provenance. All of this type of data is important for genetic counseling and risk assessments where richer representations of relationships are valuable.

The GA4GH Pedigree Standard will natively incorporate PED to enable interoperability with legacy tools.

Example Use Cases
======

A full listing of the use cases that informed development can be `reviewed here <https://docs.google.com/document/d/1i__95wmm3EpVytRD2gngFAXPhUajK2knWOtuHT9r8W8/edit?usp=sharing>`_.

* Diagnostic genomic testing across a range of rare disease groups, with de-identified data from unsolved patients progressing into discovery research (data from solved cases also stored in research environment). Majority of testing undertaken as singletons, <5% as trios, other family configurations extremely rare (parent/child duo, sib pair, half-sib pair, quad). Pedigree information is required to inform clinical and research genomic data analysis.
* Seamlessly share collected clinical and family health history information with bioinformatics systems and research environments (or other services) to unambiguously document relationships between sequenced individuals to support joint calling of variants and filtering of variants based on segregation, as well as describing wider family history (re: non-sequenced individuals).
* Using family health history, genotype, and phenotype results of a patient or relative, to determine if the patient needs further testing or sequence analysis, and/or if a relative needs the same
* It is difficult to predict all of the secondary uses of this information and so having it in a programmatic standard that people can consume across a number of resources in both a format for analysis as well as for building algorithms and tools over would be of high utility.
* Link multiple individuals within same pedigree.
* Describe multiple phenotypic/diagnostic/genetic features per individual.
* Robustly represent relationships necessary for counseling (e.g., adoption), risk assessment (e.g., infertility, miscarriage, health history), and assisted reproduction (e.g., IVF, MRT).

.. image:: https://user-images.githubusercontent.com/48133386/152238295-c0e29021-ea0b-44b7-aef1-e7520e776ad1.png
The GA4GH Pedigree Ecosystem

Recommended Dataset Document
======

The collection and use of family health histories span medical activities from genetic research to heritable risk assessment in patient care. For all the stakeholders in this process, the goal must be data that is accurate and coded for effective analysis, and transferable between systems. To achieve this, a globally accepted and universally implemented family health history (FHH) data set should be established as a benchmark. The purpose of the recommended dataset document is to create an updated minimum data set that can be used not only in both research and clinical settings, but to eliminate the gap between the two disciplines. This recommendation should also guide the development of research, clinical, and patient-facing FHH data and information collection tools, applications, and data repositories. This document should only be used as informative.

`Recommended Dataset Document <https://docs.google.com/document/d/1GQRd5jeZeB5qhHclLZxDe6kPD173bXWGYlTsmCbTeuI/edit?usp=sharing>`_

This work was inspired by the efforts of the Personalized Health Care Workgroup of the American Health Information Community, which first released its recommendation on a core family health history (FHH) minimum data set on October 25, 2007. A `peer-reviewed paper <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2585527/>`_ was published in December 2008.

Requirement Levels
======

The Pedigree model uses two requirement levels. 

Required
########

If a field is required, its presence is  an absolute requirement of the specification, failing which the entire
model is regarded as malformed. This corresponds to the key words ``MUST``, ``REQUIRED``, and ``SHALL`` in
`RFC2119 <https://www.ietf.org/rfc/rfc2119.txt>`_.

Optional
########

A field is truly optional. This category can be applied to fields that are only useful for a certain type of data. For
instance, the Proband ID and Type field is only required when the pedigree is used to focus on heritable risk for a specific person in the pedigree. For other use cases such as research, a Proband type may be needed.

Brief Explainer of JSON, Protobuf, FHIR...
======

