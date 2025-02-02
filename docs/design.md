# Conceptual design of interoperable specifications

The core of interoperable specifications is to find a design that allow parts of information models to be reused across specifications to foster interoperability. The following picture shows the four pillars of interoperable specifications in the form of application profiles, diagrams, data vocabularies and terminologies.

<img src="pics/interoperable_specifications.svg" width="800">

## Machine readability requirements

We will take the perspective that an information model is best described by classes and properties (attributes and relations) as well as ranges of fixed values, from now on referred to as concepts. Hence, we want to reuse the following constituents across specification borders:

* **Classes** - named collections of individuals (instances/objects/things)
* **Properties** - attributes and relations on or between individuals (*)
* **Concepts** - fixed values with clear semantics

To describe how we reuse and contextualize classes, properties and concepts we need an **application profile**. An application profile is a kind of information model that is good at referring to existing classes, properties and concepts via their stable identifiers. Furthermore the application profile restricts usage of classes, properties and concept in a specific context. A special aspect is that it accomplishes such restriction without relying on introducing new classes, properties or concepts via inheritance or similar approaches, see [discussion on subclassing](subclassing.md).

Although an application profile may refer to individual concepts, it is more common to refer to a set of concepts, e.g. expected as values for a certain property. In such cases we will refer to **terminologies** (or code lists), which is also the way concepts are managed together. We may also make use of **concept collections** if it is necessary to refer to a smaller set of concepts from a single or across several terminologies.

(*) Those familiar with UML may notice something different here, attributes and relations are in UML always bound to the class where they are introduced. The only way to let them have a wider applicability is to introduce subclasses. Allowing reuse of properties (attributes and relations) and apply them to classes in new contexts is indeed something different from the traditional object oriented paradigm. However, treating both classes and properties as reusable building blocks that can be repurposed in new settings is the best practise in the linked data domain and widely recognized as a best practice.

## Human readability requirements

An interoperable specifications should include **diagrams** to provide a visual overview of how classes, properties and concepts are combined. Formally, diagrams does not provide any new information as everything they convey are already covered in the application profile. However, diagrams do help human readers to quickly gain an overview as well as provide ways to navigate complex information models.

## Practical considerations

It is important to notice that individual classes and properties are never introduced on their own. Rather, 
classes and properties are introduced in larger sets to cover a specific purpose or domain. We will refer to such a set of classes and properties as a **data vocabulary**. Interoperable specification should indicate which data vocabularies it is based on to improve human readability and simplify discovery of associated information.

It could be argued that the data vocabularies could be deduced automatically if classes and properties have identifiers that allows them to be looked up according to linked data principles. This would also require that classes and properties indicate the data vocabulary they belong to. A similar argument could be made for looking up the concepts from the terminology or concept collections indicated from the application profile. Although automatic detection is possible, providers on interoperable specifications is adviced to not rely on such discovery processes and instead provide explicit dependencies.

To conclude, an interoperable specification should reference not just the application profiles and diagrams but also the data vocabularies and terminologies used to avoid unneccessary complicated lookup processes.

## Foundational specifications

There are organizations that focus on providing generic data vocabularies, e.g. Dublin Core, FOAF, VCard etc. that will be part of many specifications. The same applies to well known terminologies like GEMET and the authority tables of the European Unions publication office. When an interoperable specification reuse these data vocabularies and terminologies they should clarify via a special "**reused" mark**" that they have another origin.

Please note that this does not mean that an interoperable specification is forbidden to introduce its own specific data vocabulary or terminology. (But if it only did this, it is evident that it is not trying to build on top of what other have done already.)

We will refer to specifications that introduce generic data vocabularies or terminologies (i.e. no reuse mark) without providing an application profile as **foundational specifications**. This will allow for organizations that provide these generic data vocabularies or terminologies to announce their work and simplify / encourage discovery and reuse.
