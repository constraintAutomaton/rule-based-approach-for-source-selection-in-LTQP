## Abstract
<!-- Context      -->
Linked Datasets can be published on the Web in a large variety of ways.
When publishing datasets in which resources have a certain relationship to each other,
such as bus routes, city plans, and data events streams like sensors outputs,
it can be useful for clients to publish them in a fragmented manner,
as they can selectively fetch only specific fragments to answer their needs.
<!-- Need         -->
To enable clients to find data in such fragmented collections,
there is a need for a query execution paradigm that can answer these needs using declarative SPARQL queries.
<!-- Task         -->
In this paper, we investigate Link Traversal Querying Processing (LTQP) as a way to search through such fragments,
as it is able to follow links between fragments.
Traditionally, querying using LTQP can be inefficient due to the number of resources that must be fetched to answer a query.
In our case, we can reduce the number of fragments that needs to be visited,
by taking into account the relationships between fragments,
and mapping it to the retrieval needs in the current query.
<!-- Object       -->
Concretely, we apply this method over datasets published using the [TREE specification](https://treecg.github.io/specification/),
<del class="comment" data-author="RT">
which structures fragmented documents published over the web by letting the data publisher describe the
links between the fragments with a constraint over a property of the member inside the fragment. 
That constraint must be followed by every member inside the fragment and can be interpreted as a boolean equation.
</del>

<!-- Findings     -->
TODO
<!-- Conclusion   -->
TODO
<!-- Perspectives -->
TODO

{:.comment data-author="RT"}
Make sure to also talk about findings, conclusions, and perspectives.