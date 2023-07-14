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
Concretely, we interpret the hypermedia descriptions of constraints in TREE fragments as boolean equations
as shown in [](#TREE-relation-turtle-example).
Upon discovery of a document, the query engine gathers the relevant triples to form the boolean expression 
representing the constraint of the next fragment and effectively pushed down the SPARQL filter expression into the engine's dereferencing component.
After having those two boolean expressions close together they are evaluated to find the satisfiability and upon satisfaction
the IRI targeting the next fragment is added to the link queue following the concept of reachability criterium.

We have implemented our approach into the LTQP version of [Comunica](cite:cites comunica) at
[https://github.com/constraintAutomaton/comunica-feature-link-traversal/tree/feature/time-filtering-tree-sparqlee-implementation]() with an associated demo available at
[https://github.com/constraintAutomaton/TREE-Solver-LTQP-demo-engine]().

## Experimental results

To evaluate our approach, we executed four queries similar to the one in [](#example-sparql).
All queries are available at [https://github.com/constraintAutomaton/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/tree/main/content/code](https://github.com/constraintAutomaton/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries/tree/main/content/code).
These queries were executed with two configurations that will be described below.
They were executed over the DAHCC participant 31 dataset (of 487 MB) with a timeout of two minutes.
We fragmented the dataset following the TREE specification with a one-ary tree topology with 100 and 1000 nodes.
<span class="comment" data-author="RT">Mention what timeout you set</span>

<figure id="results-queries" markdown="1" class="table table-smaller-font">

| Configuration                              | Query       | Time (ms) | Req | Res    |
|--------------------------------------------|-------------|---------|----|-------|
| One-ary 100/1000 fragments                 | Q1/Q2/Q3/Q4 | X       | X  | X     |
| One-ary 100/1000 fragments Filter pushdown | Q4          | X       | X  | X     |
| One-ary 100 fragments Filter pushdown      | Q1          | 8,892   | 3  | 0     |
| One-ary 100 fragments Filter pushdown      | Q2          | 3,541   | 3  | 1     |
| One-ary 100 fragments Filter pushdown      | Q3          | 59,274  | 8  | 8,166 |
| One-ary 1000 fragments Filter pushdown     | Q1          | 1,171   | 3  | 0     |
| One-ary 1000 fragments Filter pushdown     | Q2          | 734     | 3  | 1     |
| One-ary 1000 fragments Filter pushdown     | Q3          | 39,987  | 51 | 8,166 |

<figcaption markdown="block">
Result of the query with our different configurations. *Time* is the query execution time,
*Req* The number of HTTP request and *Res* the number of result.
<span class="comment" data-author="RT">Say what X means</span>
</figcaption>
</figure>

<span class="comment" data-author="RT">The first row in this table is a bit confusing. Could we instead have filter-pushdown (yes/no) as separate column, and just show X for every row without filter pushdown?</span>

In the first configuration we use a reachability criterium where the engine follow each link of the fragmented dataset.
For the second one we use our pushdown filter approach.
As shown in [](#results-queries) no query could be answered below the timeout value by following every fragment. 
The reason is the very large size of the dataset to be traversed before executing the joining and solution mapping.
For the configurations with our filter pushdown method, we see that the queries with 1000 fragments perform better than
the one with 100 fragments, particularly when the query has one or zero results. In those cases, the query execution time is almost one
order of magnitude lower with the largest number of fragments. 
With Q3 we can see that the percentage of reduction is 32%, this lowering of performance might be caused by the increase by a factor of 17
of HTTP requests. The query Q4 was not able to be answered, with both fragmentations, because it covers a far larger range and hence requires more data to be downloaded and more processing time.
