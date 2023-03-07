## Abstract
<!-- Context      -->
Semantically fragmented linked data is a useful method for storing graph-type information,
such as bus routes and city plans, as well as data events streams like sensors outputs.
Link traversal querying is the most effective way to search the entire collection,
since each fragment refers to another and there is not necessarily a summary document of the fragmentation.
<!-- Need         -->
However, querying using link traversal query processing can be inefficient due to the number of resources that must be fetched to answer a query.
<!-- Task         -->
We can reduce the number of fragments that needs to be visited,
by enabling our query engine with the capacity of capturing the semantic link between the fragment evaluating the solvability of the combination of those links
with a SPARQL filter expression provided by the user by using a method called Guided Link Traversal Query Processing.
<!-- Object       -->
We proposed to use that method over documents that follow the [TREE specification](https://treecg.github.io/specification/),
a web specification that structure fragmented document published over the web by letting the data publisher describe the
links between the fragment with a constraint over a property that all the data inside the target fragment must respect using a boolean equation.


