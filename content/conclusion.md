## Conclusion
{:#conclusion}

Concluding from our preliminary implementation, a GLTQP approach to prune links to fragmented RDF documents following the TREE specification based on SPARQL filter expressions is possible.
We show that we can exploit the structural properties of the TREE specification 
and the hypermedia constraints on links between fragments to reduce the search domain
that a LTQP query engine has to explore.

This work opens the possibility for faster traversal-based query execution over fragmented RDF documents, 
given that the data provider uses hypermedia descriptions to characterize the fragmentation
in a structured manner. 
In future work, we are going to benchmark this GLTQP approach and are going to
empirically prove the efficiency of our method.

