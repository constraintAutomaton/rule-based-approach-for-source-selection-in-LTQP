## Guided Link traversal for the TREE specification
{:#guided_link_traversal}

The objective of GLTQP is to reduce the search domain of a query engine by selecting data sources based on knowledge 
acquired before or during execution.
Our approach to GLTQP consists of restricting the links 
that the query engine can follow using a combination prior knowledge about the structural properties of the TREE specification,
and the [hypermedia descriptions](cite:cites thomasFieldingPhdThesis) of fragment constraints.

More formally, this consists of creating a [reachability criterion](cite:cites hartig2012) 
that can discriminate each triple encountered
inside a TREE document with the objective to decide if dereferencing the URLs in
those triples will contribute to result from the query.
A conceptual example is presented at [](#running_example).

{:.comment data-author="RT"}
Ok, the example is good, but make sure to explain it briefly as well.
Something like: "if we have a query X, then we only need to follow these fragments."

<figure id="running_example">
<img src="img/running_example.drawio.svg" alt="[Running example of our GLTQP approach]" class="figure-narrow" style="height: 20vh">
<figcaption markdown="block">
Running example of our GLTQP approach.

{:.comment data-author="RT"}
Explain what is shown in this figure. What does green/white mean?

{:.comment data-author="RT"}
You could make this a lot more compact, to handle the page limit.
</figcaption>
</figure>


To determine if a URL will lead to another fragment in the current TREE dataset,
or another document elsewhere on the Web,
we need to consider the structural aspects of the TREE specification.
Concretely, fragments of TREE datasets
are called [`tree:Node`](https://treecg.github.io/specification/#Node)s.
Those fragments contain hypermedia descriptions of the next `tree:Node`s that are related to this node.
Those descriptions are called [`tree:relation`](https://treecg.github.io/specification/#Relation),
which contains a `tree:Node` that is linked to the current node via the predicate `tree:node`.
An example of the descriptions of such a TREE dataset is shown 
in [](#TREE-relation-turtle-example) using the [Turtle](https://www.w3.org/TR/turtle/) serialization.

<figure id="TREE-relation-turtle-example" class="listing">
````/code/example_tree_relation.ttl````
<figcaption markdown="block">
These triples represent a relation inside a TREE dataset. 
The relation indicates that the next fragment exists at the address https://example.be/nextNode,
and that all members inside that fragment have the property `ex:publication_date` with
value $$ \text{2023-01-07T00:00:00Z} $$.

{:.comment data-author="RT"}
What you write below should be in the text, not in the caption.

The relation can be converted into the following boolean equation $$ x= unitTime(\text{2023-01-07T00:00:00Z}) $$ where $$ x $$ is any variable inside the client SPARQL query that as the predicate `ex:publication_date`.
</figcaption>
</figure>

The second part of our reachability criterion pertains to the compatibility between the constraint
of the `tree:Node` and the SPARQL Filter expression. 

To do so, we need to dive deeper into the `tree:relation`, to understand how its constraint is formed.

{:.comment data-author="RT"}
The sentence above should not be needed. Just explain it :-)

The constraint of the `tree:relation`, can be conceptualized as a boolean equation.
This equation as a variable, a comparator and a literal,
we can exemplify it by the following simple expression; $$ x > 5 $$.
The left hand of the expression, the variable, is contingent to a SPARQL query expression.
It is the variable pertaining to the property that the `tree:relation` is targeting.
The property targeted by the relation is defined by the object of the triple
having the predicate `tree:path`.
In the example of [](#example-sparql) it was $$ ?t $$, because the example of [](#TREE-relation-turtle-example)
has the `tree:path` property `ex:storage_date`.

{:.comment data-author="RT"}
Instead of giving two examples, can we just start using this SPARQL example immediately?
It's also quite simple, and will save you some space.

The operator is described by the RDF type of TREE relation.
The query engine needs to have a mapping between those types and simple boolean operators such as; equal,
greater than and so on.

{:.comment data-author="RT"}
What is the operator here?

{:.comment data-author="RT"}
First time you talk about the query engine here.
I would avoid talking about it here, and instead keep it at the level of reachability criterion.

Finally, the literal is simply represented by the object of the triple in the relation,
containing the predicate `tree:value`. 
Given,
the boolean representation of the `tree:relation` and the SPARQL filter expression that is already a boolean expression,
the query engine can evaluate if the combination of those two expressions is solvable, hence
discriminate if the relation should be followed if there are possible solutions to this boolean expression.