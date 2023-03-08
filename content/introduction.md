## Introduction
{:#introduction}

Today, there is a large number of RDF documents hosted on the web,
for some perspective in 2015 there was 88 billion Linked Data triples distributed
inside 9960 knowledge graphs [](cite:cites Ermilov2013, Verborgh2016TriplePF).
A naive way to host linked data online is by hosting it into one file,
because it comes with a lot of advantages derived from the locality of the data.
This locality make simple the update and query of part of the data set, because there is only one source of truth. 
But fragmentation also comes with a lot of advantages that can in some situation
outweighed the disadvantage induced by the distance between part of the data.
Before diving into the advantages, we defined fragmentation has a methodology that divide data into substructures,
where this data is conceptually part of a single dataset.
[Triple Pattern Fragments](cite:cites Verborgh2016TriplePF) has shown how fragmentation of the data
on the server side can improve the query execution time. 
But
fragmentation also has other advantages such as that the client does not have to download the whole dataset to access
the information needed and the server does not have to create and maintain an interface 
to send the precise data needed by the client[](cite:cites ColpaertMaterializedTREE, lancker2021LDS).
Another advantage, related to the previous one is that by fragmenting
the data in an adequate way in relation to a specific objective and such as the query engine can understand the fragmentation,
the client will not need to implement a complex algorithm
to process the information, considering that it tries to accomplish and objective related to the one, the data publisher had in mind.
Then, the client will simply need to query the relevant fragment without having to do a lot of processing.
A concrete example of such application is [client side auto completion](https://tree.linkeddatafragments.org/demo/autocompletion/)
and client-side route planning [](cite:cites Delva2020, Delva2020a).

In this paper we consider the [TREE specification](cite:cites spec:tree), a web specification that structure the publication of
RDF documents in a way that the link between the fragment of a dataset, where we call the fragments `tree:Node`,
can be described using set of triples called `tree:relation`.
Those relations can be interpreted as boolean equations that constraints the value of a property of the data inside the next subject fragment.
We apply a technique called, Guided Link Traversal Query Processing (GLTQP) [](cite:cites verborgh2020, taelman2023)
a derivative of Link Traversal Query Processing (LTQP).
LTQP  consist of recursively looking up new data sources by dereferencing the named node result and/or intermediary result
capture by the query engine [](cite:cites Hartig2013AnOO).
The guided variance introduces the usage of acquired knowledge from the triple captured to prune,
the irrelevant data source, hence diminishing the search domain. 
We propose to use this technique to leverage the information that the `tree:relation` provided to the query engine
by evaluating the solvability of the combination of the boolean equation of the `tree:relation` and the SPARQL filter expression
of a query. 
This can be particularly useful when querying sensor data,
where the fragmentation is made over the time of publication of the data.
Users might want to get a time slice of the dataset hence they will use a SPARQL Filter operation
which will enable the use of GLTQP.   