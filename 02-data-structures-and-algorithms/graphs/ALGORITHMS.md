🟢 Graph Algorithms: Comprehensive Guide to Solving Every Graph Problem
1. Shortest Path Algorithms
a. Dijkstra's Algorithm (Single-Source, Non-Negative Weights)
Use Cases:

GPS navigation, network routing, finding shortest path in networks.

Works when all edge weights ≥ 0.

Algorithm:

text
FUNCTION dijkstra(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    minHeap = MinHeap()
    minHeap.push((0, start))
    visited = SET()
    
    WHILE NOT minHeap.isEmpty() DO
        currDist, node = minHeap.pop()
        
        IF node IN visited THEN
            CONTINUE
        END IF
        visited.add(node)
        
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                newDist = currDist + weight
                IF newDist < distances[neighbor] THEN
                    distances[neighbor] = newDist
                    minHeap.push((newDist, neighbor))
                END IF
            END IF
        END FOR
    END WHILE
    
    RETURN distances
END FUNCTION
Complexity:

Time: O((V + E) log V) with min-heap, O(V²) with array.

Space: O(V).

Interview Tip: Implement with both heap and array versions.

b. Bellman-Ford Algorithm (Handles Negative Weights)
Use Cases:

Currency exchange arbitrage, detecting negative cycles.

Slower but flexible; handles negative weights.

Algorithm:

text
FUNCTION bellmanFord(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    // Relax edges V-1 times
    FOR i = 1 TO len(graph.vertices)-1 DO
        FOR u IN graph.vertices DO
            FOR v, weight IN graph.vertices[u] DO
                IF distances[u] != INFINITY AND 
                   distances[u] + weight < distances[v] THEN
                    distances[v] = distances[u] + weight
                END IF
            END FOR
        END FOR
    END FOR
    
    // Check for negative cycles
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.vertices[u] DO
            IF distances[u] != INFINITY AND 
               distances[u] + weight < distances[v] THEN
                RETURN "Negative cycle detected"
            END IF
        END FOR
    END FOR
    
    RETURN distances
END FUNCTION
Complexity:

Time: O(V * E) (slower than Dijkstra).

Space: O(V).

Advantage: Detects negative cycles.

c. Floyd-Warshall Algorithm (All-Pairs Shortest Paths)
Use Cases:

Compute all shortest paths; transitive closure.

Small graphs (V ≤ 500).

Algorithm:

text
FUNCTION floydWarshall(graph)
    n = len(graph.vertices)
    dist = MATRIX(n x n, INFINITY)
    vertices = LIST(graph.vertices.keys())
    
    FOR i = 0 TO n-1 DO
        dist[i][i] = 0
    END FOR
    
    FOR u IN graph.vertices DO
        u_idx = vertices.indexOf(u)
        FOR v, weight IN graph.vertices[u] DO
            v_idx = vertices.indexOf(v)
            dist[u_idx][v_idx] = weight
        END FOR
    END FOR
    
    FOR k = 0 TO n-1 DO
        FOR i = 0 TO n-1 DO
            FOR j = 0 TO n-1 DO
                dist[i][j] = MIN(dist[i][j], dist[i][k] + dist[k][j])
            END FOR
        END FOR
    END FOR
    
    RETURN dist
END FUNCTION
Complexity:

Time: O(V³).

Space: O(V²).

Use When: V small, need all-pairs, negative weights allowed.

d. A Search (Heuristic-Guided Shortest Path)*
Use Cases:

Game pathfinding with obstacles, GPS with real-time updates.

Faster than Dijkstra with good heuristic.

Key Idea:

text
f(node) = g(node) + h(node)
g(node) = actual distance from start
h(node) = heuristic estimate to goal
Algorithm:

text
FUNCTION aStar(graph, start, goal, heuristic)
    openSet = MinHeap()
    openSet.push((heuristic(start, goal), start))
    cameFrom = {}
    gScore = {}
    
    FOR v IN graph.vertices DO
        gScore[v] = INFINITY
    END FOR
    gScore[start] = 0
    
    closedSet = SET()
    
    WHILE NOT openSet.isEmpty() DO
        f, current = openSet.pop()
        
        IF current == goal THEN
            RETURN reconstructPath(cameFrom, current)
        END IF
        
        closedSet.add(current)
        
        FOR neighbor, weight IN graph.getNeighbors(current) DO
            IF neighbor IN closedSet THEN
                CONTINUE
            END IF
            
            tentativeG = gScore[current] + weight
            IF tentativeG < gScore[neighbor] THEN
                cameFrom[neighbor] = current
                gScore[neighbor] = tentativeG
                fScore = gScore[neighbor] + heuristic(neighbor, goal)
                openSet.push((fScore, neighbor))
            END IF
        END FOR
    END WHILE
    
    RETURN null
END FUNCTION
Complexity:

Time: O(E) with good heuristic, O((V+E)logV) worst-case.

Space: O(V).

Good Heuristics: Euclidean distance, Manhattan distance.

2. Minimum Spanning Tree (MST) Algorithms
a. Kruskal's Algorithm (Edge-Based, Union-Find)
Use Cases:

Network design, connecting cities with minimum cost.

Algorithm:

text
FUNCTION kruskal(graph)
    edges = []
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.vertices[u] DO
            edges.append((weight, u, v))
        END FOR
    END FOR
    
    SORT(edges BY weight)
    
    uf = UnionFind()
    FOR v IN graph.vertices DO
        uf.makeSet(v)
    END FOR
    
    mst = []
    FOR weight, u, v IN edges DO
        IF uf.find(u) != uf.find(v) THEN
            mst.append((u, v, weight))
            uf.union(u, v)
        END IF
    END FOR
    
    RETURN mst
END FUNCTION
Complexity:

Time: O(E log E) for sorting, O(E log V) with union-find.

Space: O(V + E).

Best For: Sparse graphs, naturally sorted edges.

b. Prim's Algorithm (Vertex-Based, Greedy)
Use Cases:

MST with priority queue, dense graphs.

Algorithm:

text
FUNCTION prim(graph, start)
    inMST = SET()
    key = {}
    parent = {}
    
    FOR v IN graph.vertices DO
        key[v] = INFINITY
        parent[v] = null
    END FOR
    
    key[start] = 0
    minHeap = MinHeap()
    minHeap.push((0, start))
    
    WHILE NOT minHeap.isEmpty() DO
        weight, u = minHeap.pop()
        
        IF u IN inMST THEN
            CONTINUE
        END IF
        inMST.add(u)
        
        FOR v, edgeWeight IN graph.getNeighbors(u) DO
            IF v NOT IN inMST AND edgeWeight < key[v] THEN
                key[v] = edgeWeight
                parent[v] = u
                minHeap.push((edgeWeight, v))
            END IF
        END FOR
    END WHILE
    
    mst = []
    FOR v IN graph.vertices DO
        IF parent[v] != null THEN
            mst.append((parent[v], v, key[v]))
        END IF
    END FOR
    
    RETURN mst
END FUNCTION
Complexity:

Time: O(E log V) with heap, O(V²) with array.

Space: O(V).

Best For: Dense graphs.

3. Topological Sort (DAG Only)
Use Cases:

Course prerequisites, build systems, dependency resolution.

Algorithm (DFS-based):

text
FUNCTION topologicalSort(graph)
    visited = SET()
    stack = STACK()
    
    FUNCTION dfs(node)
        visited.add(node)
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs(neighbor)
            END IF
        END FOR
        stack.push(node)
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            dfs(v)
        END IF
    END FOR
    
    result = []
    WHILE NOT stack.isEmpty() DO
        result.append(stack.pop())
    END WHILE
    
    RETURN result
END FUNCTION
Alternative (Kahn's Algorithm with In-Degree):

text
FUNCTION topologicalSortKahn(graph)
    inDegree = {}
    FOR v IN graph.vertices DO
        inDegree[v] = 0
    END FOR
    
    FOR u IN graph.vertices DO
        FOR v, _ IN graph.getNeighbors(u) DO
            inDegree[v] += 1
        END FOR
    END FOR
    
    queue = QUEUE()
    FOR v IN graph.vertices DO
        IF inDegree[v] == 0 THEN
            queue.enqueue(v)
        END IF
    END FOR
    
    result = []
    WHILE NOT queue.isEmpty() DO
        node = queue.dequeue()
        result.append(node)
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            inDegree[neighbor] -= 1
            IF inDegree[neighbor] == 0 THEN
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    IF len(result) != len(graph.vertices) THEN
        RETURN null  // Cycle exists
    END IF
    
    RETURN result
END FUNCTION
Complexity:

Time: O(V + E).

Space: O(V).

Interview: Both DFS and Kahn's; cycle detection bonus.

4. Cycle Detection
a. Undirected Graph (DFS)
text
FUNCTION hasCycleUndirected(graph)
    visited = SET()
    
    FUNCTION dfs(node, parent)
        visited.add(node)
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                IF dfs(neighbor, node) THEN
                    RETURN true
                END IF
            ELSE IF neighbor != parent THEN
                RETURN true  // Back edge found
            END IF
        END FOR
        RETURN false
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            IF dfs(v, null) THEN
                RETURN true
            END IF
        END IF
    END FOR
    
    RETURN false
END FUNCTION
b. Directed Graph (DFS with Colors)
text
FUNCTION hasCycleDirected(graph)
    // WHITE=0, GRAY=1, BLACK=2
    color = {}
    FOR v IN graph.vertices DO
        color[v] = 0
    END FOR
    
    FUNCTION dfs(node)
        color[node] = 1
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            IF color[neighbor] == 0 THEN
                IF dfs(neighbor) THEN
                    RETURN true
                END IF
            ELSE IF color[neighbor] == 1 THEN
                RETURN true  // Back edge
            END IF
        END FOR
        color[node] = 2
        RETURN false
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF color[v] == 0 THEN
            IF dfs(v) THEN
                RETURN true
            END IF
        END IF
    END FOR
    
    RETURN false
END FUNCTION
5. Strongly Connected Components (SCC)
a. Kosaraju's Algorithm
Algorithm:

text
FUNCTION kosaraju(graph)
    // Step 1: DFS on original, record finish order
    visited = SET()
    stack = STACK()
    
    FUNCTION dfs1(node)
        visited.add(node)
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs1(neighbor)
            END IF
        END FOR
        stack.push(node)
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            dfs1(v)
        END IF
    END FOR
    
    // Step 2: Create reversed graph
    reversed = Graph()
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.getNeighbors(u) DO
            reversed.addEdge(v, u, weight)
        END FOR
    END FOR
    
    // Step 3: DFS on reversed in stack order
    visited = SET()
    sccs = []
    
    FUNCTION dfs2(node, scc)
        visited.add(node)
        scc.append(node)
        FOR neighbor, _ IN reversed.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs2(neighbor, scc)
            END IF
        END FOR
    END FUNCTION
    
    WHILE NOT stack.isEmpty() DO
        node = stack.pop()
        IF node NOT IN visited THEN
            scc = []
            dfs2(node, scc)
            sccs.append(scc)
        END IF
    END WHILE
    
    RETURN sccs
END FUNCTION
Complexity: O(V + E).

b. Tarjan's Algorithm (Single Pass)
Complexity: O(V + E) with one DFS pass.

6. Connected Components
text
FUNCTION connectedComponents(graph)
    visited = SET()
    components = []
    
    FUNCTION dfs(node, component)
        visited.add(node)
        component.append(node)
        FOR neighbor, _ IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs(neighbor, component)
            END IF
        END FOR
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            component = []
            dfs(v, component)
            components.append(component)
        END IF
    END FOR
    
    RETURN components
END FUNCTION
Complexity: O(V + E).

7. Bipartite Check (2-Coloring)
text
FUNCTION isBipartite(graph)
    color = {}
    FOR v IN graph.vertices DO
        color[v] = -1
    END FOR
    
    FOR start IN graph.vertices DO
        IF color[start] == -1 THEN
            queue = QUEUE()
            queue.enqueue(start)
            color[start] = 0
            
            WHILE NOT queue.isEmpty() DO
                node = queue.dequeue()
                FOR neighbor, _ IN graph.getNeighbors(node) DO
                    IF color[neighbor] == -1 THEN
                        color[neighbor] = 1 - color[node]
                        queue.enqueue(neighbor)
                    ELSE IF color[neighbor] == color[node] THEN
                        RETURN false
                    END IF
                END FOR
            END WHILE
        END IF
    END FOR
    
    RETURN true
END FUNCTION
Complexity: O(V + E).

8. Maximum Flow / Ford-Fulkerson
text
FUNCTION fordFulkerson(graph, source, sink)
    residual = COPY(graph)
    maxFlow = 0
    
    WHILE true DO
        path = findAugmentingPath(residual, source, sink)
        IF path == null THEN
            BREAK
        END IF
        
        // Find min capacity
        minCap = INFINITY
        FOR i = 0 TO len(path)-2 DO
            u, v = path[i], path[i+1]
            minCap = MIN(minCap, getCapacity(residual, u, v))
        END FOR
        
        // Update residual
        FOR i = 0 TO len(path)-2 DO
            u, v = path[i], path[i+1]
            decreaseCapacity(residual, u, v, minCap)
            increaseCapacity(residual, v, u, minCap)
        END FOR
        
        maxFlow += minCap
    END WHILE
    
    RETURN maxFlow
END FUNCTION
Complexity: O(E * maxFlow) with DFS, O(VE²) with Edmonds-Karp.

9. Bipartite Matching (Hungarian / Hopcroft-Karp)
Use Cases: Job assignment, marriage problem, resource allocation.

Complexity: O(VE) for augmenting path, O(E√V) for Hopcroft-Karp.

10. Algorithm Comparison Table
Algorithm	Time	Space	Use Case	Constraint
Dijkstra	O((V+E)logV)	O(V)	Single-source shortest	Non-negative
Bellman-Ford	O(VE)	O(V)	Single-source shortest	Any weights, detect negative
Floyd-Warshall	O(V³)	O(V²)	All-pairs shortest	Small V
A*	O(E) with heuristic	O(V)	Heuristic pathfinding	Need heuristic
Kruskal	O(ElogE)	O(V+E)	MST	Sort edges
Prim	O(ElogV)	O(V)	MST	Dense graphs
Topo Sort	O(V+E)	O(V)	DAG ordering	No cycles
Kosaraju	O(V+E)	O(V)	SCC	Directed
BFS/DFS	O(V+E)	O(V)	Traversal	General
Max Flow	O(VE²)	O(V+E)	Flow network	Capacities
11. Advanced Algorithms
Heavy-Light Decomposition
Efficient tree queries and updates.

Time: O(log² n) per operation.

Centroid Decomposition
Divide-and-conquer on trees.

Solves many tree path/distance queries.

Link-Cut Trees
Dynamic tree operations.

O(log n) per operation.

Graph Neural Networks
Machine learning on graph structure.

State-of-the-art for many graph tasks.

12. Interview Patterns
Google: Dijkstra, BFS, topological sort.

Amazon: Connected components, cycle detection, MST.

Facebook: DFS/BFS, SCC, bipartite.

Microsoft: Topological sort, cycle detection, matching.

Apple: A*, game trees, pathfinding.

Master these algorithms to solve virtually any graph problem and excel in FAANG interviews!

