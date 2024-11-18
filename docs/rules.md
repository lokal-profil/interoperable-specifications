# Rules for interoperable specifications

The rules for interoperable specifications are divided into five parts:

1. Rules for specifications
2. Rules for vocabularies - RDFS-SP
3. Rules for terminologies - SKOS-SP
4. Rules for profiles - SHACL-SP
5. Rules for diagrams - SVG-SP

## Rules for specifications

For a specification to be considered a interoperable specification the following must apply:

><a name="SP1"></a> **Rule SP-1:** The specification and its specification parts MUST be described with PROF

><a name="SP2"></a> **Rule SP-2:** A specification part MAY be a vocabulary, a terminology, an application profile, a diagram or other relevant resources.

><a name="SP3"></a> **Rule SP-3:** Each vocabulary MUST be possible to interpret as RDFS-SP.

><a name="SP4"></a> **Rule SP-4:** The specification MUST indicate for each vocabulary if it is introduced or reused

><a name="SP5"></a> **Rule SP-5:** Each terminology MUST be possible to interpret as SKOS-SP.

><a name="SP6"></a> **Rule SP-6:** The profile MUST be possible to interpret as SHACL-SP

><a name="SP7"></a> **Rule SP-7:** The specification MUST contain either an introduced vocabulary, a single application profile or both

## Rules for vocabularies - RDFS-SP

RDFS-SP builds on top of the RDF Schema specification by providing the following additional restrictions:

><a name="VO1"></a> **Rule VO-1:** A vocabulary MUST be expressed in a single RDF Dataset. (*)

><a name="VO2"></a> **Rule VO-2:** There MUST be a single vocabulary resource typed as sp:Vocabulary in the RDF Dataset

><a name="VO3"></a> **Rule VO-3:** All included classes and properties of the vocabulary MUST point to the vocabulary resource via the rdfs:isDefinedBy property

><a name="VO4"></a> **Rule VO-4:** All included classes and properties as well as the vocabulary resource MUST have URIs

><a name="VO5"></a> **Rule VO-5:** Classes and properties from other vocabularies MAY BE included in the RDF Dataset, e.g. when being pointed to via rdfs:subClassOf and rdfs:subProperty, but MUST NOT point to the vocabulary resource via the rdfs:isDefinedBy property

## Rules for terminologies - SKOS-SP

SKOS-SP builds on top of the SKOS specification by providing the following additional restrictions:

><a name="TE1"></a> **Rule TE-1:** A terminology MUST be expressed in a single RDF Dataset. (*)

><a name="TE2"></a> **Rule TE-2:** There MUST be a single terminology resource typed as skos:ConceptScheme in the RDF Dataset

><a name="TE3"></a> **Rule TE-3:** All concepts and collections in the terminology MUST point to the terminology resource via the skos:inScheme property

><a name="TE4"></a> **Rule TE-4:** All concepts, collections as well as the terminology resource MUST have URIs

## Rules for profiles

SHACL-SP builds on top of the SHACL specification by providing the following additional restrictions.

THE FOLLOWING TEXT / RULES ARE VERY UNSTABLE

To simplify the rules below we introduce the following terminology:

1. Main node shape - a node shape with no sh:node pointing to it 
2. Supportive node shape - a node shape with at least one sh:node pointing to it 
3. Ordering property shape - a property shape with an sh:order pointed to directly from a node shape via sh:property
4. Main property shape - a property shape pointed to from an ordering property shape 
5. Extended property shape - a property shape extended by a main property shape

><a name="AP1"></a> **Rule AP-1:** A profile MUST be expressed in a single RDF Dataset (**)

><a name="AP2"></a> **Rule AP-2:** There MUST be a single profile resource typed as sp:Profile

><a name="AP3"></a> **Rule AP-3:** All shapes MUST have URIs and point to the profile resource via the rdfs:isDefinedBy except ordering property shapes introduced for ordering purposes only

><a name="AP3"></a> **Rule AP-3:** All ordering property profiles MUST be represented as blank nodes and MUST NOT point to the profile resource via the rdfs:isDefinedBy

><a name="AP5"></a> **Rule AP-5:** Shapes MAY inherit characteristics from other shapes via the sh:and construct, those shapes may reside in other RDF Datasets that are indicated via owl:import.   

><a name="AP5"></a> **Rule AP-5:** Entity profiles and main property profiles MAY indicate that they are based on other shapes via the sp:basedOn property, those shapes may reside in other RDF Datasets indicated via owl:import.


(*) Both RDFS and SKOS introduce building blocks (classes and properties) for for defining things (other classes, properties, concepts, collections) in an open world manner. However, both vocabularies and terminologies needs to work in the context of semantic specifications where it is stated explicitly what is included, hence it corresponds to a closed world perspective. Hence, in both RDFS-SP and SKOS-SP there is a restriction to assume that everything needed is provided in the indicated RDF Datasets.

(**) SHACL supports imports declared via owl:import, the rules are written from the perspective that these are respected and all RDF Datasets (potentially recursively) are imported first into a single RDF Dataset.