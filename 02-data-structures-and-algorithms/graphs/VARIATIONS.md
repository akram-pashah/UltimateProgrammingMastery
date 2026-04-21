🟢 Graph Variations: Types, Representations & Real-World Applications
1. Graph Type Variations
a. Undirected Graphs
Definition: Edges have no direction; connection is bidirectional.

Representation: If u-v exists, both u→v and v→u present.

Properties:

Symmetric adjacency (matrix is symmetric).

Degree counts all incident edges.

Use Cases: Social networks (friendships), roads, internet backbone.

Algorithms: DFS/BFS work naturally; may visit nodes twice (optimization needed).

b. Directed Graphs (Digraphs)
Definition: Edges have direction; u→v means one-way connection.

Properties:

In-degree vs out-degree distinction.

Not necessarily symmetric.

Use Cases: Web links, citation networks, task dependencies, workflows.

Algorithms: DFS for SCC, topological sort, cycle detection.

c. Weighted Graphs
Definition: Edges carry numeric values (distance, cost, capacity, probability).

Properties:

Weights can be positive, negative, or mixed.

Algorithms must consider weights (e.g., Dijkstra vs BFS for shortest path).

Use Cases: GPS navigation, network routing, flight pricing, recommendation confidence.

Algorithms: Dijkstra (non-negative), Bellman-Ford (negative), Floyd-Warshall (all-pairs).

d. Unweighted Graphs
Definition: All edges treated equally (weight = 1).

Simplification: BFS sufficient for shortest path.

Use Cases: Social networks (degree separation), game move exploration.

e. Cyclic vs Acyclic
Cyclic: Contains at least one cycle.

Acyclic (DAG): No cycles; linear dependencies; every vertex has topological order.

Trees: Special acyclic connected graphs (n vertices, n-1 edges).

Forests: Multiple disconnected trees.

f. Connected vs Disconnected
Connected: At least one path between every pair of vertices.

Disconnected: Multiple connected components; some vertices unreachable from others.

Strongly Connected (directed): Every vertex reachable from every other.

Weakly Connected (directed): Connected if treated as undirected.

g. Complete Graph
Definition: Every vertex connected to every other; maximum edges.

Properties: Kn (n vertices), edges = n*(n-1)/2 (undirected), n*(n-1) (directed).

Use Cases: Baseline for comparison, theoretical models.

h. Bipartite Graphs
Definition: Vertices partitionable into two independent sets; edges only between sets.

2-Colorable: Vertices can be 2-colored such that no adjacent vertices share color.

Detection: BFS/DFS with 2-coloring.

Use Cases: Job assignments (workers ↔ jobs), user-item recommendations, matching problems.

i. Sparse vs Dense
Sparse: Few edges relative to possible (E << V²/2).

Adjacency list preferred (space-efficient, O(V+E)).

Examples: Social networks (each user ~100 friends, not billions).

Dense: Many edges (E ≈ V²/2).

Adjacency matrix acceptable (O(V²) but fast lookup).

Examples: Complete graphs, collaboration networks.

j. Multigraph
Definition: Multiple edges between same vertex pair allowed.

Use Cases: Multiple flights between cities, multiple transactions between accounts.

Handling: Store edge counts or lists of edges.

k. Hypergraph
Definition: Edges connect arbitrary number of vertices (not just 2).

Use Cases: Modeling complex relationships (e.g., papers with multiple authors).

Complexity: Harder to analyze; often reduced to standard graphs.

2. Graph Representation Variations
a. Adjacency Matrix
text
Space: O(V²)
Matrix[i][j] = weight or 1/0
Pros: O(1) edge lookup, dense graphs
Cons: O(V²) always, wasteful for sparse
Traversal: O(V²) with DFS/BFS
b. Adjacency List
text
Space: O(V + E)
Each vertex stores neighbors
Pros: Space-efficient sparse, O(V+E) traversal
Cons: O(V) worst-case edge lookup
Most practical: Used in 90% of implementations
c. Edge List
text
Space: O(E)
List of (u, v, weight) tuples
Pros: Very sparse graphs (barely used)
Cons: Slow edge/neighbor lookup; must scan all edges
Use: Kruskal's MST (edges pre-sorted)
d. Incidence Matrix
text
Space: O(V * E)
V×E matrix; entry = 1 if vertex incident to edge
Rarely used: Too much space
e. Compressed Sparse Row (CSR) Format
text
Optimized for sparse graphs, machine learning
Three arrays: values, column_index, row_pointer
Space: O(V + E)
Used in: Graph neural networks, scientific computing
f. Implicit Representation
text
Graph defined by function, not stored explicitly
Example: Puzzle states (no storage, generate neighbors on-demand)
Used in: Game trees, state space search
3. Specialized Graph Structures
a. Tree
Definition: Connected, acyclic graph (n vertices, n-1 edges).

Properties: Unique path between any two vertices.

Variants: Binary trees, N-ary trees, trees with weights (networks).

Used for: File systems, hierarchies, DOM, BST, decision trees.

b. Forest
Definition: Multiple disjoint trees; acyclic but not connected.

Used for: Representing multiple independent hierarchies, union-find structure.

c. DAG (Directed Acyclic Graph)
Definition: Directed, no cycles; linear partial order on vertices.

Properties: Topological sort exists and is unique.

Use Cases: Build dependencies, class hierarchies, compiler data flow.

Algorithms: Topological sort, critical path analysis.

d. Complete Graph (Kn)
Definition: All vertices connected to all others.

Edges: n*(n-1)/2 (undirected).

Diameter: 1 (max distance between any two vertices).

Use Cases: Baseline, theoretical models.

e. Bipartite Matching Graph
Definition: Special bipartite graph used for assignment problems.

Use Cases: Job-worker matching, hospital-resident matching.

Algorithm: Hungarian algorithm, maximum bipartite matching (max flow).

f. Planar Graph
Definition: Drawable on plane without edge crossings.

Euler's Formula: V - E + F = 2 (V vertices, E edges, F faces).

Maximum Edges: E ≤ 3V - 6.

Use Cases: Geographic routing, circuit design.

g. Interval Graph
Definition: Vertices are intervals on real line; edges connect overlapping intervals.

Properties: Chordal (special structure enabling efficient algorithms).

Use Cases: Scheduling problems, activity selection.

h. Flow Network
Definition: Directed weighted graph with source, sink, capacities.

Properties: Flow conservation (input = output at intermediate nodes).

Algorithms: Ford-Fulkerson, Dinic, push-relabel.

Use Cases: Network flow, bipartite matching, airline seat allocation.

i. Weighted Directed Acyclic Graph (WDAG)
Definition: DAG with edge weights for cost/distance.

Use Cases: Shortest paths, critical path method (project management).

Algorithms: Topological sort + relaxation (O(V+E) for single-source).

j. Knowledge Graph
Definition: Semantic graph with entities (vertices) and relationships (edges).

Use Cases: Google Knowledge Graph, Wikidata, enterprise knowledge bases.

Features: Labeled edges, attributes on vertices.

4. Graph Coloring Variants
a. Vertex Coloring
Definition: Assign colors to vertices; adjacent vertices have different colors.

Chromatic Number: Minimum colors needed.

NP-Hard: No polynomial algorithm (3-coloring is NP-complete).

Use Cases: Register allocation (compilers), exam scheduling, map coloring.

b. Edge Coloring
Definition: Assign colors to edges; incident edges have different colors.

Chromatic Index: Minimum colors needed.

Vizing's Theorem: Chromatic index ≤ max degree + 1.

c. List Coloring
Definition: Each vertex has list of allowed colors; assign from list.

List Chromatic Number: Minimum list size needed.

5. Special Vertex/Edge Properties
a. Weighted Vertex (Vertex Labeling)
Definition: Vertices carry values/labels (not just identity).

Use Cases: Cities (population, GDP), atoms (atomic number), tasks (duration).

b. Signed Graphs
Definition: Edges marked as positive (+1) or negative (-1).

Properties: Balance theory (structural stability).

Use Cases: Social networks (trust/distrust), sentiment analysis.

c. Temporal Graphs
Definition: Edges exist only at certain times; dynamic topology.

Use Cases: Communication networks, transportation schedules, epidemic spread.

Challenge: Complex algorithms; snapshot or streaming models.

d. Attributed Graphs
Definition: Vertices/edges carry additional attributes (not just identity/weight).

Use Cases: Social networks (profiles), knowledge graphs (relations), heterogeneous networks.

e. Heterogeneous Graphs
Definition: Multiple vertex types and/or edge types.

Examples: User-movie-genre network, paper-author-venue network.

Algorithms: Meta-path based, heterogeneous graph neural networks.

6. Language and Library Implementations
Python
networkx: Most comprehensive graph library; all structures supported.

igraph: Fast graph algorithms; vertex/edge attributes.

graph-tool: High-performance; C++ backend.

Custom: dict of lists for adjacency list.

Java
JGraphT: Feature-rich, multiple graph types.

Guava: Google's library; graph utilities.

Custom: HashMap<V, List<V>> for adjacency list.

C++
Boost Graph Library: Comprehensive, template-based.

SNAP: Stanford Network Analysis Platform.

Custom: vector<vector<pair<int,int>>> for weighted adjacency list.

C#
QuickGraph: Graph algorithms, visualization.

Custom: Dictionary<V, List<V>> or List<Edge>.

7. Graph Algorithm Variants
a. BFS Variants
0-1 BFS: Weights only 0 or 1; deque instead of queue.

Multi-source BFS: Start from multiple sources simultaneously.

Bidirectional BFS: Search from both ends; meet in middle.

b. DFS Variants
Iterative DFS: Use explicit stack (avoid recursion overhead).

Post-order DFS: Visit node after children (useful for deletion, postfix).

Topological Sort via DFS: Record finish times, reverse.

c. Shortest Path Variants
Dijkstra: Single-source, non-negative weights, greedy.

Bellman-Ford: Single-source, negative weights allowed, DP-based.

Floyd-Warshall: All-pairs, cubic but handles negative weights.

A Search:* Shortest path with heuristic (faster than Dijkstra with good heuristic).

0-1 BFS: Weights 0 or 1 only.

d. MST Variants
Kruskal: Greedy edge-based; union-find.

Prim: Greedy vertex-based; priority queue.

Borůvka: Divide-and-conquer; used in distributed systems.

e. SCC Variants
Kosaraju: Two-pass DFS; simpler.

Tarjan: Single-pass DFS; slightly complex.

8. Advanced Specialized Graphs
a. Graph Neural Network (GNN) Graphs
Representation: Node features + graph structure.

Used in: Deep learning on graphs, molecule generation, recommendation systems.

Libraries: PyTorch Geometric, DGL (Deep Graph Library).

b. Causal Graphs
Definition: Directed edges represent causal relationships.

Used in: Causal inference, epidemiology, economics.

Analysis: Structural causal models (SCM).

c. Bayesian Networks
Definition: DAG where edges represent probabilistic dependencies.

Used in: Probabilistic inference, decision-making under uncertainty.

d. Markov Networks
Definition: Undirected graphs representing conditional dependencies.

Used in: Machine learning, image processing (graphical models).

e. Petri Nets
Definition: Bipartite graph (places, transitions) for concurrent systems modeling.

Used in: Workflow systems, distributed systems, formal verification.

f. Ontology Graphs
Definition: Semantic knowledge representation (entities, relations, hierarchy).

Used in: Knowledge bases, AI systems, semantic web.

9. Choosing the Right Variant
Based on Use Case:

Scenario	Variant	Why
Social network	Undirected, sparse	Mutual friendships, many users
Navigation	Directed, weighted	One-way roads, distance matters
Dependencies	DAG	No circular dependencies
Bipartite matching	Bipartite	Two distinct entity types
Sparse large graph	Adjacency list	Space efficiency
Dense small graph	Adjacency matrix	Fast edge lookup
Dynamic topology	Temporal graph	Time-varying structure
Recommendation	Attributed bipartite	User-item-feature
10. Common Pitfalls When Choosing Variants
Using matrix for sparse graph: Wastes O(V²) space.

Using list for dense graph: Slower traversal than matrix.

Ignoring negative weights: Dijkstra fails; use Bellman-Ford.

DAG assumption wrong: Cycle present; algorithm breaks.

Not handling disconnected components: Partial results.

Multigraph treated as simple: Loses information, incorrect count.

Master graph variants to select optimal structures for your problem, scale efficiently, and solve complex real-world challenges with precision and elegance!