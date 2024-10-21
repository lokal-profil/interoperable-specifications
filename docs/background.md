# Background and motivation

We will start by looking into what we mean with specifications and then move over to semantic specifications.

## Specifications

With a specification we mean an expression that specifies how data in a dataset should be expressed and understood.

Historically specifications have often been considered to be documents written for human consumption. But in a modern setting this is not enough, a specification has to be capable to encompass a set of distinct resources. The traditional specification document is itself perhaps the most important of these resources, but in addition there are resources corresponding to a schema, a guideline, one or several terminologies, an information model. Some of these resources are directed towards humans, while others are useful for machines, e.g. to validate that data is expressed according to the intentions of the specification.

With these requirements we have defined a specification to an abstract thing that consists of one or more resources. Both the specification itself and its resources are given identifiers in the form of URIs that can be used to reference it, e.g. to express that a dataset is conformant to it. Note that with this definition a specification is not a document anymore, it is not even a digital entity. Rather a specification is something abstract that has to be understood by its descriptive metadata and in more detail by looking at the resources it contains (and the metadata that describes them).

To learn more about an individual specification and its resources we need metadata that describes them, this is accomplished by relying on RDF, more specifically the PROF vocabulary.

It is important to recognize that specification authors have different ambitions when it comes to expressing specifications. Some specifications will be nothing more than documents, perhaps accessible as PDFs or as webpages. Others want to take a step further and provide a large set of resources, including some form of detailed information model.

Data expressed according to specifications is the foundation for interoperability as it describes how data is to be expressed and understood.

## Semantic specifications

We live in an increasingly digital world where data is supposed to be exchanged and understood in new contexts all the time. Hence, the need for well written specifications is increasing. It is also the case that many specifications have lage similarities, for instance, there is often a need to express information around persons, organizations, places, events etc.

The purpose of the semantic specifications profile is to provide a mechanism for reusing parts of specifications when creating new specifications.

There are many potential added values:
* Quicker development of new specifications as you do not have to start from scratch.
* Improved quality since reused parts have been battle tested.
Shorter learning time if new specifications have fewer parts that are truly unique.
* Data from different specifications can be partially interoperable.

To reach this goal we need to allow individual parts of a specification to be defined and reused in new contexts, this means being careful about defining the semantics of these parts independent of the context they first are introduced in.
