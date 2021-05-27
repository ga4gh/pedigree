# pedigree

The need for high quality, unambiguous, computable pedigree and family information is critical for scaling genomic analysis to larger, complex families. Pedigree data is currently represented in heterogeneous formats that frequently result in the use of lowest-common-denominator formats (e.g., PED) or custom JSON formats for data transfer. The HL7 FHIR standard core data models do not support pedigrees, but there is a draft extension to support genomic pedigrees that should be evaluated and potentially extended by the GA4GH. Standardizing the way systems represent family structure will allow patients to share this information more easily between healthcare systems and help software tools to use this information to improve genome analysis and diagnosis. 

The collection of specific clinical or genetic data is outside the scope of this deliverable, and would instead be handled by other formats and references to individuals within the pedigree representation.
 
- [Use case collection](https://docs.google.com/document/d/1i__95wmm3EpVytRD2gngFAXPhUajK2knWOtuHT9r8W8/edit#)
- [Draft conceptual model](https://github.com/GA4GH-Pedigree-Standard/pedigree/blob/master/model.md)
- [Draft minimal dataset](https://docs.google.com/document/d/1UAtSLBEQ_7ePRLvDPRpoFpiXnl6VQEJXL2eQByEmfGY/edit)
- [Draft FHIR IG](https://github.com/GA4GH-Pedigree-Standard/pedigree-fhir-ig)
- [Meeting Minutes](https://docs.google.com/document/d/12gw2BBIPVaWxUNQx2qiVVIt7W0zVOHON_2Ts9yc9fWY/edit?usp=sharing)
