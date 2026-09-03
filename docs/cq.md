## Competency Questions

FaBiO can be used for answering several questions related to bibliographic objects and other entities involved in the publishing process.

In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX dcterms: <http://purl.org/dc/terms/>
    PREFIX fabio: <http://purl.org/spar/fabio/>
    PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    PREFIX frbr: <http://purl.org/vocab/frbr/core#>
    PREFIX prism: <http://prismstandard.org/namespaces/basic/2.0/>
    PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

### CQ1

What are the titles and publication years of all available journal articles?

    SELECT ?title ?year
    WHERE {
    ?article a fabio:JournalArticle ;
            dcterms:title ?title ;
            fabio:hasPublicationYear ?year .
    }

### CQ2

Who are the authors (first and last names) of the article with the DOI '10.1007/s10506-007-9036-2'?

    SELECT ?givenName ?familyName
    WHERE {
    ?article a fabio:JournalArticle ;
            prism:doi '10.1007/s10506-007-9036-2' .
    
    ?paper a fabio:ResearchPaper ;
            frbr:realization ?article ;
            dcterms:creator ?author .
    
    ?author foaf:givenName ?givenName ;
            foaf:familyName ?familyName .
    }

### CQ3

In which journal, volume, and issue was the article with the DOI '10.1007/s10506-007-9036-2'?

    SELECT ?journalTitle ?volume ?issue
    WHERE {
    ?article a fabio:JournalArticle ;
            prism:doi '10.1007/s10506-007-9036-2' ;
            frbr:partOf ?journalIssue .
    
    ?journalIssue a fabio:JournalIssue ;
            prism:issueIdentifier ?issue ;
            frbr:partOf ?journalVolume .
    
    ?journalVolume a fabio:JournalVolume ;
            prism:volume ?volume ;
            frbr:partOf ?journal .
    
    ?journal a fabio:Journal ;
            dcterms:title ?journalTitle .
    }

### CQ4

What are the keywords associated with the article with the DOI '10.1007/s10506-007-9036-2'?

    SELECT ?keyword
    WHERE {
    ?article a fabio:JournalArticle ;
            prism:doi '10.1007/s10506-007-9036-2' .
    
    ?paper a fabio:ResearchPaper ;
            prism:keywords ?keyword .
    }

### CQ5

What are the preferred labels of all subject terms assigned to the article with the DOI '10.1007/s10506-007-9036-2'?

    SELECT ?keyword
    WHERE {
    ?article a fabio:JournalArticle ;
            prism:doi '10.1007/s10506-007-9036-2' .
    
    ?paper a fabio:ResearchPaper ;
            fabio:hasSubjectTerm ?subjectTerm .

    ?subjectTerm a fabio:SubjectTerm ;
            skos:prefLabel ?subjectLabel .
    }
