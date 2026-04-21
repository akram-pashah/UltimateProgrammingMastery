🟢 Graph Pitfalls: Common Bugs, Edge Cases & Defensive Coding
1. Forgetting to Mark Visited Nodes
Description:
Not tracking visited vertices in DFS/BFS, causing infinite loops in cyclic graphs.

Impact:
Infinite loop, program hang, timeout.

Prevention:
Always maintain a visited set/array before traversal; check before processing neighbors.

text
// Bug: No visited tracking
FUNCTION buggyDFS(node)
    process(node)
    FOR neighbor IN getNeighbors(node) DO
        buggyDFS(neighbor)  // Revisit same nodes infinitely
    END FOR
END FUNCTION

// Fixed: Track visited
FUNCTION correctDFS(node, visited)
    IF node IN visited THEN RETURN END IF
    visited.add(node)
    process(node)
    FOR neighbor IN getNeighbors(node) DO
        correctDFS(neighbor, visited)
    END IF
END FUNCTION
2. Null Pointer Dereference
Description:
Accessing neighbors/edges of null/non-existent vertex without checking.

Impact:
Crashes, undefined behavior.

Prevention:
Always validate vertex existence; use safe accessor methods.

3. Wrong Algorithm Choice
Description:
Using BFS for shortest path in weighted graph (only works for unweighted).
Using Dijkstra with negative weights (Bellman-Ford needed).

Impact:
Incorrect results, failed tests.

Prevention:
Understand algorithm preconditions; validate graph properties upfront.

text
// Bug: BFS on weighted graph
// Gives wrong shortest path if weights vary
shortestPath = bfs(weightedGraph, start, end)

// Fixed: Dijkstra for weighted non-negative
shortestPath = dijkstra(weightedGraph, start)
4. Cycle Detection Failures
Description:
Not properly detecting cycles in directed vs undirected graphs.
Forgetting parent pointer in undirected; not using colors in directed.

Impact:
Missed cycles, incorrect topological sort, infinite loops.

Prevention:
Use appropriate method: DFS+parent for undirected, DFS+colors for directed.

5. Off-by-One in Adjacency Matrix/List
Description:
Index errors when accessing vertices 0-indexed vs 1-indexed.

Impact:
Out of bounds, missing/wrong edges, crashes.

Prevention:
Consistent indexing; clearly document vertex numbering.

6. Disconnected Components Ignored
Description:
Traversing from single start vertex; missing other components.

Impact:
Incomplete results, wrong connected component count.

Prevention:
Iterate through all unvisited vertices; handle all components.

text
// Bug: Only explores from start
DFS(graph, startVertex)

// Fixed: Explore all components
FOR v IN graph.vertices DO
    IF v NOT IN visited THEN
        DFS(graph, v)
    END IF
END FOR
7. Adjacency Matrix/List Mismatch
Description:
Building adjacency list incorrectly; edges missing or duplicated.
Forgetting bidirectional edges in undirected graphs.

Impact:
Wrong algorithm results, missing paths.

Prevention:
Test on small graphs; verify edge counts match expectations.

8. Memory Leak in Adjacency Structures
Description:
Not properly freeing/clearing graph nodes in C/C++; orphaned nodes.

Impact:
Memory leak, crash on large graphs.

Prevention:
Explicit cleanup in destructors; nullify references.

9. Shortest Path on Wrong Input
Description:
Dijkstra on graph with negative weights; Bellman-Ford on billion-node graph.

Impact:
Wrong results (Dijkstra), timeout (Bellman-Ford on huge graph).

Prevention:
Validate input preconditions; choose algorithm for scale.

10. Incorrect Topological Sort
Description:
Not verifying result is valid; cycle present but not detected.
Using DFS on non-DAG accidentally; cyclic dependencies undetected.

Impact:
Invalid ordering, build failure, unresolved dependencies.

Prevention:
Validate result size = V; check no cycles before sorting.

text
// Bug: Assume input is DAG
topoSort = topologicalSort(graph)

// Fixed: Verify DAG and check output
IF NOT isDAG(graph) THEN
    ERROR "Graph has cycles"
END IF
topoSort = topologicalSort(graph)
IF len(topoSort) != V THEN
    ERROR "Cycles detected"
END IF
11. Stack Overflow from Deep Recursion
Description:
DFS on very deep graph (trees, skewed graphs) exhausts call stack.

Impact:
Stack overflow, crash on large inputs.

Prevention:
Use iterative DFS with explicit stack; limit recursion depth.

12. Self-Loop and Duplicate Edge Handling
Description:
Not handling self-loops (u→u) or multiple edges between same vertices.

Impact:
Wrong degree count, broken algorithms.

Prevention:
Decide upfront: allow/disallow; test with self-loops explicitly.

13. Empty or Single-Vertex Graph Edge Cases
Description:
Not testing empty graph, single vertex, or single edge.

Impact:
Crashes, index errors on edge cases.

Prevention:
Always test: empty, one node, two nodes, disconnected.

14. Forgetting to Initialize Distances/Weights
Description:
Not setting initial distances to INFINITY before Dijkstra/Bellman-Ford.
Uninitialized arrays causing garbage values.

Impact:
Wrong shortest paths, incorrect comparisons.

Prevention:
Explicitly initialize all distances/weights before algorithm.

15. Parent Pointer Inconsistency
Description:
Not updating parent pointers during traversal; reconstruction fails.

Impact:
Cannot rebuild path; "no parent" when parent exists.

Prevention:
Update parent at same time as visited; reconstruct test.

16. Visited Set Not Cleared Between Queries
Description:
Reusing graph for multiple queries; visited set not cleared.

Impact:
Second query sees stale visited data; wrong results.

Prevention:
Clear visited before each query; use local visited per call.

text
// Bug: Shared state
visited = SET()  // Global
FOR each query DO
    result = DFS(graph, query, visited)  // Stale data

// Fixed: Local per query
FOR each query DO
    visited = SET()  // Fresh
    result = DFS(graph, query, visited)
END FOR
17. Infinite Loop in Undirected Graphs
Description:
Parent check not done; revisit parent immediately in undirected graph.

Impact:
Cycle detected erroneously; false positives.

Prevention:
Track parent; skip when neighbor == parent (undirected DFS).

18. Not Handling Disconnected Nodes in MST
Description:
Assuming graph connected; MST algorithm partial for disconnected.

Impact:
Incomplete spanning forest; wrong total weight.

Prevention:
Check connectivity first; MST returns forest for disconnected.

19. Modifying Graph During Traversal
Description:
Adding/removing edges while iterating; invalidates traversal.

Impact:
Skipped edges, revisited nodes, incorrect results.

Prevention:
Don't modify during traversal; collect changes, apply after.

20. Forgetting Bidirectional Edges in Undirected Graphs
Description:
Adding u→v but not v→u in adjacency list.

Impact:
Asymmetric graph; algorithms fail.

Prevention:
Add both directions in addEdge for undirected.

text
// Bug: Only one direction
graph.addEdge(u, v)  // u→v only

// Fixed: Bidirectional
graph.addEdge(u, v)
graph.addEdge(v, u)
21. Interview-Specific Pitfalls
Empty result handling: Return empty list, not null (unless specified).

Off-by-one in indices: 0-based vs 1-based numbering.

Forgetting to return root: In graph clone, return cloned root, not original.

Edge case dismissal: Don't ignore single vertex, disconnected graphs.

22. Common Mistakes by Algorithm
Dijkstra:

Negative weights → use Bellman-Ford.

Not initializing distances to ∞.

Processing same node twice.

BFS:

Forgetting to mark visited when enqueueing (not dequeuing).

Using stack instead of queue.

DFS:

Stack overflow on deep graphs → use iterative.

Forgetting parent for undirected cycle detection.

Topological Sort:

Assuming input is DAG.

Cycle present but undetected.

MST:

Using on disconnected graph without handling forest.

Comparing wrong edges.

23. Best Practices
Always test with: empty, single node, two nodes, disconnected, cyclic.

Use visited set/array consistently.

Match algorithm to problem (Dijkstra non-negative, Bellman negative, etc.).

Handle edge cases explicitly.

Validate preconditions upfront.

Profile on large inputs; watch for timeouts.

Master these pitfalls to write production-grade graph code, acing interviews and avoiding subtle bugs that waste days of debugging!