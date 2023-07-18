## Abstract
<!-- Context      -->
Despite its well-known availability issues,
SPARQL endpoints remain to be the most popular approach to publish Linked Data on the Web.
In recent years, the TREE hypermedia specification was introduced as an alternative approach,
aiming to strike a better balance on query processing responsibility
by enabling clients to autonomously follow hypermedia links
over domain-oriented data fragmentation strategies.
<!-- Need         -->
However, all existing TREE-based query solutions
resort to use-case-specific and hard-coded traversal of fragments.
A need arises to hide away the complexities for developers
that want to query data in TREE-fragmented datasets through generic SPARQL.
<!-- Task         -->
We propose building upon the Link Traversal Query Processing paradigm to follow links between TREE fragments.
We optimize it by applying a filter pushdown strategy
whereby only those links to fragments containing data that match a SPARQL FILTER expression will be followed.
<!-- Object       -->
This poster paper presents our strategy and early experimental results.
<!-- Findings     -->
Our preliminary findings show that by using this strategy,
we are able to drastically reduce the number of HTTP requests and the query execution time
when evaluating time filtered queries over sensor data.
<!-- Conclusion   -->
Given the promising result of initial approach,
In the future, we aim to support more complex filter expressions,
extend the evaluation to include additional datasets
and compare our results with other SPARQL interfaces.
<!-- Not sure you need this below?   -->
<p>
    <b><i>Poster paper</i></b>
</p>
