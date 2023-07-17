## Introduction
{:#introduction}

[The increasing amount of available Linked Data on the Web](https://lod-cloud.net/#diagram),
prompts the need for efficient query interfaces, with SPARQL endpoints as the most prominent one.
During a typical SPARQL query execution, the interface takes the whole query load and delivers the results to the client.
This may cause high workloads and is partially the reason for having [historically low
availability levels](cite:cites aranda2013).
Academics have made efforts to introduce alternative Linked Data publication methods
that enable clients participating in the query execution process.
The goal is to lower server-side workloads and while keeping fast query execution to the client [](cite:cites Azzam2021).
The TREE hypermedia specification is an effort in that direction [](cite:cites ColpaertMaterializedTREE, lancker2021LDS),
that introduces the concept of domain-related fragmentation of large RDF datasets.
For example, in the case of periodic measurements of sensor data,
the fragmentation can be made based on the publication date of each data entity.
TREE aims to fragment datasets in a way that enables clients to easily fetch subsets that fully answer a given query.
The data inside a fragment are bounded with constraints that are expressed using hypermedia descriptions [](cite:cites thomasFieldingPhdThesis).
More precisely, each fragment declaratively describes the constraints of the data it contains, and links to other fragments.
Since TREE fragments are hyperlinked Linked Data documents,
clients must traverse over these documents to find data,
which makes Link Traversal Query Processing (LTQP) [](cite:cites Hartig2016) a suitable technique for answering SPARQL queries over it.

LTQP typically starts with a set of seed URLs that are dereferenced.
From these dereferenced documents, links to other documents are dereferenced recursively.
So far, [applications on top of TREE datasets](cite:cites ColpaertMaterializedTREE, lancker2021LDS)
have resorted to custom traversal implementations to find data matching specific data needs.
Our aim in this work is to explore how to execute generic SPARQL queries over TREE datasets through LTQP.
We do not aim to extend the existing TREE specification,
but merely to introduce LTQP-specific link pruning techniques that exploit the structural properties of TREE,
similar to the [exploitation of structural properties when performing LTQP over the Solid environment](cite:cites taelman2023).
Specifically, we will make use of the SPARQL FILTER expression to prune links that match the constraints of TREE fragments.

As a running example throughout this paper, we consider the publication of sensor data.
For example, the query in [](#example-sparql) targets the [DAHCC](cite:cites dahcc_resource) dataset.
We have created queries to get the measures between a specific time interval (the filter expression will vary in our experiment) 
and information about the sensor using a metadata file, which is available at
[https://github.com/predict-idlab/DAHCC-Sources/blob/main/instantiated_examples/_Homelab.owl](https://github.com/predict-idlab/DAHCC-Sources/blob/main/instantiated_examples/_Homelab.owl) 


<div class="sidebysidecontainer" style="align-items: stretch !important; ">
<figure id="example-sparql" class="listing" style="padding-right: 5px; padding-left: 5px; height='100%'">
````/code/example_sparql_query.ttl````
<figcaption markdown="block">
SPARQL query to get sensor measurements and metadata
</figcaption>
</figure>

<figure id="TREE-relation-turtle-example" class="listing" style="padding-right: 5px; padding-left: 5px">
````/code/example_tree_relation.ttl````
<figcaption markdown="block">
Hypermedia description of a TREE fragment `ex:nextNode` containing resources having the property `etsi:hasTimestamp`
respecting the constraint $$ ?t>= \text{2022-01-03T09:47:59.000000} $$.
</figcaption>
</figure>
</div>
