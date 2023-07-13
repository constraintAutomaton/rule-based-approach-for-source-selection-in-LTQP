## Conclusion
{:#conclusion}

Concluding from our preliminary implementation, a pushdown filter approach to prune links of fragmented RDF documents based on the SPARQL query language performs better than an approach without it and then querying a data dump of approximately haft a gigabyte in size. 
The cause of the gain of performance seems to be the reduction of the processing by downloading of fewer data inside the engine, but the increase of the HTTP request seems to reduce the gain in performance. This implies that there might be a tradeoff between
the size of the fragments and their numbers.

This work opens the possibility for faster traversal-based query execution over fragmented RDF documents, 
given that the data provider uses hypermedia descriptions to characterize the fragmentation
in a structured manner. 
In future work, we are going to make a more extensive evaluation of solutions and compare it with a SPARQL endpoint and TPF interface.

