## Guided Link traversal for the TREE specification
{:#guided_link_traversal}

The objective of GLTQP is to reduce the search domain of a query engine by selecting data sources based on knowledge 
acquired before or during execution.
Our approach to GLTQP consists of restricting the links 
that the query engine can follow using a combination of first the structural properties of the TREE specification
and second the [hypermedia description](cite:cites thomasFieldingPhdThesis) 
of the constraint of the fragment.

More formally, this consists of creating a [reachability criteria](cite:cites hartig2012) 
that will be able to discriminate each triple encountered
inside a TREE document with the objective to decide if dereferencing the named nodes of
those triples, to find a new data source, will contribute to result from the query.
A conceptual example is presented at [](#running_example).

<figure id="running_example">
<img src="img/running_example.drawio.svg" alt="[Running example of our GLTQP approach]" class="figure-narrow" style="height: 20vh">
<figcaption markdown="block">
Running example of our GLTQP approach.
</figcaption>
</figure>


To determine if a named node will lead to a data source that is a fragment of the databases we need to consider
the structural aspect of the TREE specification.
Concretely, the fragment of the data set in the TREE specification 
is called [`tree:Node`](https://treecg.github.io/specification/#Node)s.
Those fragments contain hypermedia description of the next `tree:Node`s that can
be visited from that node.
Those descriptions are called [`tree:relation`](https://treecg.github.io/specification/#Relation).
An example of those triple sets using 
[turtle](https://www.w3.org/TR/turtle/) annotation is presented at [](#TREE-relation-turtle-example).
A `tree:relation` describe a `tree:Node` linked to the current node, via a named node with the predicate `tree:node`.
Dereferencing this named node and following its URL will lead to a new data sources that is a fragment of the
dataset. 

<figure id="TREE-relation-turtle-example" class="listing">
````/code/example_tree_relation.ttl````
<figcaption markdown="block">
The example is showing a set of triples representing a TREE relation. 
The relation indicates that the next node exists at the address https://exemple.be/nextNode,
all the members contain into the next node have the property `ex:publication_date` with
a value $$ \text{2023-01-07T00:00:00Z} $$.
The relation can be converted into the following boolean equation $$ x= unitTime(\text{2023-01-07T00:00:00Z}) $$ where $$ x $$ is any variable inside the client SPARQL query that as the predicate `ex:publication_date`.
</figcaption>
</figure>

The second part of the criteria is the analysis of the compatibility between the constraint
of the `tree:Node` and the SPARQL Filter expression. 
To do so, we need to dive deeper into the `tree:relation`, to understand how its constraint is formed.
The constraint of the `tree:relation`, can be conceptualized as a boolean equation.
This equation as a variable, a comparator and a literal,
we can exemplify it by the following simple expression; $$ x > 5 $$.
The left hand of the expression, the variable, is contingent to the SPARQL query expression.
It is the variable pertaining to the property that the `tree:relation` is targeting.
The property targeted by the relation is defined by the object of the triple
having the predicate `tree:path`.
In the example of [](#example-sparql) it was $$ ?t $$, because the example of [](#TREE-relation-turtle-example)
has the `tree:path` property `ex:storage_date`.
The operator, for its part, is described by the type of relation, the "`a`" in turtle representation.
The query engine needs to have a mapping between those types and simple boolean operators such as; equal,
greater than and so on.
Finally, the literal is simply represented by the object of the triple in the relation,
containing the predicate `tree:value`. 
Given,
the boolean representation of the `tree:relation` and the SPARQL filter expression that is already a boolean expression,
the query engine can evaluate if the combination of those two expressions is solvable, hence
discriminate if the relation should be followed if there are possible solutions to this boolean expression.