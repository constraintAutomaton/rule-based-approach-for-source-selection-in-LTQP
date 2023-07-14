## Abstract
<!-- Context      -->
RDF and Linked Data can be used to model and publish various kinds of data through a variety of different interfaces,
of which the SPARQL endpoint is the most popular.
However, its high complexity can often lead to availability issues,
which led to the introduction of alternative interfaces
that offer a tradeoff between the client and the server by fragmenting the dataset in a particular way.
In recent years, TREE was introduced that enables publication through domain-related fragmentation strategies,
where the client has to follow hypermedia links to find and access data.
However, all solutions making use of TREE so far have resorted to use-case-specific manual traversal of fragments.
<!-- Need         -->
To hide away the complexities for users that want to find data in TREE-fragmented datasets,
there is a need for an approach to execute generic SPARQL over such datasets.
<!-- Task         -->
As such, we propose building upon the Link Traversal Query Processing paradigm to follow links between TREE fragments.
Furthermore, we optimize it by applying a filter pushdown strategy
whereby only those links to fragments containing data that match a SPARQL FILTER expression will be followed.
<!-- Object       -->
This paper presents our strategy and early experimental results.
<!-- Findings     -->
Our preliminary findings show that by using this strategy,
we are able to drastically reduce the number of HTTP requests and the query execution time
when evaluating time-related queries over sensor data.
<!-- Conclusion   -->
Given the promising result of initial approach, we improve upon this work in the future
by supporting more complex filter expressions
and evaluating with more datasets and comparing our results with other SPARQL interfaces.
<p>
    <b><i>Poster paper</i></b>
</p>
