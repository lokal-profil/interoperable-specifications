# Rules for interoperable specifications

The rules for interoperable specifications are divided into five parts:

1. Rules for interoperable specifications - PROF-INSPEC
2. Rules for data vocabularies - RDFS-INSPEC
3. Rules for terminologies - SKOS-INSPEC
4. Rules for application profiles - SHACL-INSPEC
5. Rules for diagrams - SVG-INSPEC

## Rules for interoperable specifications - PROF-INSPEC

For a specification to be considered a interoperable specification the following must apply:

><a name="INSPEC1"></a> **Rule INSPEC-1:** The "interoperable specification resource" and its parts MUST have URIs and be described with PROF, the interoperable specification resource must be typed as `prof:Profile` and have a `dct:conformsTo` point to `inspec:PROF`

><a name="INSPEC2"></a> **Rule INSPEC-2:** An interoperable specification part MAY be a data vocabulary, a terminology, an application profile or a diagram. Other parts may exist but have no prescribed meaning by the interoperable specification profile.

><a name="INSPEC3"></a> **Rule INSPEC-3:** Each data vocabulary MUST be detected by `dcterms:conformsTo` pointing to `inspec:RDFS` and MUST be possible to interpret as RDFS-INSPEC.

><a name="INSPEC4"></a> **Rule INSPEC-4:** Each terminology MUST be detected by `dcterms:conformsTo` pointing to `inspec:SKOS` and MUST be possible to interpret as SKOS-INSPEC.

><a name="INSPEC5"></a> **Rule INSPEC-5:** The interoperable specification MUST list all data vocabularies and terminologies it reuses explicitly and where they are introduced via the `prof:isInheritedFrom`

><a name="INSPEC6"></a> **Rule INSPEC-6:** Each application profile MUST be detected by `dct:conformsTo` pointing to `inspec:SHACL` and MUST be possible to interpret as SHACL-INSPEC

><a name="INSPEC7"></a> **Rule INSPEC-7:** A diagram MUST be detected by `dct:conformsTo` property pointing to `inspec:SVG` and MUST follow SVG-INSPEC 

><a name="INSPEC8"></a> **Rule INSPEC-8:** An interoperable specification MUST contain at least one data vocabulary and at least one application profile

## Rules for data vocabularies - RDFS-INSPEC

RDFS-INSPEC builds on top of the RDF Schema specification by providing the following additional restrictions:

><a name="DV1"></a> **Rule DV-1:** A data vocabulary MUST be expressed in a single RDF Dataset. (*)

><a name="DV2"></a> **Rule DV-2:** There MUST be a single "data vocabulary resource" typed as `owl:Ontology` in the RDF Dataset

><a name="DV3"></a> **Rule DV-3:** All included classes and properties of the data vocabulary MUST point to the "data vocabulary resource" via the `rdfs:isDefinedBy` property

><a name="DV4"></a> **Rule DV-4:** All included classes and properties as well as the data vocabulary resource MUST have URIs

><a name="DV5"></a> **Rule DV-5:** Classes and properties from other vocabularies MAY BE included in the RDF Dataset, e.g. when being pointed to via `rdfs:subClassOf` and `rdfs:subProperty`, but MUST NOT point to the same "data vocabulary resource" via the `rdfs:isDefinedBy` property

><a name="DV6"></a> **Rule DV-6:** The "data vocabulary resource" must be indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)


## Rules for terminologies - SKOS-INSPEC

SKOS-INSPEC builds on top of the SKOS specification by providing the following additional restrictions:

><a name="TE1"></a> **Rule TE-1:** A terminology MUST be expressed in a single RDF Dataset. (*)

><a name="TE2"></a> **Rule TE-2:** There MUST be a single "terminology resource" with a URI typed as `skos:ConceptScheme` in the RDF Dataset

><a name="TE3"></a> **Rule TE-3:** All concepts and collections in the terminology MUST point to the "terminology resource" via the `skos:inScheme` property

><a name="TE4"></a> **Rule TE-4:** All concepts, collections as well as the terminology resource MUST have URIs

><a name="TE5"></a> **Rule TE-5:** The "terminology resource" must be indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)

## Rules for application profiles - SHACL-INSPEC

SHACL-INSPEC builds on top of the SHACL specification by providing additional restrictions. Since SHACL is a rich language the following rules does not cover all situations. For instance, the following rules does not indicate how to specify how to restrict to concepts from a specific terminology. For a more complete treatment see the [SHACL-INSPEC separate document](ap.md) for patterns on how to use the profile in various situations.

><a name="AP1"></a> **Rule AP-1:** An application profile MUST be expressed in a single RDF Dataset (**)

><a name="AP2"></a> **Rule AP-2:** There MUST be a single "application profile resource" with a URI in the RDF Dataset AND it must be the same as the "interoperable specification resource" (introduced in Rule INSPEC-1)

><a name="AP3"></a> **Rule AP-3:** All property shapes with a severity of `sh:VIOLATION` and with at least one constraint (`sh:and` for specialization does not count) are considered **reusable** and they MUST have URIs and MUST also be pointed to from the application profile resource via the `dcterms:hasPart` property

><a name="AP4"></a> **Rule AP-4:** All node shapes with a severity of `sh:VIOLATION` are considered **reusable** and MUST have URIs and MUST also be pointed to from the application profile resource via the `dcterms:hasPart` property

><a name="AP5"></a> **Rule AP-5:** Reusable node shapes are considered **main** if they have a target declaration, otherwise they are considered **supportive**

><a name="AP6"></a> **Rule AP-6:** All reusable node shapes as well all property shapes pointed to from those MUST provide a label and MAY also provide a definition and a usage note

><a name="AP7"></a> **Rule AP-7:** Shapes MAY inherit and further restrict other reusable shapes via the `sh:and` construct

><a name="AP8"></a> **Rule AP-8:** Shapes MAY indicate that they are based on other reusable shapes via the dcterms:isVersionOf property

><a name="AP9"></a> **Rule AP-9:** Shapes that are inherited or based on MAY reside in other RDF Datasets as long as the dataset is pointed to via owl:import AND there is a `prof:isProfileOf` relation between the application profile resources

><a name="AP10"></a> **Rule AP-10:** All classes, properties and terminologies referred to via shapes should be explicitly indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)

## Rules for diagrams - SVG-INSPEC

SVG-INSPEC builds on top of SVG to provide a way to clarify whether objects in a diagram corresponds to an entity recognized in interoperable specifications, i.e. a class, a property, a node-shape, a property-shape, a concept, a terminology, or a concept collection. Also interoperable specifications or foundational specifications should be possible to indicate as well as diagrams and data vocabularies if they are included in the diagram. Note that there are no hard restrictions on the diagrammatic style used. Although it should be noted that it is good idea to choose a style that is well known like something akin to UML class diagrams.

><a name="SVG1"></a> **Rule SVG-1:** An element corresponding to an INSPEC entity MUST have a href pointing to it's URI. 

><a name="SVG2"></a> **Rule SVG-2:** An element corresponding to an INSPEC entity MUST have a custom data attribute on the form 'data-inspec-type="TYPE"' where TYPE is one of foundational, application-profile, diagram, data-vocabulary, class, property, node-shape, property-shape, concept, terminology and concept-collection. If the element corresponds to two things, e.g. both a class and a node-shape they can be listed both with a separating comma, the first should be considered dominant.

><a name="SVG3"></a> **Rule SVG-3:** An element corresponding to an INSPEC entity MAY have an id on the form 'id="d_EID"' where EID is the md5 sum of the entitys URI.

><a name="SVG4"></a> **Rule SVG-4:** An element with type node-shape or property-shape MAY have a custom data attribute on the form 'data-inspec-reusable="true"' if it is reusable according to rule AP-3 or AP-4.

><a name="SVG5"></a> **Rule SVG-5:** An element with type node-shape that is also reusable MAY have a custom data attribute on the form 'data-inspec-weight="WEIGHT"' where WEIGHT is either "main" or "supportive" according to rule AP-5.

(*) Both RDFS and SKOS introduce building blocks (classes and properties) for for defining things (other classes, properties, concepts, collections) in an open world manner. However, both vocabularies and terminologies needs to work in the context of semantic specifications where it is stated explicitly what is included, hence it corresponds to a closed world perspective. Hence, in both RDFS-INSPEC and SKOS-INSPEC there is a restriction to assume that everything needed is provided in the indicated RDF Datasets.

(**) SHACL supports imports declared via owl:import, the rules are written from the perspective that these are respected and all RDF Datasets (potentially recursively) are imported first into a single RDF Dataset.