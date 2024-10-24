# Conceptual design

We start by identifying which parts of a specification we want to reuse. In short we want to focus on the information model and fixed value lists / terminologies we use in conjunction with it. Hence, we want to reuse the following constituents across specification borders:

* **Classes**
* **Properties** (attributes and relations) (*)
* **Concepts** (fixed values)

To contextualize the classes, properties and concepts in a specification a new specification is needed, the mechanism is referred to as an **application profile**.

Since classes and properties often are introduced together it will make sense to refer to them as a **vocabulary**.

Concepts are often managed together in a **terminology** (or code lists). Sometimes it will be necessary to select a smaller set of concepts from a single or across several thesauri, then we will just refer to them as a **concept collection**.

<img src="docs/pics/semantic_specifications.svg" width="800">

There are organizations that focus on providing generic vocabularies, e.g. Dublin Core, FOAF, VCard etc. that will be part of many specifications. The same applies to well known thesauri like GEMET and the authority tables of the European Unions publication office. To avoid confusion regarding the origin of vocabularies and thesauri they should be included in specifications with a special "**reused" mark**".

Furthermore, there is a need to have semantic specifications that does nothing else than introduce these generic vocabularies or thesauri (without the reuse mark). Hence, we introduce:

**Vocabulary semantic specification** - a specification containing either a vocabulary or thesauri (or both) without the reuse mark. It is not allowed to contain an application profile.

**Profile semantic specification** - a specification containing an application profile.

Every semantic specification is either a vocabulary or a profile semantic specification.

(*) Those familiar with UML may notice something different here, attributes and relations are in UML always bound to the class where they are introduced. The only way to let them have a wider applicability is to introduce subclasses. Allowing reuse of properties (attributes and relations) and apply them to classes in new contexts is indeed something different from the traditional object oriented paradigm. However, treating both classes and properties as reusable building blocks that can be repurposed in new settings is the best practise in the linked data domain and widely recognized as a best practice. (See X for a deeper discussion on this.)