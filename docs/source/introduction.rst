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

We asked our stakeholders our about their use of family health history and pedigree data - How are you using it? How is it stored? What do you wish you could do with your data that you currently can't? The results of the survey can be `found here <https://docs.google.com/presentation/d/17r-WyVEpl57i0wxnJcgTkmuWclQovMRKkgqJrkWC4Yo/edit?usp=sharing>`. A significant percentage of respondents were using a non-computable or non-interoperable format, and there was no common tool or format with they intede to import or export data. Importantly, 57% of respondents were experiencing challenges with standardization, including lack of computability and integration with analysis tools, and inability to represent complex families and share data easily.

Why PED is Not Enough
======

The `PED format <https://zzz.bwh.harvard.edu/plink/data.shtml>` is a simple text file with 6 columns - IDs, a binary sex field, the phenotype (singular) and SNP genotypes. You can represent a basic parent-child trio, and that may cover a lot of use cases. However, you can't represent twins, things like adoption or donors, pregnancy, vital status, multiple phenotypes and data provenance. All of this type of data is important for genetic counseling and risk assessments where richer representations of relationships are valuable.

The GA4GH Pedigree Standard will natively incorporate PED to enable interoperability with legacy tools.

Example Use Cases
======

Recommended Dataset Document
======

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
