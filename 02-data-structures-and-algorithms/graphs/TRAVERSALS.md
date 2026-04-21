🟢 Graph Traversals: Methods, Patterns & Mastery
1. Overview of Graph Traversals
Graph traversals are systematic ways to visit every vertex and explore every edge exactly once (or at least once). They are fundamental to solving most graph problems.

Two Main Categories:

Depth-First Search (DFS): Explore deeply before backtracking (stack-based or recursion).

Breadth-First Search (BFS): Explore level-by-level (queue-based).

2. Depth-First Search (DFS) – Recursive
Use Cases:
Topological sort, cycle detection, connected components.

Backtracking, constraint satisfaction problems.

Strongly connected components (SCC).

Implementation
text
FUNCTION dfsRecursive(graph, start)
    visited = SET()
    result = []
    
    FUNCTION helper(node)
        visited.add(node)
        result.append(node)
        FOR neighbor IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                helper(neighbor)
            END IF
        END FOR
    END FUNCTION
    
    helper(start)
    RETURN result
END FUNCTION
Complexity:
Time: O(V + E) with adjacency list, O(V²) with adjacency matrix.

Space: O(V) for visited set + O(h) for recursion stack (h = max depth).

Pitfall: Stack overflow on very deep graphs; use iterative DFS instead.
3. DFS – Iterative with Stack
Implementation
text
FUNCTION dfsIterative(graph, start)
    visited = SET()
    stack = STACK()
    stack.push(start)
    result = []
    
    WHILE NOT stack.isEmpty() DO
        node = stack.pop()
        IF node NOT IN visited THEN
            visited.add(node)
            result.append(node)
            // Push neighbors in reverse order to maintain DFS order
            FOR neighbor IN REVERSE(graph.getNeighbors(node)) DO
                IF neighbor NOT IN visited THEN
                    stack.push(neighbor)
                END IF
            END FOR
        END IF
    END WHILE
    
    RETURN result
END FUNCTION
Advantage:
Avoids recursion stack overflow.

Explicit control over traversal order.

Complexity: Same as recursive (O(V + E) time, O(V) space).
4. Breadth-First Search (BFS) – with Queue
Use Cases:
Shortest path in unweighted graphs.

Level-order exploration, 0-1 BFS.

Social network distance (degrees of separation).

Implementation
text
FUNCTION bfs(graph, start)
    visited = SET()
    queue = QUEUE()
    queue.enqueue(start)
    visited.add(start)
    result = []
    
    WHILE NOT queue.isEmpty() DO
        node = queue.dequeue()
        result.append(node)
        FOR neighbor IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                visited.add(neighbor)
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    RETURN result
END FUNCTION
Complexity:
Time: O(V + E).

Space: O(V) for queue and visited set.

5. BFS with Level-Order Grouping
Use Case:
Find all nodes at each depth.

Shortest path with levels.

Implementation
text
FUNCTION bfsLevelOrder(graph, start)
    visited = SET()
    queue = QUEUE()
    queue.enqueue(start)
    visited.add(start)
    levels = []
    
    WHILE NOT queue.isEmpty() DO
        levelSize = queue.size()
        currentLevel = []
        FOR i = 1 TO levelSize DO
            node = queue.dequeue()
            currentLevel.append(node)
            FOR neighbor IN graph.getNeighbors(node) DO
                IF neighbor NOT IN visited THEN
                    visited.add(neighbor)
                    queue.enqueue(neighbor)
                END IF
            END FOR
        END FOR
        levels.append(currentLevel)
    END WHILE
    
    RETURN levels
END FUNCTION
6. Multi-Source BFS
Use Case:
Find shortest distance from any source to all vertices.

Rotten oranges problem, cell spreading.

Implementation
text
FUNCTION multiSourceBFS(graph, sources)
    visited = SET()
    queue = QUEUE()
    distances = {}
    
    FOR source IN sources DO
        queue.enqueue(source)
        visited.add(source)
        distances[source] = 0
    END FOR
    
    WHILE NOT queue.isEmpty() DO
        node = queue.dequeue()
        FOR neighbor IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                visited.add(neighbor)
                distances[neighbor] = distances[node] + 1
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    RETURN distances
END FUNCTION
7. All Paths DFS
Use Case:
Find all paths from source to destination.

Backtracking problems (N-queens, Sudoku).

Implementation
text
FUNCTION allPathsDFS(graph, start, end)
    paths = []
    path = [start]
    visited = SET()
    visited.add(start)
    
    FUNCTION dfs(node)
        IF node == end THEN
            paths.append(COPY(path))
            RETURN
        END IF
        FOR neighbor IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                visited.add(neighbor)
                path.append(neighbor)
                dfs(neighbor)
                path.pop()
                visited.remove(neighbor)
            END IF
        END FOR
    END FUNCTION
    
    dfs(start)
    RETURN paths
END FUNCTION
8. Bidirectional BFS
Use Case:
Shortest path search; faster than unidirectional BFS.

Reduced search space; meet in middle.

Implementation
text
FUNCTION bidirectionalBFS(graph, start, end)
    IF start == end THEN
        RETURN [start]
    END IF
    
    frontierStart = SET([start])
    frontierEnd = SET([end])
    visitedStart = SET([start])
    visitedEnd = SET([end])
    parentStart = {start: null}
    parentEnd = {end: null}
    
    WHILE NOT frontierStart.isEmpty() AND NOT frontierEnd.isEmpty() DO
        // Expand smaller frontier
        IF frontierStart.size() <= frontierEnd.size() THEN
            newFrontier = SET()
            FOR node IN frontierStart DO
                FOR neighbor IN graph.getNeighbors(node) DO
                    IF neighbor IN visitedEnd THEN
                        // Path found!
                        RETURN reconstructPath(parentStart, parentEnd, node, neighbor)
                    END IF
                    IF neighbor NOT IN visitedStart THEN
                        visitedStart.add(neighbor)
                        parentStart[neighbor] = node
                        newFrontier.add(neighbor)
                    END IF
                END FOR
            END FOR
            frontierStart = newFrontier
        ELSE
            // Expand end frontier (similar logic)
            // ...
        END IF
    END WHILE
    
    RETURN null  // No path
END FUNCTION
Complexity: O(b^(d/2)) vs O(b^d) for unidirectional (b = branching factor, d = distance).
9. Iterative Deepening DFS (IDDFS)
Use Case:
Memory-limited environments, depth-bounded search.

Combines BFS completeness with DFS space efficiency.

Implementation
text
FUNCTION iddfs(graph, start, target)
    FOR depth = 0 TO INFINITY DO
        visited = SET()
        IF dfsDepthLimited(graph, start, target, depth, visited) THEN
            RETURN true
        END IF
    END FOR
    RETURN false
END FUNCTION

FUNCTION dfsDepthLimited(graph, node, target, depth, visited)
    IF node == target THEN
        RETURN true
    END IF
    IF depth == 0 THEN
        RETURN false
    END IF
    visited.add(node)
    FOR neighbor IN graph.getNeighbors(node) DO
        IF neighbor NOT IN visited THEN
            IF dfsDepthLimited(graph, neighbor, target, depth-1, visited) THEN
                RETURN true
            END IF
        END IF
    END FOR
    RETURN false
END FUNCTION
Complexity: O(b^d) time (same as BFS), O(d) space (better than BFS).
10. 0-1 BFS
Use Case:
Shortest path with edge weights 0 or 1.

Faster than Dijkstra for this special case.

Implementation
text
FUNCTION zeroOneBFS(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    deque = DEQUE()
    deque.pushFront(start)
    
    WHILE NOT deque.isEmpty() DO
        node = deque.popFront()
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF weight == 0 THEN
                IF distances[node] + weight < distances[neighbor] THEN
                    distances[neighbor] = distances[node]
                    deque.pushFront(neighbor)
                END IF
            ELSE  // weight == 1
                IF distances[node] + weight < distances[neighbor] THEN
                    distances[neighbor] = distances[node] + 1
                    deque.pushBack(neighbor)
                END IF
            END IF
        END FOR
    END WHILE
    
    RETURN distances
END FUNCTION
Complexity: O(V + E) (faster than Dijkstra's O((V+E)logV)).
11. Traversal for Disconnected Graphs
Use Case:
Traverse all components, not just reachable from start.

Implementation
text
FUNCTION traverseAllComponents(graph, usesDFS = true)
    visited = SET()
    allResults = []
    
    FOR vertex IN graph.vertices DO
        IF vertex NOT IN visited THEN
            IF usesDFS THEN
                result = dfsRecursive(graph, vertex)
            ELSE
                result = bfs(graph, vertex)
            END IF
            allResults.append(result)
            visited.update(SET(result))
        END IF
    END FOR
    
    RETURN allResults
END FUNCTION
12. Traversal with Pre/Post Processing
Use Case:
Record discovery time, finish time (for SCC algorithms).

Track parent relationships (for tree edges, back edges, cross edges).

Implementation
text
FUNCTION dfsWithTimestamps(graph, start)
    visited = SET()
    discoveryTime = {}
    finishTime = {}
    parent = {}
    time = 0
    
    FUNCTION dfs(node, par)
        visited.add(node)
        time += 1
        discoveryTime[node] = time
        parent[node] = par
        
        FOR neighbor IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs(neighbor, node)
            ELSE IF neighbor != par THEN
                // Back edge detected (undirected); forward/cross (directed)
            END IF
        END FOR
        
        time += 1
        finishTime[node] = time
    END FUNCTION
    
    dfs(start, null)
    RETURN discoveryTime, finishTime, parent
END FUNCTION
13. Comparison of Traversals
Traversal	Data Structure	Time	Space	Best For
DFS Recursive	Recursion	O(V+E)	O(h)	Topological sort, SCC, simple code
DFS Iterative	Stack	O(V+E)	O(V)	Avoid stack overflow, explicit control
BFS	Queue	O(V+E)	O(V)	Shortest path (unweighted), level-order
Level-Order BFS	Queue	O(V+E)	O(w)	Find nodes by depth, layer analysis
Multi-Source BFS	Queue	O(V+E)	O(V)	Multi-source distance, flooding
Bidirectional BFS	2 Queues	O(b^(d/2))	O(V)	Find path between two nodes
IDDFS	Recursion	O(b^d)	O(d)	Memory-limited, depth-bounded
0-1 BFS	Deque	O(V+E)	O(V)	0-1 weights, faster than Dijkstra
14. Interview Patterns
Common Questions:

DFS/BFS Basics: All nodes reachable from start? (Google, Amazon, Facebook)

Topological Sort: Course schedule, dependency resolution. (Google, Microsoft)

Connected Components: Number of islands, network clusters. (All FAANG)

Cycle Detection: Detect cycles in directed/undirected. (Amazon, Facebook)

Shortest Path: BFS in unweighted graphs. (Google, Uber)

Bipartite Check: 2-coloring with BFS/DFS. (Microsoft, Facebook)

Interviewer Expectations:

Both recursive and iterative DFS.

BFS with and without level-order.

Time/space complexity analysis.

Edge case handling (disconnected, single vertex, empty graph).

15. Pitfalls and Best Practices
Common Mistakes:

Forgetting visited set: Infinite loops in cyclic graphs.

Recursive DFS on deep graph: Stack overflow; use iterative.

Wrong data structure: Queue for DFS instead of stack.

Not handling disconnected: Missing entire components.

Best Practices:

Always use visited set/array in cyclic graphs.

For large/deep graphs, prefer iterative over recursive.

Match data structure to traversal (stack for DFS, queue for BFS).

Test with disconnected graphs, single nodes, empty graphs.

Master graph traversals—they form the foundation for solving virtually every graph problem in interviews, system design, and real-world engineering!