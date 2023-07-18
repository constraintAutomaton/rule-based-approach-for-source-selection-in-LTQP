## Pruning links using filter expressions
{:#guided_link_traversal}

Most research around LTQP has been done on query execution over Linked Open Data [](cite:cites Hartig2016).
Given the pseudo-infinite number of documents on the Web, traversing over all documents is practically infeasible.
To get a grasp on this completeness issue, different [reachability criteria](cite:cites hartig2012) were introduced
that allow certain links not to be followed based on the query that is executed.
Recently, an [alternative direction was introduced](cite:cites verborgh2020)
where instead of just looking at the query to determine which links to follow,
the data publisher could _guide_ the engine towards query-relevant links.
This strategy was [applied on the Solid ecosystem](cite:cites taelman2023),
exploiting structural properties of Solid to prune links when traversing over Solid vaults.

Our approach builds upon this concept of guided link traversal,
and similar to the [Solid approach](cite:cites taelman2023), exploits the structural properties of TREE datasets to prune links.
Concretely, we interpret the hypermedia descriptions of constraints in TREE fragments as boolean equations
as shown in [](#TREE-relation-turtle-example).
Upon discovery of a document, the query engine gathers the relevant triples to form the boolean expression
representing the constraint of the next fragment and effectively pushes down the SPARQL filter expression into the engine's dereferencing component.
After having those two boolean expressions close together they are evaluated to determine if the are satisfied,
in which case,
the IRI targeting the next fragment is added to the link queue following the concept of reachability criterium.

We have implemented our approach in a fork of the LTQP version of [Comunica](cite:cites comunica) at
[https://anonymous.4open.science/pr/3785]() with an associated demo available at
[https://anonymous.4open.science/r/TREE-Solver-LTQP-demo-engine-21FF/]().

## Experimental results

To evaluate our approach, we executed four queries similar to the one in [](#example-sparql).
All queries are available at [https://anonymous.4open.science/r/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries-8BE8/content/code/](https://anonymous.4open.science/r/How-TREE-hypermedia-can-speed-up-Link-Traversal-based-Query-Processing-queries-8BE8/content/code/).
These queries were executed with two configurations that will be described below.
They were executed over the DAHCC participant 31 dataset (of 487 MB) with a timeout of two minutes.
We fragmented the (same) dataset following the TREE specification with a one-ary tree topology with 100 and 1000 nodes.

<figure id="results-queries" markdown="1" class="table table-smaller-font">

| Fragments | Query | Time (ms) | Req | Res | Time-pushdown (ms) | Req-pushdown | Res-pushdown |
|-----------|-------|-----------|-----|-----|--------------------|--------------|--------------|
| 100       | Q1    | X         | X   | X   | 8,892              | 3            | 0            |
| 100       | Q2    | X         | X   | X   | 3,541              | 3            | 1            |
| 100       | Q3    | X         | X   | X   | 59,274             | 8            | 8,166        |
| 100       | Q4    | X         | X   | X   | X                  | X            | X            |
| 1000      | Q1    | X         | X   | X   | 1,171              | 3            | 0            |
| 1000      | Q2    | X         | X   | X   | 734                | 3            | 1            |
| 1000      | Q3    | X         | X   | X   | 39,987             | 51           | 8,166        |
| 1000      | Q4    | X         | X   | X   | X                  | X            | X            |

<figcaption markdown="block">
Result of the query with our different configurations.
X means that the query did not finish before the timeout, _Fragment_ is the number of fragments,
_Time_ is the query execution time, _Req_ The number of HTTP request and _Res_ the number of result. 
The pushdown suffix indicates that the use of the pushdown filter configuration.
</figcaption>
</figure>

In the first configuration we use a reachability criterium where the engine follows each link of the fragmented dataset.
For the second one we use our pushdown filter approach.
As shown in [](#results-queries) no query could be answered below the timeout value by following every fragment.
The reason is the very large size of the dataset to be traversed before executing the joining and solution mapping.
For the configurations with our filter pushdown method, we see that the queries with 1000 fragments perform better than
the one with 100 fragments, particularly when the query has one or zero results. In those cases, the query execution time is almost one order of magnitude lower with the largest number of fragments.
With Q3 we can see that the percentage of reduction is 32%, this lowering of performance might be caused by the observed increase by a factor of 17 of HTTP requests. 
The query Q4 was not able to be answered, with both fragmentations, because it covers a far larger range and hence requires more data to be downloaded and more processing time.
