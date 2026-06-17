################################################
Kinship Ontology (KIN)
################################################

.. toctree::
   :maxdepth: 1


The Kinship Ontology
========================

`The Kinship Ontology (KIN) <https://github.com/GA4GH-Pedigree-Standard/family_history_terminology>`_ is a family relations ontology developed as part of the GA4GH Pedigree Standard project. It provides a structured, OWL-based vocabulary for recording the biological and social/legal relationships between individuals in a pedigree. KIN enables the use of an OWL reasoner to automatically validate a family history graph (e.g., detect cycles in the parent-child tree) and infer new relations (e.g., derive grandparent from two parent assertions).

The latest version of the ontology can be found at: http://purl.org/ga4gh/kin.owl.

The ontology is open-source and managed at: https://github.com/GA4GH-Pedigree-Standard/family_history_terminology

.. note::

   We are working with colleagues to explore migrating KIN to the `Relations Ontology (RO) <https://github.com/oborel/obo-relations>`_.


KIN Subsets and Pedigree Fields
=================================

KIN terms are divided into two top-level subsets that map directly onto the two relationship fields in the Pedigree model:

.. list-table::
   :header-rows: 1
   :widths: 30 35 35

   * - KIN subset root
     - Pedigree field
     - Examples
   * - ``KIN:002 isBiologicalRelativeOf``
     - ``biological_relationship``
     - ``isBiologicalParentOf``, ``isBiologicalMotherOf``, ``isGestationalCarrierOf``, ``isSpermDonorOf``, ``isBiologicalSiblingOf``, ``isBiologicalGrandparentOf``
   * - ``KIN:019 isSocialLegalRelativeOf``
     - ``social_relationship``
     - ``isAdoptiveParentOf``, ``isFosterParentOf``, ``isStepParentOf``, ``isSocialLegalParentalSiblingOf``, ``isPartnerOf``

A ``Relationship`` may have values in both fields simultaneously — for example, a gestational carrier (``biological_relationship: isGestationalCarrierOf``) who later formally adopts the child (``social_relationship: isAdoptiveParentOf``).

For the full list of KIN terms and their definitions, see the `KIN ontology documentation <https://github.com/GA4GH-Pedigree-Standard/family_history_terminology>`_.


Preferred Term Direction
=========================

KIN terms are **directional**: they describe a relationship from the subject (``individual``) to the object (``relative``). KIN includes both a term and its logical inverse for many relationships (e.g., ``isBiologicalParentOf`` and ``isBiologicalChildOf``). Both directions are semantically valid, but **implementors should prefer the downward direction** (ancestor → descendant) for consistency with the pedigree's :doc:`reduced form <using-the-pedigree-model>` and to avoid redundant assertions.

.. list-table::
   :header-rows: 1
   :widths: 40 40 20

   * - Preferred (downward)
     - Inverse (avoid unless needed)
     - Notes
   * - ``isBiologicalParentOf``
     - ``isBiologicalChildOf``
     -
   * - ``isBiologicalMotherOf``
     - *(no preferred inverse)*
     -
   * - ``isBiologicalFatherOf``
     - *(no preferred inverse)*
     -
   * - ``isBiologicalGrandparentOf``
     - ``isBiologicalGrandchildOf``
     -
   * - ``isBiologicalGreatGrandparentOf``
     - ``isBiologicalGreatGrandchildOf``
     -
   * - ``isBiologicalParentalSiblingOf``
     - ``isBiologicalNiblingOf``
     -
   * - ``isAdoptiveParentOf``
     - *(no dedicated inverse)*
     - Inverse via KIN:019 hierarchy
   * - ``isSocialLegalGrandparentOf``
     - ``isSocialLegalGrandchildOf``
     -

**Why inverses exist:** Inverse terms are included in KIN to support cases where data has been collected from the descendant's perspective (e.g., a patient report of "my grandmother is..."). An OWL reasoner that loads the pedigree will automatically infer the inverse direction from a single assertion, so there is no need to record both. Recording both directions for the same pair is redundant and should be avoided.

Sibling and partner relationships are **symmetric** (e.g., ``isBiologicalSiblingOf``, ``isPartnerOf``) and have no preferred direction — record them once in either direction.


Example Usage
==============

The following example records a relationship where Individual B is the biological child of Individual A (egg + gestation), expressed in the preferred downward direction:

.. code-block:: yaml

   relationships:
     - individual: A
       relative: B
       biological_relationship:
         - id: KIN:027
           label: isBiologicalMotherOf

An adoptive relationship alongside a gestational one (two separate relationship records, or combined biological and social on the same record):

.. code-block:: yaml

   relationships:
     - individual: A
       relative: B
       biological_relationship:
         - id: KIN:005
           label: isGestationalCarrierOf
       social_relationship:
         - id: KIN:022
           label: isAdoptiveParentOf

A sibling relationship (symmetric — direction does not matter):

.. code-block:: yaml

   relationships:
     - individual: B
       relative: C
       biological_relationship:
         - id: KIN:008
           label: isFullsiblingOf
