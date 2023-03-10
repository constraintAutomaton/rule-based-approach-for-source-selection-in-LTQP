# Guided Link Traversal Query Processing over RDF documents fragmented with TREE [poster article]

## Abstract
Linked Datasets can be published on the Web in a large variety of ways.
When publishing datasets in which resources have a certain relationship to each other,
such as bus routes and sensors outputs,
it can be useful for servers to publish them in a fragmented manner,
as the clients can selectively fetch only specific fragments to answer their desire.
To enable clients to find data in such fragmented collections,
there is a need for a query execution paradigm that can fulfill these desire using declarative SPARQL queries.
In this paper, we investigate Link Traversal Querying Processing (LTQP) as a way to search through such fragments,
as it is able to follow links between fragments.
Traditionally, querying using LTQP can be inefficient due to the number of resources that must be fetched to answer a query.
In our case, we can reduce the number of fragments that need to be visited,
by taking into account the relationships between fragments.
Concretely, we apply this method over datasets published using the [TREE specification](https://treecg.github.io/specification/).
Our findings show that we are able to use the hypermedia descriptions of the TREE specification
to prune links to fragments that will certainly not contribute to the SPARQL query.
This method opens the possibility for faster client-side query execution over fragmented Linked Datasets,
such as in the case of sensor data.
As such, this facilitates the creation of more user-friendly applications over decentralized and fragmented Knowledge Graphs.

## Development mode
```
bundle install
bundle exec guard
```

## Build
```
bundle install
bundle exec nanoc compile
```

View on http://localhost:3000/

This article makes use of the [ScholarMarkdown](https://github.com/rubensworks/ScholarMarkdown/) framework.
