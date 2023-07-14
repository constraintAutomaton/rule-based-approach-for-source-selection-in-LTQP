## Introduction
{:#introduction}

[In the current years, the web of linked data has grown in size](https://lod-cloud.net/#diagram).
To access those data it is necessary to have an interface, the most common is the SPARQL endpoint.
When querying SPARQL endpoints, the interface takes the whole query load and delivers the results to the client,
which causes high workloads and is partially the reason that [historically they had a low
availability](cite:cites aranda2013).
Academics have made efforts to introduce linked data publication methods to make the client participate in the query execution,
with the aim to diminish the workloads of the server and still have fast query execution to the client [](cite:cites Verborgh2016TriplePF, Azzam2021).
The TREE specification is an effort in that direction [](cite:cites ColpaertMaterializedTREE, lancker2021LDS),
that introduces the concept of domain-related fragmentation of large RDF datasets.<span class="comment" data-author="RT">Could you give a brief example to readers can imagine what this means? Just a single short sentence should be sufficient</span>
TREE aims to fragment datasets in a way that enables clients to easily fetch a subset of it to answer their query fully.
The data inside a fragment are bounded with constraints that are expressed using hypermedia descriptions [](cite:cites thomasFieldingPhdThesis).
More precisely, each fragment declaratively describes the constraints of the data it contains, and links to other fragments.
Since TREE fragments are hyperlinked Linked Data documents,
clients must traverse over these documents to find data,
which makes Link Traversal Query Processing (LTQP) [](cite:cites Hartig2016) a suitable technique for answering SPARQL queries over it.

LTQP typically starts with a set of seed URLs that are dereferenced.
From these dereferenced documents, links to other documents are dereferenced recursively.
So far, [applications on top of TREE datasets](cite:cites ColpaertMaterializedTREE, lancker2021LDS)
have resorted to custom traversal implementations to find data matching custom data needs.
Our aim in this work is to explore how to execute generic SPARQL queries over TREE datasets through LTQP.
We do not aim to extend the existing TREE specification,
but merely to introduce LTQP-specific link pruning techniques that exploit the structural properties of TREE,
similar to the [exploitation of structural properties when performing LTQP over the Solid environment](cite:cites taelman2023).
Specifically, we will make use of the SPARQL FILTER expression to prune links that match the constraints of TREE fragments.

As a running example throughout this paper, we consider the publication of sensor data.
For example, the query in [](#example-sparql) targets the [DAHCC](cite:cites dahcc_resource) dataset.
We make queries to get the measure of a specific interval (the metavariable `{:filter_expression}`) <span class="comment" data-author="RT">Can we just fill in one example value of this filter expression?</span>
and information about the sensor using a metadata file that is available online at
[https://github.com/predict-idlab/DAHCC-Sources/blob/main/instantiated_examples/_Homelab.owl]() 
(we adapt the file by deleting the named graph and we use the metavariable`{:property}` to accommodate the dataset). 

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
<span class="comment" data-author="RT">This listing comment contains too many details. Just explain the constraint, nothing more.</span>
The example is showing a set of triples representing a TREE relation. <span class="comment" data-author="RT">Relations have not been mentioned in the text yet. Constraints on the other hand have. Make sure to use consistent terminology.</span>
The relation can be converted into the following boolean equation <span class="comment" data-author="RT">Boolean equations are also new.</span> 
$$ x >= \text{2022-01-03T09:47:59.000000} $$ 
where $$ x $$ is any variable inside the client SPARQL query that as the predicate `etsi:hasTimestamp`.
</figcaption>
</figure>
</div>

<span class="comment" data-author="RT">The second listing is never mentioned in the text.</span>

<span class="comment" data-author="RT">If I remember correctly, you were planning a demo. As such, explicitly say that this is a demo. You'll also need a dedicated section explaining what you will show during the demonstration. You can find inspiration in my older papers.</span>


<span class="comment" data-author="RT">The sentence below can go.</span>
The following section will present our approach and early evaluation.