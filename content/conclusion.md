## Conclusion
{:#conclusion}

In this paper, we presented a GLTQP approach to 
prune links to fragmented RDF documents following the TREE specification based on SPARQL filter expressions. 
We showed that we can exploit the structural properties of the TREE specification 
and the hypermedia constraints on links between fragments to reduce the search domain
that a LTQP query engine has to explore.

{:.comment data-author="RE"} 
Not a huge fan of past tense in conclusion, would prefer "In this paper, we present GLTQP ..." 
and "We show that we can exploit" But I'm pretty sure its just preference based 

This work opens the possibility for faster traversal-based query execution over fragmented RDF documents, 
given that the data provider uses hypermedia descriptions to characterize the fragmentation
in a structured manner. 
It comes then that our work is a motivation for more structured fragmented RDF documents
and for the implementation of this a priori knowledge inside client-side SPARQL query engines.

{:.comment data-author="RE"} 
"It follows that our work is ...." Not sure what is meant by: "It comes then ... "
