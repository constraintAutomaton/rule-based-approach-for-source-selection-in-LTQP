## Introduction
{:#introduction}

[In the current years, the web of linked data has grew in size](https://lod-cloud.net/#diagram).
To access those data it is necessary to have an interface, the most common is the SPARQL endpoint.
When querying SPARQL endpoints, the interface takes the whole query load and delivers the results to the client,
which causes high workloads and is partially the reason that [historically they had a low
availability](cite:cites aranda2013).
Academics have made efforts to introduce linked data publication methods to make the client participate in the query execution,
with the aim to diminish the workloads of the server and still have fast query execution to the client [](cite:cites Verborgh2016TriplePF, Azzam2020, Azzam2021).
The TREE specification is an effort in that direction [](cite:cites ColpaertMaterializedTREE, lancker2021LDS, Delva2020, Delva2020a),
that introduces the concept of domain-related fragmentation of large RDF datasets.
This line of research aims to fragment what would be a data dump in a way that client can easily fetch a subset of it to answer their query fully.
The data inside a fragment are bounded with constraints that are expressed using hypermedia description [](cite:cites thomasFieldingPhdThesis).
More precisely inside a fragment there is a description of the constraint of the data of the other fragments accessible for this location
in web space.
Because there is no centralized index in those documents Link Traversal Query Processing (LTQP) [](cite:cites Hartig2016),
inspired techniques are used to answer queries. 
This consist in starting with a small set of URL that provided the first data sources and dereferencing recursively 
the IRI present inside the data source to obtain new data sources [](cite:cites Hartig2016). 
In the current state of affairs, it is not possible to use SPARQL to query those documents efficiently because there are no method, to
skip fragments that do not contain data pertaining to the answer to a query.
It result that custom solutions has been created to make use of this data.
The aim of this work is not to introduce the TREE specification but to propose a pushdown filter method 
[](cite:cites Hellerstein1998OptimizationTF, Yang2021FlexPushdownDBHP) with the use of  structural assumption [](cite:cites taelman2023)
to diminish the query execution time by using the SPARQL filter expression has link discrimant.

One idiomatic use case of fragmented dataset is the publication of sensor data, hence in this paper
we consider the example of [](#example-sparql) using the [DAHCC](cite:cites dahcc_resource) dataset.
We make queries to get the measure of a specific interval (the metavariable `{:filter_expression}`)
and information about the sensor using a metadata file that is available online at
[https://github.com/predict-idlab/DAHCC-Sources/blob/main/instantiated_examples/_Homelab.owl]() 
(we adapt the file by deleting the named graph and we use the metavariable`{:property}` to accommodate the dataset). 

<div class="sidebysidecontainer" style="align-items: stretch !important; ">
<figure id="example-sparql" class="listing" style="padding-right: 5px; padding-left: 5px; height='100%'">
````/code/example_sparql_query.ttl````
<figcaption markdown="block">
SPARQL query to get sensor measurement and the information about the sensor
</figcaption>
</figure>

<figure id="TREE-relation-turtle-example" class="listing" style="padding-right: 5px; padding-left: 5px">
````/code/example_tree_relation.ttl````
<figcaption markdown="block">
The example is showing a set of triples representing a TREE relation. 
The relation can be converted into the following boolean equation 
$$ x >= \text{2022-01-03T09:47:59.000000} $$ 
where $$ x $$ is any variable inside the client SPARQL query that as the predicate `etsi:hasTimestamp`.
</figcaption>
</figure>
</div>



The following section will present our approach and early evaluation.