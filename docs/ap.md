# Application Profile - SHACL-INSPEC

The purpose of an application profile is to clarify in more detail on how to reuse classes, properties and concepts in new settings.
The needs can to a large extent be covered by the SHACL Shapes Constraint Language, a language for validating RDF graphs against a set of conditions.
However, SHACL is too flexibile for the use case of application profiles, hence we define the SHACL-INSPEC to capture the specific requirements / constraints that needs to be met when using SHACL for expressing application profiles. (A side note is that SHACL-INSPEC itself is formally an application profile of SHACL and could therefore be expressed with the help of itself. In time this will perhaps be done, but for now we ignore this as it most likely cause more confusion rather than help to clarify.)

## Glossary
Let's clarify the words we are using in SHACL-INSPEC:

* Entity - a distinct thing/resource/instance/individual described in a dataset
* Property - a specific characteristic of an entity
* Class - a set of entites of the same kind / character / category
* Data graph - data about an entity expressed as RDF
* Node - a reference to a entity in a data graph
* Triple - a fact/statement about an entity (via a node) in a data graph
* Property Shape - constrains the triple expression for a certain property
* Node Shape - joins a set of property shapes into a larger constraint 
* Application Profile - groups a set of node and property shapes together for a certain domain/use case

## Application Profile expression

If you have not already, take a look at the [nine rules of SHACL-INSPEC](rules.md#SHACL-INSPEC).
These rules may be a bit hard to take in, especially since SHACL is a complex language. Hence, below we list what is to be expected based on the application profile as a whole as well as on the shapes.

First of all, for simplicity the requirement on multilinguality are not written explicitly below, i.e. labels, description / definition and usage note are all expected to be expressed with a language and potentially translated into several languages.

The following picture shows the important relations between the different parts of an application profile expression:
<img src="pics/application_profile_model.svg" width="800">

### Application Profile resource

The following information MUST be provided for an application profile resource:

* The application profile resource must have a stable identity in the form of a URI (subject position in triples)
* The application profile resource must be typed as `prof:Profile`
* A label expressed via the property `sh:name`
* A list of main node shapes indicated via the property `dcterms:hasPart`
* A list of supportive node shapes indicated via the property `dcterms:references`
* A list of all property shapes referred to from main or supportive node shapes via the property `dcterms:references`
* A list of all classes and properties used in the application profile must be indicated via the property `dcterms:requires` (the foundational classes from SKOS and RDFS should be exluded)

The following information MAY be provided:

* A description / definition expressed via the property `sh:description`
* A usage note expressed via the property `vann:usageNote`
* Reference to another application profile it is based on via the property `prof:isProfileOf` if shapes have been reused in some way

### Main and supportive node shapes

Note that there are node shapes of more technical nature that are excluded from the requirements below, all those must have a `sh:severity` set to `sh:INFO` or `sh:WARNING`. For example this covers node shapes pointed to via `sh:node` used to express more complex constraints such as indicating which terminology to choose concepts from.

The following information MUST be provided for a node shape:

* A stable identity in the form of a URI (subject position in triples)
* A label expressed via the property `sh:name`
* A list of property shapes via the property `sh:property` (see [section on defining order](#order))

The following information MAY be provided:

* A class it corresponds to via the target declaration `sh:targetClass`
* A description / definition expressed via the property `sh:description`
* A usage note expressed via the property `vann:usageNote`
* A reference to another node shape it
    * "refines" via `sh:and` with a SHACL list containing the refined node shape (see [section on refinement](#refine)), OR
    * "is based on" via the `dcterms:isVersionOf` property (see [section on based on](#basedon))


### Property Shapes pointed to from main and supportive node shapes

Note that there are property shapes of more technical nature that are excluded from the requirements below. Property shapes are either excluded as they are referred to only from excluded node or from logical constraint components.  

The following information MUST be provided for a property shape:

* A stable identity in the form of a URI (subject position in triples)
* A label expressed via the property `sh:name`
* The property it describes how to use via `sh:path`
* The value type to match against, `sh:nodeKind` pointing to `sh:IRI`, `sh:BlankNode`, `sh:Literal` etc.

The following information MAY be provided:

* Express cardinality by:
  * `sh:minCount` "1"^^xsd:integer for mandatory
  * `sh:minCount` "-1"^^xsd:integer for preferred
  * `sh:minCount "0"^^xsd:integer` for preferred (can be left out)
  * `sh:maxCount "N"^^xsd:integer` for a maximum cardinality of `N`
* A description / definition expressed via the property `sh:description`
* A usage note expressed via the property `vann:usageNote`
* That a datatype is required on literals by using `sh:datatype` (if several datatypes are allowed, a construction with several property shapes with individual `sh:datatype` joined togehter via `sh:or` is neccessary)
* That a language is required on literals by setting `sh:datatype` to `rdf:langString`
* Constraints on which literals that is allowed by:
    * An explicit list by using `sh:in` pointing to a SHACL list
    * A constraining pattern expressed by `sh:pattern` (Regular expression)
* Constraints on which URIs that is allowed by:
    * An explicit list by using `sh:in` pointing to a SHACL list
    * A constraining URI pattern expressed by `sh:pattern` (Regular expression)
    * Constrain to concepts in a terminology (see [section below](#terminology))
    * Constrain to concepts in a concept collection (see section below (TODO))
    * Constrain to instances of a class by `sh:class` (if instances from several classes are allowed, a construction with several property shapes with `sh:class` joined togehter via a `sh:or` is neccessary)
* A reference to another property shape it:
  * "refines" via `sh:and` with a SHACL list containing the refined property shape (see [section on refinement](#refine)), OR
  * "is based on" via the `dcterms:isVersionOf` property (see [section on based on](#basedon))

## <a name="refine"></a>How to refine/specialize/inherit shapes

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

## <a name="basedon"></a>Based on - when refinement breaks down

Note that we are not allowed to relax constraints via the refinement construction since it is a conjuction. For instance if we need to relax the constraint and make the publisher optional we cannot specialize the property shape, instead we have to duplicate all information. But we can still provide an indication that we have "based" our property shape on another via the `dcterms:isVersionOf` property like this: 

    ex:ps3 a sh:PropertyShape ;
      sh:label "Publisher" ;
      sh:path dcterms:publisher ;
      sh:nodeKind sh:URI ;
      sh:class foaf:Agent ;
      dcterms:isVersionOf ex:ps1 .

Note that for node shapes it is more common to be be "based" on each other as you often want to change the order or include a slightly different set of property shapes.

## <a name="order"></a>Providing order of property shapes

An important aspect of application profiles is to generate specification documents in a predictable manner. Furthermore, since we aim for multilinguality the order cannot be based on alphabetical sorting of labels. Consequently we outline two rules:

1. Provide an explicit order of all property shapes via `sh:order`.
2. Sort all property shapes that have no sh:order via the alphabetical order of the localname of the property given via sh:path.

Note that if two property shapes have the same property, they must be separated by an explicit `sh:order`.

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

## <a name="terminology"></a> Restricting to concepts in a terminology

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
