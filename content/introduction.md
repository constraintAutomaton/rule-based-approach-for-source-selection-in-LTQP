## Introduction
{:#introduction}

Today, a large number of RDF documents is hosted on the Web [](cite:cites Ermilov2013, Verborgh2016TriplePF).
One way to publish Linked Data is by hosting it into one RDF file,
which is simple for the data publisher.
The data publisher can also publish its data in a fragmented manner,
which can be beneficial for query execution time,
as demonstrated by [Triple Pattern Fragments](cite:cites Verborgh2016TriplePF).
The fragmentation also helps the users by giving them the possibility to fetch only the relevant data subsets [](cite:cites ColpaertMaterializedTREE, lancker2021LDS). 
It also helps client-side applications to enable functionalities by having the data pre-processed and stored into fragments,
so that the client has to perform less effort during [auto-completion](https://tree.linkeddatafragments.org/demo/autocompletion/)
and client-side route planning [](cite:cites Delva2020, Delva2020a). 
The disadvantage of fragmenting such datasets is that there are multiple sources of truth that the user
has to query to find the information needed, which means more processing power is needed on the client side. 

{:.comment data-author="RT"}
Sources of truth is the wrong argument here, because the source of truth still exists in one source, it's just fragmented now.
Instead, we can say that data is just spread over multiple sources.
Also, this will not lead to a need for more processing power, but instead, it requires more HTTP lookups. (it may even lead to less client-side processing!)

In the scope of this paper, we consider the following use case;
a user want to determine the temperature of all units that have been stored before or at the first of January 2023,
which can be expressed in SPARQL as shown in [](#example-sparql).

<figure id="example-sparql" class="listing">
````/code/example_sparql_query.ttl````
<figcaption markdown="block">
SPARQL query to get the temperature and the associated unit IRI.
</figcaption>
</figure>

{:.comment data-author="RT"}
Can we make use of real ontologies in this example query?

{:.comment data-author="RT"}
What is "unit" here exactly? Is it a cooler or something?
We need to be more precise here.

As there many units, the data provider decides
to fragment the dataset using the [TREE specification](cite:cites spec:tree).
TREE enables data publisher to place a boolean constraint on a property of the data inside each fragment.

{:.comment data-author="RT"}
"property of the data"
Do you mean a property on members inside the dataset?

Furthermore, these fragments linked to each other with a hypermedia description of the constraint.

Given these hypermedia-based constraints,
it is therefore possible for a client-side query engine
to interpret them and use them during SPARQL query execution.
Furthermore, the engine could map them to SPARQL FILTER expressions,
and only fetch those fragments that could contribute to query results.

One approach to achieve this, is to apply a query paradigm called
[Guided Link Traversal Query Processing (GLTQP)](cite:cites verborgh2020, taelman2023)
a specific form of [Link Traversal Query Processing (LTQP)](cite:cites Hartig2013AnOO).
The idea of LTQP consists of recursively looking up new documents
by following links inside them.
GLTQP goes a step further,
by making use of a priori knowledge pertaining to the structure of Linked Data documents to reduce the search domain,
which may be discovered during link traversal
