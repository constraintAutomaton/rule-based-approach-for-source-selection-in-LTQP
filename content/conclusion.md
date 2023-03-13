## Conclusion
{:#conclusion}

In this paper, we present a GLTQP approach to 
prune links to fragmented RDF documents following the TREE specification based on SPARQL filter expressions. 
We show that we can exploit the structural properties of the TREE specification 
and the hypermedia constraints on links between fragments to reduce the search domain
that a LTQP query engine has to explore.

This work opens the possibility for faster traversal-based query execution over fragmented RDF documents, 
given that the data provider uses hypermedia descriptions to characterize the fragmentation
in a structured manner. 
It follows that our future work is to benchmark our GLTQP with different solver approaches, to
empirically prove the efficiency of our method.

