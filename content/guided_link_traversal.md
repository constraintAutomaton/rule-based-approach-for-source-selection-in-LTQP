## Guided Link traversal for the TREE specification
{:#solver_description}

To optimize the discovery of members represented in TREE documents during query execution, we apply the concepts of guided link traversal.
Our source of knowledge is the `tree:relation`,
the query engine can capture those triples and interpret it into a boolean equation.
Given the SPARQL filter expression (which is already a boolean equation)
the query engine can check the satisfiability of the combination of two expressions and use the result as a discriminant pertaining to the link to follow.
It comes then that when we traverse the graph links,
we ignore the links in which it would be impossible to find elements that would respect the query,
reducing our reachability condition and the search domain and adding a more adequate stop criterion.


Our current implementation uses a boolean satisfiability solver as a discriminator mechanism. The solver evaluates if the solution domain is empty or not.
Currently,
we only support numbers,
boolean and [date time](https://www.w3.org/TR/2004/REC-xmlschema-2-20041028/#dt-dateTime),
which are converted into Unix time to interpret them as a number.
[SPARQL function](https://www.w3.org/TR/sparql11-query/#SparqlOps) are also not supported.

{:.comment data-author="RT"}
In the explanation above, you're mixing method (high-level) and implementation (low-level) together in one, which makes things very confusing.
I would suggest first starting with a very high-level description of your approach, without talking about details such as `tree:relation` and boolean equations.
Then, once the reader understands the high-level, you can zoom in on the details.

In this section,
We will first formalize the TREE specification,
explain the new reachability criterion,
analyze the search domain of our solution,
then we will explain the working of the solver.

### Preliminary

#### TREE collection formalization 

We defined the TREE specification this way; all the members inside a collection $$ C = \{ \{m_1 \cdots m_{nz} \}, V \} $$,
where $$ n_z $$ is the total number of members inside the collection and $$ V $$ is the set of views.

The `tree:view` is a superset containing all the `tree:Node`s of a specific fragmentation, we defined the views as $$ V_i = \{ NO_1 \cdots NO_n \} $$.
Where there is $$ n $$ `tree:Node` $$ NO_i $$ containing $$ n_i $$ members and $$ n_r $$ relations.
A relation is a link between $$ NO_i $$ and $$ NO_j $$ defined as $$ r_{ij}: M  \rightarrow \{ true, false \} \in R_i $$,
where $$ R_i $$ is the set of all the relations contained inside $$ NO_i $$.
It come then that every `tree:Node` inside $$ V_l $$ can be defined as 
$$ NO_{i} = \{ m_{z} \in C : r_{ji} (m_z) \wedge  m_z \notin NO_{k} \forall k<i, R_i \} $$, where $$ NO_{k} \in V_l $$.

#### Reachability criterion
{:#reachability_criterion}


{:.comment data-author="RT"}
Since you build on the reachability semantics, it's important that you explain those semantics (formally) here, with the proper reference where you got it.

In a naive traversal approach of a TREE document,
in the context of querying its members,
it is trivial to not use $$ c_{all} $$ (following all the links) from the semantic defined by [](cite:cites hartig2012),

{:.comment data-author="RT"}
Not sure what you mean with the above.

as we know from the specification that new members will be found inside the `tree:Node`
which are accessed by interpreting the triple set that define the `tree:relation`
more precisely by dereferencing the object in the triples where the predicate is the named node `tree:node`. 
we defined $$ tp = \{ v_1, v_2, v_3 \} $$ as the current triple,
$$ \mathfrak{N} $$ as the set of all the named nodes and  $$ B_r \in \mathfrak{B} $$ as the set of all the blank node defining the relations, 
given that $$ \mathfrak{B} $$ is the set of all the blank nodes.

{:.comment data-author="RT"}
I guess we'll have to reuse the existing SPARQL semantics for this, from the paper I sent to you recently (and also cite that one).
You can see an example of how this can be done in my paper (section 5.1).

We define our reachability criterion for finding members in TREE datasets as follows:

$$c_{TREE}(tp)  = \begin{cases}
true & \text{if} \quad v_2 = \text{tree:node} | (v_1, v_2, v_3) \land v_3 \in \mathfrak{N} \land v_1 \in B_r \\
false & \text{else} \\
\end{cases}$$

{:.comment data-author="RT"}
I'm not sure that the above is correct.
There is no relationship between tp and your v's.
(This is why I think the reachability semantics are not expressive enough to capture this, while the subweb formalization is, also see 5.1 in my paper)

With our approach of guided link traversal, we can define again a new reachability criterion,
called $$ c_{TREEr}$$, where we again follow the dereference link in relation to the `tree:node` predicate, 
but on the condition that the SPARQL filter expression combined with a boolean expression derived from the triples defining the `tree:relation`.
In the absence of a condition than we simply use $$ c_{TREE} $$.
We can define this reachability criterion this way,
where $$ i $$ is the index of the current `tree:Node`,
$$ j $$ is the index of the potential following `tree:Node`,


$$ c_{TREEr}(tp, i,j)  = \begin{cases}
true & \text{if} \quad v_2 = \text{tree:node} | (v_1, v_2, v_3) \land v_3 \in \mathfrak{N} \land v_1 \in B_r \land ( dom(r_{ij} \land R) \neq \emptyset \lor \nexists r_{ij}) \\
false & \text{else} \\
\end{cases} $$

{:.comment data-author="RT"}
The above is also not precise enough.
The filter F has no relation to tp.
Also, reachability criteria don't accept i and j parameters.

#### Search domain

Contrary to the traditional LTQP problem, the search domain of the TREE specification is finite. 
From the point of view of discovering relevant data sources, it is the `tree:Node` of a `tree:view` that are the search domain,
but from the point of view of finding the fitting triples in relation to the query,
then it is the members inside those `tree:node`s that represent the search domain. 
We can define the search domain as the superset $$ S_{TREE} = \{ M \in NO_i \} $$, where the elements are the set of members inside each `tree:Node`.
Which means that the size of the domain is the finite cardinality  $$ n_{TREE} =  \| V_i\| $$ at the query time $$ t $$.
Using the criterion $$ c_{TREEr} $$, the search domain is a subset of the domain with $$ c_{TREE} $$,
defined as $$ S_{TREEr} = \{ M \in NO_i: dom(R \land r_{ij}) \neq \emptyset \}  \subseteq S_{TREE} \Rightarrow n_{TREEs} \leq n_{TREE}$$.

{:.comment data-author="RT"}
I don't really understand why you need this search domain formalization. Isn't this exactly what the reachability criterion enables?

#### Stop criterion

{:.comment data-author="RT"}
Is the stop criterion really necessary to define here?
I guess it can be derived implicitly from the finite search space implied by the reachability criterion.
But I guess you could talk about it as a consequence of this finite search space.

To stop the search there is a multiple criterion possible in the context of the TREE specification,
for example a limit of triple result using the `LIMIT` SPARQL parameter in the query can be defined,
the query engine can also implement a timeout, to stop the search after a certain duration and
the most trivial one is to stop the search when we traveled the whole `tree:view`.
With our method we can stop the query processing when there is no next `tree:Node` 
candidate, from every $$ NO_i $$ inside the link queue where, we can define it as for every $$ NO_i $$  $$ dom (r_{ij} \land R) = \emptyset \forall j $$. 
