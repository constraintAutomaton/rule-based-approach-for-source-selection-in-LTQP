Review 1
Overall evaluation	
-1: (weak reject)
This paper presents an approach for publishing and reliving RDF data as fragments based on TREE specification.
The authors suppose that many RDF fragments are published with links among them. In this case, client-side query time could be potentially higher because more HTTP lookups are needed.
To overcome this problem, the authors apply a query paradigm called Guided Link Traversal Query Processing (GLTQP).
This approach maps hypermedia-based constraints of those fragments to SPARQL FILTER expressions. Using this information, the client can only fetch those fragments that could contribute to query results.

In this paper, some example of the proposed approach based on TREE specification.
I could understand how to map their constraints to SPARQL FILTER expressions while it is not enough clear how it is technically novel.

Though the authors mention about open source implementation of the proposed approach, its details are not discussed.
In my understanding, the implementation works as client engine while I could not understand how SPARQL FILTER expressions are interpreted in it.

Miner comments:
Please state the source of the following references.
4. Verborgh, R., Taelman, R.: Guided Link-Traversal-Based Query Processing
6. Fielding, R.T.: Architectural Styles and the Design of Network-based Software
Architectures
Relevance to the Semantic Web conference	
1: (Average)
Novelty and originality	
1: (Average)
Clarity and quality of presentation	
0: (Needs Improvement)
Impact	
1: (Average)
Demo available online	
0: (No)
Code available?	
0: (No)


Review 2
Overall evaluation	
-1: (weak reject)
The paper describes an approach to improve

* In terms of presentation, could be improved in terms of language and clarity of exposition. Feels to have been quite rushed.
* Despite the above, it is possible to grasp the general idea of the approach, however, there is no evaluation and not even a hint on how such empirical evaluation could be done. Along these lines, there are a few technical questions:
- Reachability criterion should be more precise, in the paper is only exemplified
- From the example it seems the approach is only useful for queries with filters. Please clarify.
- What is the tradeoff between declaring the partitions with TREE and just using ASK queries before dereferencing?
* Considering that TREE hypermedia is in a very early effort, with a community group set up at end of March 2023 and invented by co-authors of the paper, I believe there should be some motivation about why we need Tree Hypermedia in general, or if it is being invented for this particular problem. As it reads now, the narrative is: TREE is there, let's use it to improve GLTQP.
Relevance to the Semantic Web conference	
1: (Average)
Novelty and originality	
1: (Average)
Clarity and quality of presentation	
0: (Needs Improvement)
Impact	
1: (Average)
Code available?	
0: (No)