## Pruning links using filter expressions
{:#guided_link_traversal}

Most research around LTQP has been done on query execution over Linked Open Data [](cite:cites Hartig2016).
Given the pseudo-infinite number of documents on the Web, traversing over all documents is practically infeasible.
To get a grasp on this completeness issue, different [reachability criteria](cite:cites hartig2012) were introduced
that allow certain links not to be followed based on the query that is executed.
Recently, an [alternative direction was introduced](cite:cites verborgh2020)
were instead of just looking at the query to determine which links to follow,
the data publisher could _guide_ the engine towards query-relevant links.
This strategy was [applied on the Solid ecosystem](cite:cites taelman2023),
where the structural properties of Solid were exploited to prune links when traversing over Solid vaults.

Our approach builds upon this concept of guided link traversal,
and similar to the [Solid approach](cite:cites taelman2023) exploits the structural properties of TREE datasets to prune links.
Concretely, we interpret the hypermedia descriptions of constraints in TREE fragments as boolean equations.
For example, given the query in [](#example-sparql),
the left hand of the expression, the variable, is contingent on a SPARQL query expression.
It is the variable pertaining to the property that the constraint is targeting.
The property targeted by the relation is defined by the object of the triple having the predicate `tree:path`.
In the example it was $$ ?t $$, because the example of [](#TREE-relation-turtle-example)
has the `tree:path` property `etsi:hasTimestamp`.
Boolean operators such as; equal, greater than, are described by the [RDF type of the TREE relation](https://treecg.github.io/specification/#Relation).
Finally, the literal is simply represented by the object of the triple in the relation,
containing the predicate `tree:value`.
Upon discovery of a document, the query engine gathers the relevant triples to form the boolean expression 
representing the constraint of the next fragment and effectively pushed down the SPARQL filter expression into the engine's dereferencing component.
After having those two boolean expressions close together they are evaluated to find the satisfiability and upon satisfaction
the IRI targeting the next fragment (`tree:node` in the TREE specification) is added to the link queue following the
concept of reachability criterium.
<figure id="results-queries" markdown="1" class="table table-smaller-font">

| Configuration                              | Query       | QT (ms) | NH | NR    |
|--------------------------------------------|-------------|---------|----|-------|
| Data dump                                  | Q1/Q2/Q3/Q4 | X       | X  | X     |
| One ary 100/1000 fragments                 | Q1/Q2/Q3/Q4 | X       | X  | X     |
| One ary 100/1000 fragments Filter pushdown | Q4          | X       | X  | X     |
| One ary 100 fragments Filter pushdown      | Q1          | 8,892   | 3  | 0     |
| One ary 100 fragments Filter pushdown      | Q2          | 3,541   | 3  | 1     |
| One ary 100 fragments Filter pushdown      | Q3          | 59,274  | 8  | 8,166 |
| One ary 1000 fragments Filter pushdown     | Q1          | 1,171   | 3  | 0     |
| One ary 1000 fragments Filter pushdown     | Q2          | 734     | 3  | 1     |
| One ary 1000 fragments Filter pushdown     | Q3          | 39,987  | 51 | 8,166 |

<figcaption markdown="block">
Result of the query with our different configurations. *Qt* is the query execution time,
*NH* The number of HTTP request and *NR* the number of result.
</figcaption>
</figure>

We have implemented our approach into the LTQP version of [Comunica](cite:cites comunica) at
[https://github.com/constraintAutomaton/comunica-feature-link-traversal/tree/feature/time-filtering-tree-sparqlee-implementation]() with an associated demo available at
[https://github.com/constraintAutomaton/TREE-Solver-LTQP-demo-engine]().

<span class="comment" data-author="RT">Let's move everything below this to a separate section</span>

To evaluate our approach, we executed four queries similar to the one in [](#example-sparql).
All queries are available at [https://github.com/constraintAutomaton/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/tree/main/content/code]().
These queries were executed with three configurations <span class="comment" data-author="RT">what configurations?</span>
They were executed over the DAHCC participant 31 dataset (of 487 MB) with a timeout of two minutes. 
The first one <span class="comment" data-author="RT">Are you explaining the configurations here? If so, make that explicit.</span> is a single endpoint query of the data dump. 
For the second and the third configuration we convert the data dump into a 
fragmented dataset following the TREE specification with a one-ary tree topology with 100 and 1000 nodes.
the second configuration uses a reachability criterium where we follow each link of the fragmented dataset and the third one with the 
pushdown filter approach. As shown in {{:todo }} no query could be answered by querying the data dump and following every fragment. 
The reason might be because of the size of the dataset to be processed before executing the joining and solution mapping.
For the configurations with a pushdown filter method, we see that the queries with 1000 fragments perform better than
the one with 100 fragments, particularly when the query has one or zero results. In those cases, the query execution time is almost one
order of magnitude lower with the largest number of fragments. 
With Q3 we can see that the percentage of reduction is 32%, this lowering of performance might be caused by the increase by a factor of 17
of HTTP requests. The query Q4 was not able to be answered, with both fragmentations, the cause might be because it covers a far larger range and hence requires more data to be downloaded and more processing time.

<span class="comment" data-author="RT">Honestly, I don't think we need to talk about the dataset experiment to make our point. I would suggest just comparing the TREE traversal approach _with_ and _without_ filter-based link pruning.</span>
