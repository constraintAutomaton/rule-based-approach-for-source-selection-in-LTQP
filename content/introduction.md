## Introduction
{:#introduction}

Today, a large number of RDF documents is hosted on the web,
<del class="comment" data-author="RT">for some perspective in 2015 there was 88 billion Linked Data triples distributed
inside 9960 knowledge graphs</del> [](cite:cites Ermilov2013, Verborgh2016TriplePF).
One way to publish Linked Data is by hosting it into one RDF file,
which comes with a lot of advantages derived from the locality of the data.

{:.comment data-author="RT"}
Not sure what you mean by this advantage.
Perhaps we just want to say that this is simple for the publisher?

This locality make simple the update and query of part of the data set, because there is only one source of truth. 
But fragmentation also comes with a lot of advantages that can in some situation
outweighed the disadvantage induced by the distance between part of the data.
Before diving into the advantages,

{:.comment data-author="RT"}
No. Either say it immediately, or don't say it at all :-)
I propose to say something like "Instead of publishing datasets in a single file, they can be published in a fragmented manner, which is good for X, Y, and Z. Hereafter, we elaborate on these advantages."

we defined fragmentation has a methodology that divide data into substructures,
where this data is conceptually part of a single dataset.
[Triple Pattern Fragments](cite:cites Verborgh2016TriplePF) has shown how fragmentation of the data
on the server side can improve the query execution time. 

{:.comment data-author="RT"}
Yes, this is a good argument, let's start the paragraph with this one!

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

In this paper we consider the [TREE specification](cite:cites spec:tree), a Web specification that structures the publication of
RDF documents in a way that the link between the fragment of a dataset, where we call the fragments `tree:Node`,
can be described using set of triples called `tree:relation`.

{:.comment data-author="RT"}
Before you say "how" TREE works, first explain in a more higher level what it does. (see for example my changes in the abstract)

Those relations can be interpreted as boolean equations that constraints the value of a property of the data inside the next subject fragment.

{:.comment data-author="RT"}
Also explain it in a more higher level here (mapping TREE's hypermedia relations to SPARQL FILTER expressions).

We apply a technique called, Guided Link Traversal Query Processing (GLTQP) [](cite:cites verborgh2020, taelman2023)
a derivative of Link Traversal Query Processing (LTQP).

{:.comment data-author="RT"}
GLTQP and LTQP are not really techniques, they are more querying paradigms. Because they don't offer any concrete algorithms, but just ideas for them.

LTQP consist of recursively looking up new data sources by dereferencing the named node result and/or intermediary result
capture by the query engine [](cite:cites Hartig2013AnOO).

{:.comment data-author="RT"}
data sources -> documents on the Web
named node -> URLs
result and/or intermediary result: this is not really how LTQP works, see https://github.com/constraintAutomaton/comunica-filter-article/commit/323706a42e3dfbb798f643870cb1527a1dc15f33#diff-4ac2de86721eb586f80a770fe3d380e51b0438e7296d0f43bf48a7b6c4a9f090R53

The GLTQP introduces the usage of acquired knowledge from the triple captured to prune,
the irrelevant data source, hence diminishing the search domain. 

{:.comment data-author="RT"}
The sentence above is very confusing to read.

We propose to use this technique to leverage the information that the `tree:relation` provided to the query engine

{:.comment data-author="RT"}
Don't refer to `tree:relation`, but to what it represents.
Remember the decoupling of design and implementation we discussed recently.
Your approach can be generalized to other things next to just TREE.

by evaluating the solvability of the combination of the boolean equation of the `tree:relation` and the SPARQL filter expression

{:.comment data-author="RT"}
I would avoid using "evaluating the solvability", as it's not really clear what this means.

of a query. 
This can be particularly useful when for example querying sensor data,
where the fragmentation is made over the time of publication of the data.
Users might want to get a time slice of the dataset hence they will use a SPARQL `FILTER` operation
which will enable the use of GLTQP.   

{:.comment data-author="RT"}
Can we have a figure of such fragments illustrating this idea, together with a SPARQL query?
The example you make here is really important, but it's unfortunate that this comes so late.
I would propose starting very early with this example, before even talking about TREE. (TREE is just one way of achieving this)
