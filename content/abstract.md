## Abstract
<!-- Context      -->
Linked data published on the web can naturally be interpreted as one fragmented knowledge graph.
<!-- Need         -->
The query of subgraph has been well studied in the context of single endpoints
and of multiple enpoints in federated queries.
However, in some contexts, it is not possible to know prior to the query execution the location
of those subgraphs.
Those subgraphs can also be linked following logical expressions,
for instance, data published after the first of September is located at this URI.
<!-- Task         -->
We propose to use the concept of structural assumption for publication
and querying of those documents using a lightweight arithmetic solver to discriminate
subgraphs based on the query sent by the user.
This pruning mecanism aims at reducing the query execution time
by avoiding costly unnecessary HTTP requests.
<!-- Object       -->
This poster paper presents our strategy and early experimental results.
<!-- Findings     -->
Our preliminary findings show that by using this strategy,
we are able to significantly reduce the number of HTTP requests and the query execution time
when executing queries aligned with the fragmentation of the result.
<!-- Conclusion   -->
Given the promising result of the initial approach, in future work, 
we aim to extend our solver to support string and geospatial expressions,
to support more general lightweight reasoning and to support the streaming of 
results for queries with filter expression aligned with the data fragmentation and the order of traversal.

