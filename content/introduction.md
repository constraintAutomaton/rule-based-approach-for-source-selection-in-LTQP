## Introduction
{:#introduction}

Today, a large number of RDF documents are hosted on the Web [](cite:cites Verborgh2016TriplePF).
One way to publish Linked Data is by hosting it into one RDF file,
which is simple for the data publisher.
The data publisher can also publish its data in a fragmented manner,
which can be beneficial for query execution time,
as demonstrated by [Triple Pattern Fragments](cite:cites Verborgh2016TriplePF).
Furthermore, the fragmentation helps the users by giving them the possibility to fetch only the relevant data subsets [](cite:cites ColpaertMaterializedTREE). 
Additionally, by storing pre-processed data into fragments, client-side applications can reduce computational load during resource intensive tasks like [auto-completion](https://tree.linkeddatafragments.org/demo/autocompletion/) and client-side route planning [](cite:cites Delva2020).
The disadvantage of fragmenting such datasets is that there are multiple data sources that the user
has to query to find the information needed, which means more HTTP lookups,
hence a potential higher client side query execution time. 

In the scope of this paper, we consider the following use case;
a user wants to determine the temperature of all sensor logs that have been created before or at the first of January 2023,
which can be expressed in SPARQL as shown in [](#example-sparql).

<div class="sidebysidecontainer">
<figure id="example-sparql" class="listing" style="padding-right: 5px; padding-left: 5px">
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
where $$ x $$ is any variable inside the client SPARQL query that as the predicate `dct:created`.
</figcaption>
</figure>
</div>

As there are many sensor logs, the data provider decides
to fragment the dataset using the [TREE specification](cite:cites spec:tree).
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
