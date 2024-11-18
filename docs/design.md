# Conceptual design of interoperable specifications

The focus on interoperable specifications is on reuse of the information model.
We will take the perspective that an information model is best described by classes and properties (attributes and relations) as well as any fixed value lists / terminologies used in conjunction with properties. Hence, we want to reuse the following constituents across specification borders:

* **Classes**
* **Properties** (attributes and relations) (*)
* **Concepts** (fixed values)

In the context of this specification we now make this important definition:
> 1. An **application profile** is an information model that manages to reuse and contextualize existing classes, properties and concepts.
> 2. An **interoperable specification** is a specification that contains exactly one application profile

Since sets of classes and properties often are introduced together for a specific purpose it will make sense to refer to such a set as a **data vocabulary**.

Concepts are often managed together in a **terminology** (or code lists). Sometimes it will be necessary to select a smaller set of concepts from a single or across several terminologies, then we will just refer to them as a **concept collection**.

There is a strong need for diagrammatic representations that can provide an overview of how classes, properties and concepts are combined. For this purpose we introduce **diagrams** and how they should be expressed to allow interaction with classes, properties, concepts and application profiles.

<img src="pics/interoperable_specifications.svg" width="800">

There are organizations that focus on providing generic data vocabularies, e.g. Dublin Core, FOAF, VCard etc. that will be part of many specifications. The same applies to well known terminologies like GEMET and the authority tables of the European Unions publication office. To avoid confusion regarding the origin of data vocabularies and terminologies they should be included in specifications with a special "**reused" mark**".

Furthermore, there is a need to provide a mechanism to introduce these generic data vocabularies or terminologies (without the reuse mark). Hence, we introduce:

**Vocabulary specification** - a specification containing only a data vocabulary without the reuse mark. It is not allowed to contain an application profile or a terminology.

**Terminology specification** - a specification containing only a terminology without the reuse mark. It is not allowed to contain an application profile or a vocabulary.

Vocabulary and terminology specifications are not stand alone in the sense that a dataset can be fully understood solely based on a claim that it conforms to them. Hence, we do not consider vocabulary and terminology specifications to be interoperable specifications. However, from a harvesting perspective they are crucial for providing a mechanism to avoid duplication of data vocabularies and terminologies.

(*) Those familiar with UML may notice something different here, attributes and relations are in UML always bound to the class where they are introduced. The only way to let them have a wider applicability is to introduce subclasses. Allowing reuse of properties (attributes and relations) and apply them to classes in new contexts is indeed something different from the traditional object oriented paradigm. However, treating both classes and properties as reusable building blocks that can be repurposed in new settings is the best practise in the linked data domain and widely recognized as a best practice. (See X for a deeper discussion on this.)