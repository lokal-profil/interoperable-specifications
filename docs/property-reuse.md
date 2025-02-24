# Using the same property for different purposes

This section discusses under which conditions it is possible to reuse the same property for different purposes on a single entity as captured by a node shape. Below we list the scenarios where INSPEC says it is acceptable to do so:

1. When it has sufficient different node type, i.e. literal, literal with datatype, URI or blank node.
2. Literals with different datatypes.
3. URIs if they can be separated based on different patterns.
4. URIs if they are so few it makes sense to enumerate them (typically less than 50).
5. Blank nodes if there are direct outgoing properties that are different, e.g. a `rdf:type`. 

The following section provides a longer background, motivation, example and technicalities for how we distinguish the above five situations in SHACL.

## Background

With RDF Schema properties can be introduced with different specificity leaving us with different amount of flexibility when reusing the property. In general, the more flexibility a property is given when defined, the more we need to constrain it to clarify how it is intended to be used in a certain setting (as captured by an application profile).

Properties that are the most flexible have a range of `rdf:Resource` which allows values ranging from literals, literals with language, literals with a datatype or instances of classes. Furthermore, instances of classes can be be either blank nodes or URIs.

## Why do we want to reuse the same property

The alternative to reuse is to introduce a new property, perhaps as a subproperty of a known property. There are mainly two reasons for why we may avoid introducing new properties and instead rely on reusing a single property for different purposes:

1. It complicates reuse as the semantics of new properties needs an additional lookup, see section on [subclassing](subclassing.md).
2. Defining new application profiles by reusing existing properties has a smaller footprint than having to establish new properties in a new namespace that have to be maintained.

## Example of a flexible property - `dcterms:subject`

A well known flexible property is `dcterms:subject` which has the range is rdfs:Resource. It is described that it should preferably have values from controlled vocabularies as URIs, but if that is not feasible literals are allowed (with language specified or not is left open).

This means that in principle we can use `dcterms:subject` on a single entity to do all the following below without conflicting the values:

* provide keywords as literals
* provide keywords as literals with language
* point to URIs from the controlled vocabulary A
* point to URIs from the controlled vocabulary B

If we ignore the definitions provided in natural language by the Dublin Core Metadata Initiative group and focus strictly what is expressed in the RDF Schema we could also (although it is not recommended):

* point to a blank node that corresponds to a value that is defined uniquely via further triples.
* provide a literal with a datatype

## When is it possible to reuse a property for different purposes on the same entity?

In INSPEC we are using SHACL property shapes to distinguish between different purposes. Hence, as long it is possible to match a focus node to a specific property shape we can use the same property for different purposes.

The constraint components `sh:nodeKind`, `sh:datatype`, `sh:in` and `sh:pattern` can all be checked on a single triple and are unproblematic to use. If we are allowed to check further triples we can also use `sh:node` and `sh:class`. Note that it may be necessary to follow `sh:and` and other logical constraint components to get to the relevant constraint components.

## When should we avoid to reuse the same property for different purposes on the same entity?

Just because it is possible reuse the same property for different purposes does not mean it is a good idea.

The following situations should be avoided:

* Presence of language or different languages.
* Distinguish between URIs solely based on if they belong to different classes / concept in terminologies, unless:
    * The URIs are few enough to be explicitly listed via `sh:in`
    * All matching URIs can be determined uniquely via `sh:pattern`

The reason for not recommending looking at further triples for URIs is that we typically do not have them in the same graph. For instance, when pointing to a concept in a terminology we do not usually copy over further triples like it's type, prefLabel etc. If such triples are copied over there is a risk of having stale information. In addition, in a harvesting scenario we would have to keep track of the original source of each entity to make sure we do not discard the wrong information or propagate stale information to others.