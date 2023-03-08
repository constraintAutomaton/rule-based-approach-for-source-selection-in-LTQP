## Introduction
{:#introduction}

Today, a large number of RDF documents are hosted on the web [](cite:cites Ermilov2013, Verborgh2016TriplePF).
One way to publish Linked Data is by hosting it into one RDF file,
which is simple for the data publisher.
The data publisher can also publish its data in a fragmented manner,
which can be beneficial for query execution time,
for providing only the segment of data necessary for the user and for
helping the client to enable specific functionality,
we will elaborate on those points in the following sentences.
[Triple Pattern Fragments](cite:cites Verborgh2016TriplePF) has shown how fragmentation of the data
on the server side can improve the query execution time.
Fragmentation also give to the client the possibility to acquire slice of data
containing the information needed without having the server create and maintain a specialized interface [](cite:cites ColpaertMaterializedTREE, lancker2021LDS).
Another advantage, related to the previous one is that by fragmenting
the data in an adequate way in relation to a specific objective and such as the query engine can understand the fragmentation,
the client will not need to implement a complex algorithm
to process the information, considering that it tries to accomplish and objective related to the one, the data publisher had in mind.
Then, the client will simply need to query the relevant fragment without having to do a lot of processing.
A concrete example of such application is [client side auto completion](https://tree.linkeddatafragments.org/demo/autocompletion/)
and client-side route planning [](cite:cites Delva2020, Delva2020a).

We consider the following example,
an user want to get the temperature of all the unit of a product that have been store before or at the first of january 2023.
The user decide to execute the query of [](#example-sparql).

<figure id="example-sparql" class="listing">
````/code/example_sparql_query.ttl````
<figcaption markdown="block">
SPARQL query to get the temperature and the associated unit IRI.
</figcaption>
</figure>

As there are a large number of unit the data provider decide to fragment the data set using the [TREE specification](cite:cites spec:tree),
a Web specification that structures the publication fragmented RDF documents.
The specification offer to the posibility to the data publisher to describe a boolean constraint on a property of the members for each fragments.
The fragments are also link to each other with a description of the constraint.
Given those constraints and the SPARQL filter expression,
it is possible to select the fragments that can possibly contribute to the answer of the query.  
We apply a query paradigm called, Guided Link Traversal Query Processing (GLTQP) [](cite:cites verborgh2020, taelman2023)
a derivative of Link Traversal Query Processing (LTQP).
This querying technique consists of recursively looking up new documents by following links inside documents [](cite:cites Hartig2013AnOO).
GLTQP differ from LTQP by using knowledge capture from triples during the query execution to increase the selectivity
of data sources, by consequences diminishing the search domain.
