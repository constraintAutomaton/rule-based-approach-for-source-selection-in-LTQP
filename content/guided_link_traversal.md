## Filter pushdown to discriminate link during LTQP
{:#guided_link_traversal}

LTQP in its most naive form has a pseudo-infinite domain [](cite:cites Hartig2016), as a result
it is not possible to evaluate the completeness and to have a stopping condition related to the finding of complete results [](cite:cites hartig2012). 
Also, given the vastness of the web, the overwhelming majority of data sources will not contribute to query completeness (in most realistic queries).
Hartig and Freytag [](cite:cites hartig2012) introduces the concept of reachability criterium, which consists of policies
of IRI to dereference, hence limiting the range of the web that has to be explored to consider to have obtained completeness.
Corrolary, those criteriums serve as a mechanism to prune links that are not relevant to the answer to the query.
This is particularly useful in the context of structured environments such as in the use case of Solid and fragmented datasets
where a priori we have metaknowledge of how to access data related to a query [](cite:cites taelman2023).
With this metaknowledge, the query engine can find triples that will describe the location of the information requested,
hence "guiding" the query engine towards the right data source [](cite:cites verborgh2020). 

Our pushdown filter is based in its use on the concept of reachability criterium.
In the definition by Hartig and Freytag [](cite:cites hartig2012) a reachability criterium is a function discriminating the dereferencing of 
an IRI based on the triple available in a current document a query.
In our approach, we interpret the hypermedia description of the constraint of the fragment as a boolean equation as
exemplify in [](#example-sparql).
In this example, the left hand of the expression, the variable, is contingent on a SPARQL query expression.
It is the variable pertaining to the property that the constraint is targeting.
The property targeted by the relation is defined by the object of the triple having the predicate `tree:path`.
In the example it was $$ ?t $$, because the example of [](#TREE-relation-turtle-example)
has the `tree:path` property `etsi:hasTimestamp`. 
The boolean operator such as; equal, greater than, is described by the [RDF type of the TREE relation](https://treecg.github.io/specification/#Relation), in Turtle serialization it is the object of the triple with the predicate "`a`".
Finally, the literal is simply represented by the object of the triple in the relation,
containing the predicate `tree:value`.
The SPARQL filter expression can naively be converted into a boolean expression.
Hence upon discovery of a document, the query engine gathers the relevant triples to form the boolean expression 
representing the constraint of the next fragment and pushdown the SPARQL filter expression.
After having those two boolean expressions close together they are evaluated to find the satisfiability and upon satisfaction
the IRI targeting the next fragment (`tree:node` in the TREE specification) is added to the link queue following the
concept of reachability criterium.

We made an open-source implementation of this criterium available at [https://github.com/constraintAutomaton/comunica-feature-link-traversal/tree/feature/time-filtering-tree-sparqlee-implementation]() with an associated demo available at
[https://github.com/constraintAutomaton/TREE-Solver-LTQP-demo-engine](). In the rest of this section, we will present our experimental result.
We ran four queries derived from the template of [](#example-sparql) available at 
[https://github.com/constraintAutomaton/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/tree/main/content/code]() 
with three configurations of the DAHCC participant 31 dataset (of 487 MB) and a timeout of two minutes. 
The first one is a single endpoint query of the data dump. 
For the second and the third configuration we convert the data dump into a 
fragmented dataset following the TREE specification with a one-ary tree topology with 100 and 1000 nodes.
the second configuration uses  a reachability criterium where we follow each link of the fragmented dataset and the third one with the 
pushdown filter approach. As shown in {{:todo }} no query were able to be answered by querying the data dump and following every fragment. 
The reason might be because of the size of the dataset to be processed before executing the joining and solution mapping.
For the configurations with a pushdown filter method, we see that the queries with 1000 fragments perform better than
the one with 100 fragments, particularly when the query has one or zero results. In those cases, the query execution time is almost one
order of magnitude lower with the largest number of fragments. 
With Q3 we can see that the percentage of reduction is 32%, this lowering of performance might be caused by the increase by a factor of 17
of HTTP requests. The query Q4 was not able to be answered, with both fragmentations, the cause might be because it covers a far larger range and hence requires more data to be downloaded and more processing time.
