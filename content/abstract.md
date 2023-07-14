## Abstract
<!-- Context      -->
Linked data can be used to model all sorts of objects, which can be queried to power applications.
In some use cases like sensor data publication, the size of the data dump can become quite large.
The most popular way to query RDF data is a server-side SPARQL endpoint.
This query strategy at the current time has caused a large problem of unavailability of data.
<!-- Need         -->
Alternative strategies are to create a tradeoff between the client and 
the server by fragmenting and annotating with hypermedia descriptions the dataset.
In recent years domain-related fragmentation strategies (by date, by value and so on) 
have been created where the client has to use Link Traversal Query Processing to acces the data.
At the current time, there are no generic SPARQL ways to query the data. 
<!-- Task         -->
We propose to use a filter pushdown strategy with the SPARQL query language to prune the links that are not relevant to the query result.
<!-- Object       -->
This paper presents our strategy and early experimental results.
<!-- Findings     -->
We found that by using this strategy we have been able to drastically 
reduce the query execution time and the number of HTTP requests
needed to answer time-related queries over sensor data.
<!-- Conclusion   -->
Given the positive result of early effort, we will in the future extend our investigation with  more complex filter expressions
and more datasets and compare our results with other SPARQL interfaces.


