# SPARQL Notes # 
Instructor: Thomas Kelly
## Day 1 ##
* Semantic technology is a DISRUPTIVE Technology.
* W3C Internet Standards.
* RDF Graph Datasets (Resource Description Framework)
* http://lod-cloud.net
* Access Point, RDF Database, Ontology
* Result Formats: RDF, XML,JSON, CSV/TSC, HTML
* SPARQL RDF Adopter to read data from CLoud, Traditional DBs, Hadoop Store, Excel, Internet of Things, Machine-to-Machine.
* Subject-Predicate-Object
* An ENTITY is and instance of a class in Object Oriented Terms.
* URI (Uniform Resource Identifiers) - looks like URL but it points to Data rather than to a WebPage.
* URI is made of two parts : NameSpace and LocalName
* NameSpaces can be Abbreviated using PREFIX
* URIs are based on ASCII. IRIs use Universal Character set (UNICODE)
* Ontology/Semantic Model is different from Entity Relationship Diagram. It is an integral part of RDF database. Database in SYNC with the Model.
* Modeling ontologies/Semantic Models : Dublin Core, RDF, RDFS, XSD, SKOS, OWL 
* Domain ontologies : FOAF,SCHEMA, ORG, GR, GN, TIME and many more like SSN (Semantic Sensor Network) for IOT.
* the websit prefix.cc has detials about ontologies
* Select * in SPARQL can crash you database. 

## Day 2 ##
* http://xmlns.com/foaf/spec/ for FOAF Vocabulory description 
* http://demo.openlinksw.com/sparql  . there are other webites if you cant do few things with this. 
* Use 'links' to traverse databases without naming the databases in your query.
* DBPedia is an RDF implementation of Wikipedia Data. 
* http://dbpedia.org/sparql
  Use this Query:
    Prefix dbo: <http://dbpedia.org/ontology/>
    Prefix foaf: <http://xmlns.com/foaf/0.1/> 
    
    select ?teamURI ?teamName ?teamNickname
    WHERE 
    { 
    ?teamURI a dbo:SportsTeam ;
    foaf:name ?teamName ;
    foaf:nick ?teamNickname .
    } LIMIT 100 
* ';' to end of statment means? how is it differnet from '.' Ans: when ; the left and right tuples share the same subject.
* Kingsley Uyi Idehe - FOllow him on twitter. (lod.openlinksw.com - he sent some link on this website)
* Learn about creating a class and assigning properties to it. 
* OPTIONAL in SPARQL is equivalent to LEFT JOIN in T-sql.
* Hierarchy : RDF Triples >> RDF Graph >> RDF Dataset/Database
* http://environment.data.gov.uk/lab/sparql.html
* We define a class using rdf:type and assign properties by using rdfs:domain
* Sesame is an open source Java Framework. Search in google for "Sesame windows Client" and install. 
* jena.apache.org -Open source from Apache.
* MarkLogic is a commercial tool which has parts of Jena. 
* Gruff is another freely downloadable graphical triple store browser. You can visualize the graphs in Gruff. 
* Deck: http://bit.ly/intro2SPARQL
* End of Training.
