# Interoperable Specifications Profile

The purpose of the this profile is to promote interoperability through reuse of classes, properties and concepts across specifications. By reuse we mean using the same identifiers and therefore share the formal definitions, not just names. The intention is to allow the information models of specifications to form a larger network of complementary expressions that can act as a basis for interoperability, improved quality, quicker development time and a supportive community with a shared knowledge base. 

This specification is a profile, that means that mainly propose a special use / pattern of something existing rather than introducing something entirely new. To follow the specification means following the rules set up. Absolut requirements are expressed with MUST / MUST NOT, strong requirments with SHOULD / SHOULD NOT and finally MAY for weaker recommendations, see [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) for a longer treatment.

## Short introduction

A specification has historically often been seen as a specification document containing a mixture of background, motivation and more formal descriptions. A more modern way of looking at a specification is that it is a package of resources where some are targeted towards human consumption while others towards processing by machines.   

At the core of most specification is an **information model** that describes how data is expressed. An information model can be described informally in a specification document but can also be expressed more formally using a modelling language such as UML, ER-diagrams, OWL or RDFS. The information model is often expressed in terms of **classes**, **properties** (attributes and relations) as well as **concepts**. In addition, there is nearly always a visualization, typically in the form of a UML class **diagram**.

A consequence of aiming for reuse of classes and properties is that need to defined more loosely to allow them to be combined in different ways. For instance, the property `publisher` can be applied to both a `Text` and a `Dataset`. Consequently, there is a need to describe how they are combined in a specific setting, we refer to this as an **application profile**.

Taken together, the following resources will have special roles in an interoperable specification:

* **data vocabulary** a container for:
    * **classes** that describes the kind of entities that exists in the data
    * **properties** corresponding to attributes on or relations between classes 
* **terminology** a container for:
    * **concepts** provides fixed values with predefined meeting to be used as values in the data
    * **concept collection** a set of collections assembled for a specific purpose
* **application profile** provides a mechanism for describing how classes, properties and concepts are combined in new settings
* **diagrams** provides visualizations of the information model

<img src="docs/pics/semantic_specifications_simple.svg" width="800">

Note 1 that although diagrams are mostly valuable as a help for humans for getting an overview not for specifying formal information (which is provided in the vocabulary, terminology and application profile).

Note 2 a interoperable specification may contain more resources than those mentioned above, although they will not be understood in any deeper way.

## Profile parts

1. [Background and motivation](docs/background.md) 
2. [Conceptual design and definitions](docs/design.md) 
3. [Rules for interoperable specifications](docs/rules.md) ⇐ (**the formal specification**) 
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
