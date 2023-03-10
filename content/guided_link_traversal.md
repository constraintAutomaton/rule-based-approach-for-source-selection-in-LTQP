## Guided Link traversal for the TREE specification
{:#guided_link_traversal}

The objective of GLTQP is to reduce the search domain of a query engine by selecting data sources based on knowledge 
acquired before or during execution.
Our approach to GLTQP consists of restricting the links 
that the query engine can follow using a combination of prior knowledge about the structural properties of the TREE specification,
and the [hypermedia descriptions](cite:cites thomasFieldingPhdThesis) of fragment constraints.

More formally, this consists of creating a [reachability criterion](cite:cites hartig2012) 
that can discriminate triples encountered
inside a TREE document with the objective to decide if dereferencing the URLs in
those triples will contribute to result from the query.
A conceptual example is presented at [](#running_example) based on the query of [](#example-sparql),
where we see in green that the query engine only considers the triples pertaining to
the relation between the fragment and follow the relation that is compatible with the SPARQL filter expression.


<figure id="running_example">
<img src="img/running_example.drawio.svg" alt="[Running example of our GLTQP approach]" class="figure-narrow" style="height: 20vh">
<figcaption markdown="block">
Running example of our GLTQP approach.
We see that the query engine consider the relation between the fragment separately from the other triples

{:.comment data-author="RE"} 
Is relation used above not plural?

and in green only consider the relation and fragment compatible with the SPARQL filter expression of
[](#example-sparql).
</figcaption>
</figure>


To determine if a URL will lead to another fragment in the current TREE dataset,
or another document elsewhere on the Web,
we need to consider the structural aspects of the TREE specification.
Concretely, fragments of TREE datasets
are called [`tree:Node`](https://treecg.github.io/specification/#Node)s.

{:.comment data-author="RE"} 
The s after tree:Node looks weird, not sure if it is intentional or not

Those fragments contain hypermedia descriptions of the next `tree:Node`s that are related to this node.

{:.comment data-author="RE"} 
Again, but this one doesn't link to anything, which I think is a good idea to keep doing

Those descriptions are called [`tree:relation`](https://treecg.github.io/specification/#Relation),
which contains a `tree:Node` that is linked to the current node via the predicate `tree:node`.
An example of the descriptions of such a TREE dataset is shown 
in [](#TREE-relation-turtle-example) using the [Turtle](https://www.w3.org/TR/turtle/) serialization.


The second part of our reachability criterion pertains to the compatibility between the constraint
of the `tree:Node` and the SPARQL Filter expression. 
The constraint of the `tree:relation`, can be conceptualized as a boolean equation.
This equation as a variable, a comparator and a literal,
we can exemplify it by the following simple expression; $$ x > 5 $$.
The left hand of the expression, the variable, is contingent to a SPARQL query expression.
It is the variable pertaining to the property that the `tree:relation` is targeting.
The property targeted by the relation is defined by the object of the triple
having the predicate `tree:path`.
In the example of [](#example-sparql) it was $$ ?t $$, because the example of [](#TREE-relation-turtle-example)
has the `tree:path` property `dct:created`. 
The boolean operator such as; equal, greater than, is described by the [RDF type of the TREE relation](https://treecg.github.io/specification/#Relation), in Turtle serialization the object with the predicate "`a`".
Finally, the literal is simply represented by the object of the triple in the relation,
containing the predicate `tree:value`.

{:.comment data-author="RE"} 
This part is a bit confusing to me, I know from your explanation what you're trying to do, but I don't think 
the text is very clear.. although I have no idea how to make it better 

Given,
the boolean representation of the `tree:relation` and the SPARQL filter expression that is already a boolean expression,
the query engine can evaluate if the combination of those two expressions is solvable, hence
discriminate if the relation should be followed if there are possible solutions to this boolean expression.