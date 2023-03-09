## Abstract
<!-- Context      -->
Linked Datasets can be published on the Web in a large variety of ways.
When publishing datasets in which resources have a certain relationship to each other,
such as bus routes and sensors outputs,
it can be useful for clients to publish them in a fragmented manner,
as they can selectively fetch only specific fragments to answer their needs.
<!-- Need         -->
To enable clients to find data in such fragmented collections,
there is a need for a query execution paradigm that can answer these needs using declarative SPARQL queries.
<!-- Task         -->
In this paper, we investigate Link Traversal Querying Processing (LTQP) as a way to search through such fragments,
as it is able to follow links between fragments.
Traditionally, querying using LTQP can be inefficient due to the number of resources that must be fetched to answer a query.
In our case, we can reduce the number of fragments that need to be visited,
by taking into account the relationships between fragments.
<!-- Object       -->
Concretely, we apply this method over datasets published using the [TREE specification](https://treecg.github.io/specification/).
<!-- Findings     -->
Our findings show that we are able to use the hypermedia descriptions of the TREE specification
to prune links to fragments if they will not contribute to the SPARQL query.
<!-- Conclusion   -->
This method opens the possibility for faster client-side query execution over fragmented Linked Datasets,
such as in the case of sensor data.
<!-- Perspectives -->
As such, this facilitates the creation of more user-friendly applications over decentralized and fragmented Knowledge Graphs.

