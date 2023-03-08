## Abstract
<!-- Context      -->
Semantically fragmented linked data is a useful method for storing graph-type information,
such as bus routes and city plans, as well as data events streams like sensors outputs.
Link traversal querying processing is a good way to search the entire collection,
because the fragment refers one another 
and in a lot of cases there is not a summary document to lookup fragments based on the client query.
<!-- Need         -->
However, querying using link traversal query processing can be inefficient due to the number of resources that must be fetched to answer a query.
<!-- Task         -->
We can reduce the number of fragments that needs to be visited,
by enabling our query engine with the capacity of capturing the semantic link between the fragment 
and evaluating the solvability of the combination of those semantic links
with a SPARQL filter expression provided by the user. This variance of Link traversal is called Guided Link Traversal Query Processing.
<!-- Object       -->
We proposed to use that method over documents that follow the [TREE specification](https://treecg.github.io/specification/),
a web specification that structure fragmented document published over the web by letting the data publisher describe the
links between the fragments with a constraint over a property of the member inside the fragment. 
That constraint must be followed by every member inside the fragment and can be interpreted as a boolean equation.


