# Conceptual design of interoperable specifications

The core of interoperable specifications is to find a design that allow parts of information models to be reused across specifications to foster interoperability. The following picture shows the four pillars of interoperable specifications in the form of application profiles, diagrams, data vocabularies and terminologies.

<img src="pics/interoperable_specifications.svg" width="800">

## Machine readability requirements

We will take the perspective that an information model is best described by classes and properties (attributes and relations) as well as ranges of fixed values, from now on referred to as concepts. Hence, we want to reuse the following constituents across specification borders:

* **Classes** - named collections of individuals (instances/objects/things)
* **Properties** - attributes and relations on or between individuals (*)
* **Concepts** - fixed values with clear semantics

To describe how we reuse and contextualize classes, properties and concepts we need an **application profile**. An application profile is a kind of information model that is good at referring to existing classes, properties and concepts via their stable identifiers. Furthermore the application profile restricts usage of classes, properties and concepts in a specific context. A special aspect is that it accomplishes such restriction without relying on introducing new classes, properties or concepts via inheritance or similar approaches, see [discussion on subclassing](subclassing.md).

Although an application profile may refer to individual concepts, it is more common to refer to a set of concepts, e.g. as expected values for a certain property. In such cases we will refer to **terminologies** (or code lists), which is also the way concepts are managed together. We may also make use of **concept collections** if it is necessary to refer to a smaller set of concepts from a single or across several terminologies.

(*) Those familiar with UML may notice something different here, attributes and relations are in UML always bound to the class where they are introduced. The only way to let them have a wider applicability is to introduce subclasses. Allowing reuse of properties (attributes and relations) and apply them to classes in new contexts is indeed something different from the traditional object oriented paradigm. However, treating both classes and properties as reusable building blocks that can be repurposed in new settings is the best practise in the linked data domain and widely recognized as a best practice.

## Human readability requirements

An interoperable specifications should include **diagrams** to provide a visual overview of how classes, properties and concepts are combined. Formally, diagrams do not provide any new information as everything they convey is already covered in the application profile. However, diagrams do help human readers to quickly gain an overview as well as provide ways to navigate complex information models.

## Practical requirements

From the requirements above it is clear that an interoperable specification need to be considered a container with multiple parts, see also discussion in the [background and motivation document](background.md). In practise this means that an interoperable specification needs an expression where it is possible to describe the specification and the parts with metadata separately as well as connect them. It also means that there needs to be an explicit mechanism to retrieve the parts. From the requirements above we recognize terminologies, diagrams and application profiles to be clear specification parts that should be described individually and retrievable. 

However, when it comes to classes and properties we prefer to not include as parts individually into the specification for two reasons. First they would be quite many and would lead to a loss of overview of the specification. Second, this would lead to a mismatch with how they are managed. Classes and properties are generally introduced and managed in larger sets to cover a specific purpose or domain. Consequently, we will refer to such a set of classes and properties as a **data vocabulary** and include it as a part.

A sidenote is that data vocabularies could be deduced automatically from the application profile if classes and properties have identifiers that allows them to be looked up according to linked data principles. A prerequisite would be that classes and properties indicate the data vocabulary they belong to. A similar argument could be made for looking up the terminology or concept collections indicated from the application profile. Although automatic detection is possible, providers on interoperable specifications is adviced to not rely on such discovery processes and instead provide explicit dependencies.

To conclude, an interoperable specification should reference all parts explicitly to avoid unnecessary complicated lookup processes.

## Foundational specifications

There are organizations that focus on providing generic data vocabularies, e.g. Dublin Core, FOAF, VCard etc. that will be part of many specifications. The same applies to well known terminologies like GEMET and the authority tables of the European Unions publication office. When an interoperable specification reuse these data vocabularies and terminologies they should clarify via a special "**reused" mark**" that they have another origin.

Please note that this does not mean that an interoperable specification is forbidden to introduce its own specific data vocabulary or terminology. (But if it only did this, it is evident that it is not trying to build on top of what other have done already.)

We will refer to specifications that introduce generic data vocabularies or terminologies (i.e. without reuse mark) without providing an application profile as **foundational specifications**. This will allow for organizations that provide these generic data vocabularies or terminologies to announce their work and simplify / encourage discovery and reuse.
