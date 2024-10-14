# Profile for semantic specifications

The rules for semantic specifications are divided into three parts:

1. Rules for specifications
2. Rules for vocabularies
3. Rules for terminologies
4. Rules for application profiles

No rules additional rules are needed for terminologies, relying on SKOS is enough.

## Rules for specifications

For a specification to be considered a semantic specification the following must apply:

><a name="SP1"></a> **Rule SP-1:** The specification and its specification parts MUST be described with PROF

><a name="SP2"></a> **Rule SP-2:** A specification part MAY be a vocabulary, a terminology, an application profile or other relevant resources.

><a name="SP3"></a> **Rule SP-3:** Each vocabulary MUST be possible to interpret as RDFS-SP.

><a name="SP4"></a> **Rule SP-4:** The specification MUST indicate for each vocabulary if it is introduced or reused

><a name="SP5"></a> **Rule SP-5:** Each terminology MUST be possible to interpret as SKOS-SP.

><a name="SP6"></a> **Rule SP-6:** The application profile MUST be possible to interpret as SHACL-SP

><a name="SP7"></a> **Rule SP-7:** The specification MUST contain either an introduced vocabulary, a single application profile or both

## Rules for vocabularies - RDFS-SP

RDFS is a vocabulary for defining vocabularies in an open world. RDFS-SP on the other defines a vocabulary as a single RDF Dataset in a closed world perspective. For simplicity we will think of this RDF Dataset as a file with triples, potentially divided into multiple named graphs.

For a vocabulary to be considered to follow RDFS-SP the following must apply:

><a name="VO1"></a> **Rule VO-1:** A vocabulary is expressed in a single RDF Dataset.

><a name="VO2"></a> **Rule VO-2:** There is a single resource typed as sp:Vocabulary in the RDF Dataset

><a name="VO3"></a> **Rule VO-3:** All included classes and properties must point to the vocabulary resource via the rdfs:isDefinedBy property

><a name="VO4"></a> **Rule VO-4:** Classes and properties from other vocabularies MAY BE refined via rdfs:subClassOf and rdfs:subProperty in the vocabulary

><a name="VO5"></a> **Rule VO-5:** Classes and properties from other vocabularies MAY BE included in the RDF Dataset but MUST NOT be indicated to be part of the current vocabulary via the rdfs:isDefinedBy property


## Rules for application profiles - SKOS-SP

SKOS is a vocabulary for defining terminologies in an open world. SKOS-SP on the other defines a terminology as a single RDF Dataset in a closed world perspective. For simplicity we will think of this RDF Dataset as a file with triples, potentially divided into multiple named graphs.

><a name="TE1"></a> **Rule TE-1:** A terminology is expressed in a single RDF Dataset.

><a name="TE2"></a> **Rule TE-2:** There is a terminology resource typed as skos:ConceptScheme in the RDF Dataset

><a name="TE3"></a> **Rule TE-3:** All included concepts and collections must point to the terminology resource via the skos:inScheme property

><a name="TE4"></a> **Rule TE-4:** Concepts referred to via skos:related must be concepts in the same vocabulary

><a name="TE5"></a> **Rule TE-5:** Concepts from other terminologies referred to via skos:closeMatch, skos:exactMatch, skos:relatedMatch, skos:narrowMatch or skos:broaderMatch MUST NOT be concepts in the current terminology.

## Rules for application profiles

><a name="AP1"></a> **Rule AP-1:** 

