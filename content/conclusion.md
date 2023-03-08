## Conclusion
{:#conclusion}

In this paper, we presented a Guided Link Traversal Query Processing approach to optimize query execution over fragmented RDF documents
following the TREE specification. 
We showed that we can exploit the structural properties of the TREE specification and the constraints of the members inside fragments, 
to create a new reachability criterion.
This reachability criterion will look for named node with the predicate `tree:node` and only dereference them if the boolean equation
combining the SPARQL filter expression provided by the user and a boolean equation derived from the `tree:relation` is solvable.

{:.comment data-author="RT"}
The sentence above is too low-level.


This query optimization technique shows how RDF query engines 

{:.comment data-author="RT"}
Not really an optimization technique, more a pruning of sources in link traversal.
RDF query engines -> SPARQL query engines 

can leverage the structural properties of Linked Data documents
to reduce the number of fragment lookups,
and to minimize query execution time. More precisely, in this work we 
open the possibility for faster client side query of fragmented Linked Datasets,
such as in the case of sensor data.