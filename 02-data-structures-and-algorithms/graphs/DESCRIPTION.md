🟢 Graphs: Comprehensive Mastery from Foundations to Advanced Systems
1. What is a Graph?
A graph is a non-linear data structure consisting of:

Vertices (Nodes): Discrete objects or entities.

Edges: Connections between pairs of vertices.

Formal Definition: G = (V, E) where V = set of vertices, E = set of edges.

2. Foundational Concepts
a. Vertex and Edge Terminology
Adjacent Vertices: Connected by a direct edge.

Incident Edge: Edge connected to a vertex.

Degree: Number of edges connected to a vertex.

In-degree: Incoming edges (directed).

Out-degree: Outgoing edges (directed).

Path: Sequence of vertices connected by edges.

Cycle: Path that starts and ends at same vertex.

Connected Component: Subgraph where all vertices are connected.

b. Graph Properties
Order: Number of vertices (|V|).

Size: Number of edges (|E|).

Density: Ratio of edges to possible edges.

Dense: Many edges (close to complete graph).

Sparse: Few edges (trees, forests).

3. Types of Graphs
a. Undirected Graphs
Edges have no direction; connection is bidirectional.

Example: Social networks (friendship), road networks.

Max edges: n*(n-1)/2.

b. Directed Graphs (Digraphs)
Edges have direction; one-way connections.

Example: Web links, citation networks, workflows.

Max edges: n*(n-1).

c. Weighted Graphs
Edges carry weights (costs, distances, capacities).

Example: GPS routes (distance), flight booking (price).

Used in shortest path, minimum spanning tree algorithms.

d. Unweighted Graphs
Edges have no weights; all connections equally important.

e. Cyclic vs Acyclic
Cyclic: Contains at least one cycle.

Acyclic: No cycles (DAG = Directed Acyclic Graph, Tree).

f. Complete Graph
Every vertex connected to every other vertex.

Kn denotes complete graph with n vertices.

g. Bipartite Graph
Vertices partitioned into two disjoint sets; edges only between sets.

No edges within same set.

Example: Job applicants ↔ Job positions.

h. Multigraph
Multiple edges between same pair of vertices allowed.

Example: Multiple flights between two cities.

4. Graph Representations
a. Adjacency Matrix
text
Space: O(V²)
Representation: V×V 2D array, matrix[i][j] = weight or 1/0
Pros: O(1) edge lookup, dense graphs
Cons: O(V²) space, heavy for sparse graphs
b. Adjacency List
text
Space: O(V + E)
Representation: Array of lists; each vertex stores its neighbors
Pros: Space-efficient for sparse graphs, fast iteration
Cons: O(V) worst-case edge lookup
c. Edge List
text
Space: O(E)
Representation: List of (u, v, weight) tuples
Pros: Space-efficient for very sparse graphs
Cons: Slow edge lookup
d. Incidence Matrix
text
Space: O(V * E)
Representation: V×E matrix, entry = 1 if vertex incident to edge
Cons: Rarely used; space inefficient
5. Core Graph Algorithms
a. Traversal Algorithms
Depth-First Search (DFS):

Explores deeply before backtracking.

Time: O(V + E), Space: O(V).

Use: Topological sort, cycle detection, connected components.

Breadth-First Search (BFS):

Explores level-by-level.

Time: O(V + E), Space: O(V).

Use: Shortest path (unweighted), level-order problems.

b. Shortest Path Algorithms
Dijkstra's Algorithm:

Single-source shortest path; works with non-negative weights.

Time: O((V + E) log V) with min-heap, O(V²) with array.

Use: GPS navigation, network routing.

Bellman-Ford Algorithm:

Single-source shortest path; handles negative weights.

Detects negative cycles.

Time: O(V * E), slower but robust.

Floyd-Warshall Algorithm:

All-pairs shortest paths.

Time: O(V³), Space: O(V²).

Use: Small graphs, transitive closure.

A Search:*

Shortest path with heuristic guidance.

Time: O(E) with good heuristic.

Use: Game AI, pathfinding with obstacles.

c. Minimum Spanning Tree (MST)
Kruskal's Algorithm:

Greedy; sort edges by weight, add if no cycle.

Time: O(E log E) or O(E log V).

Uses: Union-Find data structure.

Prim's Algorithm:

Greedy; grow tree from starting vertex.

Time: O(E log V) with heap, O(V²) with array.

Use: Network design, clustering.

d. Topological Sort
Linear ordering of DAG vertices; u before v if edge u→v.

Time: O(V + E) with DFS.

Use: Dependency resolution, task scheduling, compilation.

e. Strongly Connected Components (SCC)
Kosaraju's Algorithm:

Two DFS passes; Time: O(V + E).

Tarjan's Algorithm:

Single DFS with stack; Time: O(V + E).

Use: Social network analysis, compiler design.

f. Maximum Flow / Min Cut
Ford-Fulkerson Algorithm:

Finds maximum flow in network.

Time: O(E * max_flow) with DFS, O(VE²) with Edmonds-Karp.

Use: Network flow, bipartite matching.

Dinic's Algorithm:

Optimized max flow; Time: O(V²E).

6. Advanced Graph Structures
a. Trees (Special Graphs)
Acyclic connected graphs.

Subclass of graphs; n vertices, n-1 edges.

Used: File systems, databases, DOM.

b. Forests
Collection of disjoint trees.

c. DAG (Directed Acyclic Graph)
No cycles; used for dependency graphs.

Example: Build systems, task scheduling.

d. Bipartite Graphs
Two-coloring possible; vertices partitionable into independent sets.

Used: Matching problems, recommendation systems.

e. Planar Graphs
Drawable on plane without edge crossings.

Euler's formula: V - E + F = 2.

f. Graph Coloring
Assign colors to vertices; no adjacent vertices same color.

Chromatic number: minimum colors needed.

NP-hard; used in scheduling, register allocation.

7. Key Patterns and Interview Problems
a. Search and Traversal
Number of islands, connected components (DFS/BFS).

Surrounded regions (BFS/DFS flood fill).

b. Shortest Path
Dijkstra, Bellman-Ford, A* (Google, Amazon, Uber).

Network delay time, path with minimum effort.

c. Topological Sort
Course schedule, alien dictionary (Google, Facebook).

Build order, task dependencies.

d. Cycle Detection
Undirected: Union-Find or DFS.

Directed: DFS with colors or Tarjan.

e. Bipartite Check
2-coloring (BFS/DFS).

Is graph bipartite? (Facebook, Microsoft).

f. Graph Coloring
Assign minimum colors; NP-hard in general.

g. Matching Problems
Maximum bipartite matching (Ford-Fulkerson, Hungarian).

Job assignment, network flow.

h. Cliques and Independent Sets
NP-hard; approximation or brute force for small graphs.

8. Memory, Performance, and Complexity
Adjacency Matrix
Space: O(V²) always.

Time: O(1) edge lookup, O(V) neighbors.

Best for: Dense graphs, complete graphs.

Adjacency List
Space: O(V + E) with unweighted, O(V + 2E) with weights.

Time: O(degree) neighbor lookup.

Best for: Sparse graphs, most practical uses.

Traversal Complexity
DFS/BFS: O(V + E) for adjacency list, O(V²) for matrix.

Algorithm Complexity Summary
Algorithm	Time	Space	Notes
DFS/BFS	O(V+E)	O(V)	Linear traversal
Dijkstra	O((V+E)logV)	O(V)	Positive weights
Bellman-Ford	O(VE)	O(V)	Negative weights
Floyd-Warshall	O(V³)	O(V²)	All pairs
Kruskal	O(ElogE)	O(V)	MST
Prim	O(ElogV)	O(V)	MST
Topological Sort	O(V+E)	O(V)	DAG only
SCC (Kosaraju)	O(V+E)	O(V)	Directed
Max Flow	O(VE²)	O(V+E)	Edmonds-Karp
9. Real-World Applications
Social Networks
Friendship graphs, recommendation systems (BFS, graph coloring).

Shortest path to connect users.

Navigation and Routing
GPS (Dijkstra), road networks (shortest paths).

Multi-modal routing (flights, trains, buses).

Web and Search Engines
Page rank algorithm (Google) – random walk on web graph.

Web crawling, link analysis.

Recommendation Systems
User-item graphs (Netflix, Amazon).

Graph neural networks for collaborative filtering.

Compiler and Build Systems
Dependency graphs (topological sort).

Cyclic dependency detection.

Network Flow
Airline networks, bandwidth allocation.

Maximum flow in data centers.

Bioinformatics
Protein interaction networks.

Genomic sequence analysis.

Game Development
Pathfinding, NPC AI (A* search).

Game tree analysis (minimax).

Supply Chain and Logistics
Warehouse networks, delivery optimization.

Vehicle routing, facility location.

10. FAANG and Top Company Patterns
Google: Page rank, BFS/DFS, Dijkstra, topological sort.

Amazon: Shortest path, delivery optimization, bipartite matching.

Facebook: Social graph, connected components, cycle detection.

Apple: Game trees, A* search, path planning.

Microsoft: Topological sort, cycle detection, graph coloring.

LinkedIn: Shortest path, network analysis, SCC.

11. Common Pitfalls
Null Pointer Errors: Uninitialized adjacency lists, null vertices.

Cycle Detection Failures: Forgetting to mark visited nodes.

Infinite Loops: Not handling all vertices in traversal.

Stack Overflow: Deep DFS without iterative approach.

Off-by-One: Index errors in adjacency matrix.

Memory Leaks: Not cleaning up graph data structures.

Wrong Algorithm Choice: Using matrix on sparse graph (wasteful space).

12. Best Practices
Choose representation based on graph density (sparse → list, dense → matrix).

Always handle disconnected components and isolated vertices.

Use iterative traversal for deep graphs (avoid stack overflow).

Preprocess: Compute connected components, topological order, SCC upfront.

Validate graph properties before running algorithms.

Test with edge cases: single vertex, disconnected graph, cycles, self-loops.

13. Language Implementations
Python: networkx library, custom classes with dictionaries.

Java: Custom classes, HashMap for adjacency list.

C++: vector of vectors for adjacency list, std::map for weights.

C#: List<List<int>> for adjacency, Dictionary for weights.

14. Advanced Topics Not to Miss
Graph Neural Networks (GNN): Deep learning on graphs; used in recommendation systems.

Heavy-Light Decomposition: Advanced path queries on trees.

Centroid Decomposition: Divide-and-conquer on trees.

Link-Cut Trees: Dynamic tree operations.

2-SAT: Boolean satisfiability using graph algorithms.

Graph Isomorphism: Checking if two graphs are equivalent (NP-hard).

Planar Graph Algorithms: Special algorithms for planar graphs.

Random Walks: Markov chains on graphs (PageRank).

Community Detection: Finding clusters in social networks.

15. Summary and Learning Outcomes
Master graph fundamentals: vertices, edges, adjacency, degree.

Understand all graph representations and choose appropriately.

Implement and master: DFS, BFS, Dijkstra, topological sort, MST, SCC.

Recognize real-world applications across domains.

Solve FAANG interview problems confidently.

Avoid common pitfalls; write robust, efficient graph code.

Know when to apply advanced techniques (GNN, centroid decomposition, etc.).

Graphs are the fabric of modern systems—from social networks and search engines to logistics and AI. Master them to unlock career-defining engineering skills and FAANG success!

