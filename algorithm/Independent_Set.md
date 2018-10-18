# Independent Set

[wiki link](https://en.wikipedia.org/wiki/Independent_set_(graph_theory)#Finding_independent_sets)

## Maximum Independent set

Problem Definition:

In the **maximum independent set** problem, the input is an undirected graph, and the output is a maximum independent set in the graph. If there are multiple maximum independent sets, only one need be output. This problem is sometimes referred to as "**vertex packing**".

This is a **NP** problem

https://en.wikipedia.org/wiki/Clique_problem

https://en.wikipedia.org/wiki/Independent_set_(graph_theory)



## Maximal Independent Set

Definition: a **maximal independent set** (MIS) or **maximal stable set** is an [independent set](https://en.wikipedia.org/wiki/Independent_set_(graph_theory)) that is not a subset of any other independent set.

### Find MIS:

This is P problem, which can be solved with a very simple greedy algorithm.

Given a Graph G(V,E), it is easy to find a single MIS using the following algorithm:

1. Initialize I to an empty set.
2. While V is not empty:
   - Choose a node v∈V;
   - Add v to the set I;
   - Remove from V the node v and all its neighbours.
3. Return I.

The runtime is O(*m*) since in the worst case as we have to check all edges.

O(m) is obviously the best possible runtime for a serial algorithm. But a parallel algorithm can solve the problem much faster.