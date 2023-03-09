## Introduction
{:#introduction}

Today, a large number of RDF documents are hosted on the web [](cite:cites Ermilov2013, Verborgh2016TriplePF).
One way to publish Linked Data is by hosting it into one RDF file,
which is simple for the data publisher.
The data publisher can also publish its data in a fragmented manner,
which can be beneficial for query execution time as have been demonstrated by
[Triple Pattern Fragments](cite:cites Verborgh2016TriplePF).
The fragmentation also helps the users by giving them the possibility to query only the segment of data necessary for them [](cite:cites ColpaertMaterializedTREE, lancker2021LDS). 
It's also help client application to enable functionalities by having the data already process 
and store into fragments, 
in a way that most of the work consist in querying the data,
such as in the case of [client side auto completion](https://tree.linkeddatafragments.org/demo/autocompletion/)
and client-side route planning [](cite:cites Delva2020, Delva2020a). 
The disadvantage of fragmenting the document is that there are multiple sources of truth that the user
has to query to find the information needed , which means more processing power is needed on the client side. 


For this paper, we consider the following example,
a user want to get the temperature of all the unit of a product that has been stored before or at the first of January 2023.
The user decides to execute the query presented at [](#example-sparql).
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
where $$ x $$ is any variable inside the client SPARQL query that as the predicate `ex:publication_date`.
</figcaption>
</figure>
</div>

As there are a large number of units, the data provider decides 
to fragment the data set using the [TREE specification](cite:cites spec:tree),
a Web specification that structured the publication of fragmented RDF documents.
More precisely, the specification offers the possibility to the data publisher to describe a boolean constraint on a property of the data inside each fragment.
The fragments are also linked to each other with a description of the constraint.
Given those constraints and the SPARQL filter expression,
it is possible to select the fragments that can possibly contribute to the answer to the query.

The query engine can then apply a query paradigm called,
Guided Link Traversal Query Processing (GLTQP) [](cite:cites verborgh2020, taelman2023)
a derivative of Link Traversal Query Processing (LTQP).
This querying paradigm consists of recursively looking up new documents
by following links inside them [](cite:cites Hartig2013AnOO).
GLTQP differ from LTQP by using knowledge capture from triples during the query execution to increase the selectivity
of data sources, or/and by using a priori knowledge pertaining to the structure
of RDF documents to reduce the search domain.

