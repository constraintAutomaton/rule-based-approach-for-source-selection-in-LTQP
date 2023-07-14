## Conclusion
{:#conclusion}

Our preliminary results show that our filter-based link pruning approach for LTQP over fragmented RDF documents
can be effective for significantly reducing the number of links to traverse.
This leads to a significantly lower amount of data to be downloaded, and overall query execution time to be lowered
<span class="comment" data-author="RT">If you remove the experiment with the data dump, then the following can be removed.</span>
<del class="comment" data-author="RT">
The cause of the gain of performance seems to be the reduction of the processing by downloading of fewer data inside the engine, but the increase of the HTTP request seems to reduce the gain in performance. This implies that there might be a tradeoff between
the size of the fragments and their numbers.
</del>

This work opens the possibility for faster traversal-based query execution over fragmented RDF documents, 
given that the data provider uses hypermedia descriptions to characterize the fragmentation
in a structured manner. 
In future work, we will investigate filter-based link pruning for more expressive filter expressions,
and evaluate the approach more extensive over more diverse datasets and queries, and compare our approach with alternative query interfaces.

