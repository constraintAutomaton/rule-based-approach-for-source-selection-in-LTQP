# [How TREE hypermedia can speed up Link Traversal-based Query Processing for SPARQL queries with filters](https://constraintautomaton.github.io/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/)

Poster article written to the [ESWC 2023](https://2023.eswc-conferences.org/), **Not yet published** .

Accessible online at https://constraintautomaton.github.io/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/

## Abstract
Linked Data can be published on the Web in a variety of APIs such as in subject pages or in a SPARQL endpoint.
Each API has its own set of trade-offs:
in the former, the client needs to perform a rather slow link traversal algorithm to find an answer to any query, but the 
API is cheap to host; while in the latter the server is able to provide a rather fast and concise answer to a 
well-defined query, but the API is more expensive to maintain. 
An in-between solution could work using guided link traversal and hypermedia, in which the server already exposes hints 
of an ordering of subjects in a search space.
The [TREE hypermedia specification](https://w3id.org/tree/specification) allows to define such relations between pages (fragments) containing zero or more members of the collection.
In this paper, we investigate a Guided Link Traversal-based Query Processing (GLTQP) approach to search through such 
interlinked fragments, and reduce the number of fragments that need to be visited by taking into account the described relations.
Our findings show that we are able to use the hypermedia descriptions of the TREE specification to prune links to 
fragments that will certainly not contribute to the SPARQL query based on a reachability criteria that can be
the basis for a solver resolving a boolean equation combinaing the SPARQL FILTER expression and TREE Relation descriptions.
This solver has been made available as open-source to demonstrate how it can generate faster results on the client for 
SPARQL queries with FILTER clauses over TREE fragmented Linked Datasets.

## Editing the article
### Development mode
```
bundle install
bundle exec guard
```

### Build
```
bundle install
bundle exec nanoc compile
```

View on http://localhost:3000/

**This article makes use of the [ScholarMarkdown](https://github.com/rubensworks/ScholarMarkdown/) framework.**
