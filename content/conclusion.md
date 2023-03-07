## Conclusion
{:#conclusion}

In this paper, we presented a Guided Link Traversal Query Processing approach to optimize the query of fragmented RDF documents
following the TREE specification. 
We showed that we can exploit the structural aspect of the TREE specification and the constraint of the member inside the fragment, 
to create a new reachability criteria.
This reachability criteria will look for named node with the predicate `tree:node` and only dereference them if the boolean equation
combining the SPARQL filter expression provided by the user and a boolean equation derived from the `tree:relation` is solvable.


This query optimization technique shows how RDF query engine 
can leverage the structure of linked data document to gain a priori knowledge on the data source
to minimize query execution time. More precisely, in this work we 
open the possibility for faster client side query of heavily fragmented RDF documents,
such as in the case of sensor data.