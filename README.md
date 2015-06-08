
# Notes for Open Repositories 2015

## Day 1

### Fedora 4 Workshop

+ Morning
    + Introductions
    + What is a Fedora Repositories?
        + Original Acronym: Flexible Extensible Durable Object Repository Architecture
        + Support for semantic relationships
            + in 4 linked data Support
            + RDF
            + RESTful API with native RDF response
            + See wiki prospectus for 4 primary goals
            + Flexible storage options
            + Improved platform for developers
         + Fedora 3 is a 15 year old codebase
         + Fedora 3 is hard to work on and the institutional knowledge of what can be cut is missing
         + Fedora 3 has scaling problems
         + Fedora 4 is built on RDF and Portland Common Data Model
         + Native hierarchy is a tree:
            + Container
                + Container|binary
        + single top level root container but that can contain many containers
        + Resources have properties (name/value pairs)
    + ModeShape provides foundation over Infinispan for REST Framework and Fedora Services
    + Stack from bottom (storage) to top (http layer)
        + storage (Clustering)
        + [Infinispan](http://infinispan.org/) (Clustering)
        + [ModeShape](http://modeshape.jboss.org/) (Repository Services)
        + REST Framework/Fedora Services
    + Core Features and Standards
        + CRUD - LDP (Linked Data Platform, aka LDP 1.0)
        + Versioning - [Momento](http://www.mementoweb.org/guide/)? (moving to this standard, has custom version right now)
        + Authorization - [WebAC](http://www.w3.org/wiki/WebAccessControl)? (Web Access Control, linked data to make assertions about authority) (Not implemented yet, will be pluggable)
            + No-op
            + Role-Based
            + XACML (currently implemented in 3 and will be available in 4)
            + WebACL coming
            + Note: Fedora does not handle authentication, only authorization
        + Transactions
            + 4 only
        + Fixity
            + overtime digital objects and file can be corrupt (e.g. bit rot)
            + check summing, fixity is the API to see if this has happened
        + Import/Export - RDF export?
            + Camel is the tool currently being used
    + Non-Core Features
        + Pluggable components
        + External components  (consume/act off messages from repostiory)
            + examples include indexing, Solr integration, Elastric Search, etc.
            + Camel is an implementation of adding a listener that then triggers process desired
        + External - Tripplestore
            + Any triple store (SPARCL update supported tripple store)
                + Fuseki/Sesame have been tested
                + Tripple store will be how the SPARCL interface for querying
             + Fedora stores tripples but is NOT a tripplestore itself, tripple store provides query layer
        + External - Audit Service
    + Metrics (see wiki performance metrics for version 4)
        + uploaded 1 TB file via REST API
        + 16 milliojn objects via federation
        + 10 million objects via REST API
    + Road Map (next 6 months)
        + Web Access Control
        + Async storage
        + Human-readable file system

## Migration 3 to 4

+ Fedora 3
    + [FOXML](http://fedora-commons.org/download/2.0/userdocs/digitalobjects/introFOXML.html)
    + Inline XML and XML datastreams
+ Fedora 4 (ModeShape based)
    + Web resources (containers & binaries)
    + RDF properties and XML binaries
+ Flat vs. hierarchy
    + Fedora 3
        + Objects and data streams at the top level
        + No inherent tree structure
    + Fedora 4
        + Containers and binaries in a hierarchy
        + All resources descend from a single root resource
+ File system
    + Fedora 3
        + Objects directory and datastreams directory
        + Both objects and datastreams are in a PairTree
    + Fedora 4
        + Containers directory and binaries directory
        + Containers in a database (e.g. LevelDB)
        + Datastreams in a PairTree
+ PID vs. Path
    + Fedora 3
        + Objects have Persistent Identifiers (PIDs)
        + An object's PID can never ne altered
    + Fedora 4
        + Resources have an internal UUID
        + Resources have a repository Path (primary identifier for resource)
        + This can be user-defined or generated via a PID-minter
 + Linked Data
    + Fedora 4 conforms to the LDP 1.0
    + Metadata can be represented as RDF triples that point to resources inside and outside the repository
    + Many possibilities for exposing, importing, sharing resources with the broader Web
    + E.g. Portland Common Data Model is LDP based
+ Benefits of LDP and Linked Data
    + Interoperability:
        + Between different Fedora implementations
        + Between Fedora and LDP clients
        + Between Fedora and linked data Services
        + Common Data models like Portland Common Data Model
    + Authority Control
    + Discoverability
+ RDF Serializations Formats
    + RDF can be serialized into a number of Formats
        + JSON-LD
        + Turtle
        + N-Triples
        + N3
        + XML
+ Ontologies
    + Ontologies are formal specification of shared conceptualizations
    + Well-known RDF Ontologies:
        + Dublin Core
        + FOAF
        + shema.org+ SKOS
        + BIBFRAME
        + ...
+ Vocabularies
    + Vocabularies provide a controlled list of authorities, each with a user-defined
    + Well-known Vocabularies:
        + DBpedia
        + Library of Congress Subject Headings
        + Getty Vocabularies
        + Virtual International Authority File
+ Data Modeling and mappings
    + from the data via RDF to Modeling
    + A data model defines intellectual concepts and relationships with one another
    + |Collection| -> contains -> |object| -> contains -> |file|
    + Mapping involves translating your data (including relationships) to fit the model
    + Mapping Object Properties (table given in slide as an example between Fedora 3, Fedora 4 and example)
    + Mapping datastreams table
+ [Portland Common Data Model](https://wiki.duraspace.org/display/FF/Portland+Common+Data+Model)
    + Collections (Access, Descriptive)
    + Objects (conceptual level: Access, Descriptive)
    + Files (instance of: Access, Bitstream, Descriptive, Technical)
    + buys Interoperability
+ Ordering
    + implemented via proxies
    + Optional Ordering Contruction slide (e.g. everyone page could have a project)
    + firt, last, prev, next are the only predicates
    + hasMember
    + proxyIn, proxyFor
+ Data migration tools
    + Motivations
        + Preserving Fedora 3, content , history and audit log
        + Leverage Fedora 4 Features
        + Make data accessible
    + Fedora-based "migration-utils"
    + Hydra-based "fedora-migrate" (ruby gem)

  
## Background Notes

+ Best practices is do not expose Fedora 4 URL (future proof for migrations) but through a resolver service
+ Let Fedora build the hierarchy that is semantically meaningless but used RDF to build query structure, this is because there is a bug in ModeSpace where performance will degrade if the hierarchy is deep or unbalanced
+ Hydra Community into RDF everything
+ [Vagrant](http://docs.vagrantup.com/v2/)
+ [Introduction to RDF](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XUL/Tutorial/Introduction_to_RDF) at Mozilla
+ [Intro to Linked Data](http://linkeddata.org/guides-and-tutorials) also called LDP, Linked Data Platform
+ Tripples --> |Subject (noun)| Predicate (verb) | Object (noun) |
