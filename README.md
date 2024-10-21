# Semantic Specifications Profile

This specification is a profile, that means that mainly propose a special use / pattern of something existing rather than introducing something entirely new. To follow the specification means following the rules set up. Absolut requirements are expressed with MUST / MUST NOT, strong requirments with SHOULD / SHOULD NOT and finally MAY for weaker recommendations, see [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) for a longer treatment.

## Short introduction

At the core of most specification is an information model that describes how data is expressed. An information model is sometimes described informally in a document and sometimes more formally using a modelling language such as UML, ER-diagrams, OWL or RDFS. The information model often consists of:

* **classes** that describes the kind of entities that exists in the data
* **properties** corresponding to attributes on or relations between classes 
* **concepts** that provides fixed values with predefined meeting to be used as values in the data
* **diagrams** that provides a visualization of how the information model

Note that although diagrams are mostly valuable as a help for humans for getting an overview not for specifying formal information, that is provided in the other parts.

Now, lets turn our focus on semantic specifications:

> *The purpose of semantic specification profile is to promote interoperability through reuse of classes, properties and concepts across specifications.* 

A consequence of reuse of classes and properties is that they are defined more loosely to allow them to be combined in different ways. For instance, the property `publisher` can be applied to both a `Text` and a `Dataset`. Hence, we need to introduce a mechanism to combine and restrict their use in new situations:

* **application profiles** provides a mechanism for describing how classes, properties and concepts are combined in new settings

## Profile parts

1. [Background and motivation](docs/background.md) 
2. [Conceptual design and definitions](docs/design.md) 
3. [Rules for semantic specifications](docs/rules.md) ⇐ (**the formal specification**) 
4. Harvesting 
   1. Flavour 1 - natively according to profile 
   2. Flavour 2 - via UML according to the OSLO principles 
   3. Flavour 3 - via tabular annotations (CSV on the web)
5. Bootstrapping specifications used in the different flavours
   1. RDFS 
   2. SHACL 
   3. UML-OSLO 
   4. CSV 
   5. SKOS-SE 
   6. DCAT-AP-SE
6. RDF considerations
   1. Divisions into named graphs
7. Example specification according to the profile
