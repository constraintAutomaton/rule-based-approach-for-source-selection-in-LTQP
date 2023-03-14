## Introduction
{:#introduction}

Hosting a number of RDF documents is a cost-efficient choice when it comes to Web API design.
One alternative would be to host a SPARQL endpoint, that can generate an infinite amount of RDF documents depending on the precise SPARQL query.
The data publisher however can also publish its data in fragments with a certain locality,
which can lead to an interesting new trade-off between cost-efficiency and query execution when the client takes part in the query execution,
as demonstrated by the work on [Triple Pattern Fragments](cite:cites Verborgh2016TriplePF).
The fragmentation helps the users by giving them the possibility to fetch only the relevant data subsets.

By storing pre-processed data into fragments in various ways, client-side applications can reduce computational load during other tasks as well, like [auto completion](cite:cites ColpaertMaterializedTREE) and client-side route planning [](cite:cites Delva2020).
The disadvantage of fragmenting such datasets is that there are multiple data sources that the user
has to query to find the information needed, which means more HTTP lookups,
hence a potential higher client side query execution time. 
In this paper, we consider the following use case;
a user wants to determine the temperature of all sensor logs that have been created before or at the first of January 2023,
which can be expressed in SPARQL as shown in [](#example-sparql).

<div class="sidebysidecontainer" style="align-items: stretch !important; ">
<figure id="example-sparql" class="listing" style="padding-right: 5px; padding-left: 5px; height='100%'">
````/code/example_sparql_query.ttl````
<figcaption markdown="block">
SPARQL query to get the temperature and the associated unit IRI.
</figcaption>
</figure>

<figure id="TREE-relation-turtle-example" class="listing" style="padding-right: 5px; padding-left: 5px">
````/code/example_tree_relation.ttl````
<figcaption markdown="block">
The example is showing a set of triples representing a TREE relation. 
The relation can be converted into the following boolean equation 
$$ x= unitTime(\text{2023-01-07T00:00:00Z}) $$ 
where $$ x $$ is any variable inside the client SPARQL query that as the predicate `sosa:resultTime`.
</figcaption>
</figure>
</div>

As there are too many sensor logs for one page, the data provider decides
to fragment the dataset using the [TREE specification](https://w3id.org/tree/specification).
TREE enables data publishers to place a boolean constraint on a property of the members inside each fragment.
Furthermore, these fragments linked to each other with a hypermedia description of the constraint.

Given these hypermedia-based constraints,
it is therefore possible for a client-side query engine
to interpret those constraints and use them during SPARQL query execution.
Furthermore, the engine could map the constraints to SPARQL FILTER expressions,
and only fetch those fragments that could contribute to query results.

One approach to achieve this, is to apply a query paradigm called
[Guided Link Traversal Query Processing (GLTQP)](cite:cites verborgh2020)
a specific form of [Link Traversal Query Processing (LTQP)](cite:cites Hartig2013AnOO).
The idea of LTQP consists of recursively looking up new documents
by following links inside them.
GLTQP goes a step further,
by making use of a priori knowledge pertaining to the structure of Linked Data documents and 
knowledge captured during the query execution to reduce the search domain.
