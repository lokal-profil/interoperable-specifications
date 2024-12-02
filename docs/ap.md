# Application Profile - SHACL-INSPEC

The purpose of an application profile is to clarify in more detail on how to reuse classes, properties and concepts in new settings. We will to a large extent rely on SHACL to express application profiles and the explicit reuse via node shapes and property shapes. 

<img src="pics/application_profile_model.svg" width="800">

Let's repeat the central terminology we are using:

* Entity - a distinct thing/resource/instance/individual described in a dataset
* Property - a specific characteristic of an entity
* Class - a set of entites of the same kind / character / category
* Data graph - data about an entity expressed as RDF
* Node - a reference to a entity in a data graph
* Triple - a fact/statement about an entity (via a node) in a data graph
* Property Shape - constrains the triple expression for a certain property
* Node Shape - joins a set of property shapes into a larger constraint 
* Application Profile - groups a set of node and property shapes together for a certain domain/use case

## Application Profile contents

For simplicity the requirement on multilinguality are not written explicitly below, i.e. labels, description / definition and usage note are all expected to be expressed with a language and potentially translated in several languages.

### Application Profile

The following information MUST be provided for an application profile:

* A stable identity in the form of a URI (subject position in triples)
* A label

The following information MAY be provided:

* A description / definition
* A usage note
* Reference to another application profile it is based on

### Node shape

The following information MUST be provided for an node shape:

* A stable identity in the form of a URI (subject position in triples)
* Reference to the application profile it belongs to
* The class it corresponds to
* A label
* A list of ordered Property shapes
* A mark if it is a main or supportive node shape

The following information MAY be provided:

* A description / definition
* A usage note
* A reference to another node shape it is based on

### Property Shape

The following information MUST be provided for a property shape:

* A stable identity in the form of a URI (subject position in triples)
* A reference to the application profile it belongs to
* The property it describes how to use
* A label
* Express mandatory, preferred or optional as well as the max cardinality
* The value type to match against (URI, literal or blank node)

The following information MAY be provided:

* A description / definition
* A usage note
* That a datatype is required on literals (there might be one or several allowed datatypes)
* That a language is required on literals
* Constraints on which literals that is allowed by:
    * An explicit list
    * A constraining pattern (Regular expression)
* Constraints on which URIs that is allowed by:
    * An explicit list
    * A constraining URI pattern (Regular expression)
    * Constrain to concepts in an terminology
    * Constrain to concepts in a concept collection
    * Constrain to instances of a class
* A reference to a another property shape:
    * that it refines OR
    * that it is based on

## How to specialize shapes

SHACL allows shapes to be combined via `sh:and`. This can be used to specialize an existing shapes with additional constraints or further restricting. E.g. consider the following property shape for the property `dcterms:publisher` where the range is `foaf:Agent`.

    ex:ps1 a sh:PropertyShape ;
      sh:label "Publisher" ;
      sh:path dcterms:publisher ;
      sh:nodeKind sh:URI ;
      sh:minCount "1" ;
      sh:class foaf:Agent .

we can further constrain it to the subclass `foaf:Organization` via the following construction:

    ex:ps2 a sh:PropertyShape
      sh:path dcterms:publisher ;
      sh:class foaf:Organization ;
      sh:and ( ex:ps1 ) .

Note that at a minimum we have to duplicate the `sh:path` property.

Please note that we are not allowed to relax constraints via this construction. For instance if we need to relax the constraint and make the publisher optional we cannot specialize the property shape, instead we have to duplicate all information. But we can still provide an indication that we have "based" our property shape on another via the `dcterms:isVersionOf` property like this: 

    ex:ps3 a sh:PropertyShape ;
      sh:label "Publisher" ;
      sh:path dcterms:publisher ;
      sh:nodeKind sh:URI ;
      sh:class foaf:Agent ;
      dcterms:isVersionOf ex:ps1 .

Note that for node shapes it is more common to be be "based" on each other as you often want to change the order or include a slightly different set of property shapes.

## Providing order of property shapes

An important aspect of application profiles is to generate specification documents in a predictable manner. Furthermore, since we aim for multilinguality the order cannot be based on alphabetical sorting of labels. Consequently we outline two rules:

1. Provide an explicit order of all property shapes via `sh:order`.
2. Sort all property shapes that have no sh:order via the alphabetical order of the localname of the property given via sh:path.

Note that if two property shapes have the same property, they need to be separated by an explicit `sh:order`.

> It is a strong recommendation to always provide an explicit order on every property shape.

If you are reusing property shapes in new settings (e.g. in specialization of node shapes) you may want to change the order. Lets consider the example where we have a profile for a book with a title and an identifier:

    ex:ns2 a sh:NodeShape ;
      sh:label "Book"@en ;
      sh:property ex:ps-title, ex:ps-identifier .
    ex:ps-title a sh:PropertyShape ;
      sh:path dcterms:title ;
      sh:nodeKind sh:Literal ;
      sh:label "Title"@en ;
      sh:order "1"^^xsd:decimal ;
    ex:ps-identifier a sh:PropertyShape ;
      sh:path dcterms:identifier ;
      sh:nodeKind sh:Literal ;
      sh:label "Identifier"@en ;
      sh:order "2"^^xsd:decimal ;

To change the order so the `dcterms:identifier` is at the top you can make a minimal specialization of that property shape with another `sh:order`:

    ex:ns2 a sh:NodeShape ;
      sh:label "Book 2" ;
      sh:property [
          sh:path dcterms:identifier ;
          sh:order "0.5"^^xsd:decimal ;
          sh:and ( ex:ps-identifier ) .
      ], ex:ps-title.

Note that you have to repeat the `sh:path` due to SHACL rules. We have chosen to not give the new property shape a URI since it does not provide any additional value beyond the order, which is specific to the node shape. This is possible since it does not fall under the AP-4 rule since `sh:and` is excluded, `sh:path` is not a constraint and an `sh:order` is a characteristic (i.e. a non-validating property).

## Restricting to concepts in a terminology

To restrict to concepts in a terminology you should specify:
1. That you are expecting instances of the class `skos:Concept`.
2. That you are expecting the concepts to have a `skos:inScheme` property pointing to the terminology.
3. A regular expression for the concept URIs (optional).

```
ex:ps1 a sh:propetyShape ;
    sh:path dcterms:subject ;
    sh:pattern "^http://example.com/terminologyA/.*$" ;
    sh:node [
        a sh:NodeShape ;
        sh:severity sh:Info ;
        sh:property [
          sh:path rdf:type ;
          sh:hasValue skos:Concept
        ], [
          sh:path skos:inScheme ;
          sh:hasValue ex:terminologyA
        ]
    ]
```

The reason we se the `sh:severity` to `sh:Info` is that if we try validate a data graph against this SHACL expression we do not always expect to have the entire terminology loaded with `rdf:type` and `skos:inScheme` triples for all concepts. In this case we instead rely on the `sh:pattern` to give us a more syntactical indication that the URI in the data graph corresponds to a correct concept. The expression with `rdf:type` and `skos:inScheme` (1 & 2 above) is provided to allow us to both detect that this is in fact a terminology when we render the specification and a more correct way to search for the intended concepts.

TODO Concept Collections

## Anti patterns

### Multiple top-level property shapes for the same triple
SHACL allows a node shape to include multiple property shapes that together constrain a single triple. For instance, one property shape may constrain the node type and another the cardinality. This is problematic and should be avoided as it both makes it hard to generate documentation and complicates reuse. E.g. the following example is not encouraged:

    ex:ns1 a sh:NodeShape ;
        sh:label "Person" ;
        sh:property ex:ps1, ex:ps2 .
    ex:ps1 a sh:PropertyShape ;
        sh:label "Name" ;
        sh:path foaf:givenName ;
        sh:nodeKind sh:Literal .
    ex:ps2 a sh:PropertyShape ;
        sh:path foaf:givenName ;
        sh:minCount "1" .

Instead the expression should be done via a single property shape:

    ex:ns1 a sh:NodeShape ;
        sh:label "Person" ;
        sh:property ex:ps1 .
    ex:ps1 a sh:PropertyShape ;
        sh:label "Name" ;
        sh:path foaf:givenName ;
        sh:minCount "1" .
        sh:nodeKind sh:Literal .

However, there are situations where the same property is reused on the same node for different reasons. But then the constraints should be made in such a way that the triples matched for each property shape are disjoint. Mechanism to ensure that they match different sets of triples includes `sh:nodeKind` and `sh:pattern`. The following example shows how to point to two different sets of concepts using the same property (`foaf:topic_interests`):

    ex:ns1 a sh:NodeShape ;
        sh:label "Person" ;
        sh:property ex:ps1, ex:ps2 .
    ex:ps1 a sh:PropertyShape ;
        sh:label "Hobbies" ;
        sh:path foaf:topic_interests ;
        sh:nodeKind sh:URI ;
        sh:pattern "^http://example.com/hobbies/.*$";
    ex:ps2 a sh:PropertyShape ;
        sh:label "Proffessional interests in computer science" ;
        sh:path foaf:topic_interests ;
        sh:nodeKind sh:URI ;
        sh:pattern "^http://example.com/computer_science/.*$";
