🟢 Pseudocode: Graphs – Core Operations to Advanced Algorithms
1. Graph Node and Edge Structures
text
CLASS GraphNode
    value
    neighbors = []  // For adjacency list representation
END CLASS

CLASS Edge
    source
    destination
    weight
END CLASS

CLASS Graph
    vertices = {}      // Map: vertex -> adjacency list
    isDirected = false
    
    FUNCTION addVertex(v)
        IF v NOT IN vertices THEN
            vertices[v] = []
        END IF
    END FUNCTION
    
    FUNCTION addEdge(u, v, weight = 1)
        addVertex(u)
        addVertex(v)
        vertices[u].append((v, weight))
        IF NOT isDirected THEN
            vertices[v].append((u, weight))
        END IF
    END FUNCTION
END CLASS
2. Depth-First Search (DFS) – Recursive
text
FUNCTION dfsRecursive(graph, start)
    visited = SET()
    result = []
    
    FUNCTION helper(node)
        visited.add(node)
        result.append(node)
        FOR neighbor, weight IN graph.vertices[node] DO
            IF neighbor NOT IN visited THEN
                helper(neighbor)
            END IF
        END FOR
    END FUNCTION
    
    helper(start)
    RETURN result
END FUNCTION
3. DFS – Iterative with Stack
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
            FOR neighbor, weight IN graph.vertices[node] DO
                IF neighbor NOT IN visited THEN
                    stack.push(neighbor)
                END IF
            END FOR
        END IF
    END WHILE
    
    RETURN result
END FUNCTION
4. Breadth-First Search (BFS) – with Queue
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
        FOR neighbor, weight IN graph.vertices[node] DO
            IF neighbor NOT IN visited THEN
                visited.add(neighbor)
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    RETURN result
END FUNCTION
5. Dijkstra's Algorithm (Single Source Shortest Path)
text
FUNCTION dijkstra(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    minHeap = MinHeap()
    minHeap.push((0, start))  // (distance, node)
    visited = SET()
    
    WHILE NOT minHeap.isEmpty() DO
        currDist, node = minHeap.pop()
        
        IF node IN visited THEN
            CONTINUE
        END IF
        visited.add(node)
        
        FOR neighbor, weight IN graph.vertices[node] DO
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
6. Bellman-Ford Algorithm (Handles Negative Weights)
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
7. Floyd-Warshall Algorithm (All-Pairs Shortest Paths)
text
FUNCTION floydWarshall(graph)
    // Initialize distance matrix
    n = len(graph.vertices)
    dist = MATRIX(n x n, initialized to INFINITY)
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
    
    // Triple nested loop for Floyd-Warshall
    FOR k = 0 TO n-1 DO
        FOR i = 0 TO n-1 DO
            FOR j = 0 TO n-1 DO
                IF dist[i][k] + dist[k][j] < dist[i][j] THEN
                    dist[i][j] = dist[i][k] + dist[k][j]
                END IF
            END FOR
        END FOR
    END FOR
    
    RETURN dist
END FUNCTION
8. A Search Algorithm*
text
FUNCTION aStar(graph, start, goal, heuristic)
    openSet = MinHeap()
    openSet.push((0, start))
    cameFrom = {}
    gScore = {}
    fScore = {}
    
    FOR v IN graph.vertices DO
        gScore[v] = INFINITY
        fScore[v] = INFINITY
    END FOR
    gScore[start] = 0
    fScore[start] = heuristic(start, goal)
    
    closedSet = SET()
    
    WHILE NOT openSet.isEmpty() DO
        f, current = openSet.pop()
        
        IF current == goal THEN
            RETURN reconstructPath(cameFrom, current)
        END IF
        
        closedSet.add(current)
        
        FOR neighbor, weight IN graph.vertices[current] DO
            IF neighbor IN closedSet THEN
                CONTINUE
            END IF
            
            tentativeGScore = gScore[current] + weight
            
            IF tentativeGScore < gScore[neighbor] THEN
                cameFrom[neighbor] = current
                gScore[neighbor] = tentativeGScore
                fScore[neighbor] = gScore[neighbor] + heuristic(neighbor, goal)
                openSet.push((fScore[neighbor], neighbor))
            END IF
        END FOR
    END WHILE
    
    RETURN null  // No path found
END FUNCTION

FUNCTION reconstructPath(cameFrom, current)
    path = [current]
    WHILE current IN cameFrom DO
        current = cameFrom[current]
        path.prepend(current)
    END WHILE
    RETURN path
END FUNCTION
9. Topological Sort (Kahn's Algorithm with In-Degree)
text
FUNCTION topologicalSortKahn(graph)
    inDegree = {}
    FOR v IN graph.vertices DO
        inDegree[v] = 0
    END FOR
    
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.vertices[u] DO
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
        FOR neighbor, weight IN graph.vertices[node] DO
            inDegree[neighbor] -= 1
            IF inDegree[neighbor] == 0 THEN
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    IF len(result) != len(graph.vertices) THEN
        RETURN null  // Cycle detected
    END IF
    
    RETURN result
END FUNCTION
10. Topological Sort (DFS-based)
text
FUNCTION topologicalSortDFS(graph)
    visited = SET()
    stack = STACK()
    
    FUNCTION dfs(node)
        visited.add(node)
        FOR neighbor, weight IN graph.vertices[node] DO
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
11. Cycle Detection – Undirected Graph
text
FUNCTION hasCycleUndirected(graph)
    visited = SET()
    
    FUNCTION dfs(node, parent)
        visited.add(node)
        FOR neighbor, weight IN graph.vertices[node] DO
            IF neighbor NOT IN visited THEN
                IF dfs(neighbor, node) THEN
                    RETURN true
                END IF
            ELSE IF neighbor != parent THEN
                RETURN true
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
12. Cycle Detection – Directed Graph (DFS with Colors)
text
FUNCTION hasCycleDirected(graph)
    // WHITE = 0, GRAY = 1, BLACK = 2
    color = {}
    FOR v IN graph.vertices DO
        color[v] = 0
    END FOR
    
    FUNCTION dfs(node)
        color[node] = 1  // GRAY
        FOR neighbor, weight IN graph.vertices[node] DO
            IF color[neighbor] == 0 THEN
                IF dfs(neighbor) THEN
                    RETURN true
                END IF
            ELSE IF color[neighbor] == 1 THEN
                RETURN true  // Back edge
            END IF
        END FOR
        color[node] = 2  // BLACK
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
13. Connected Components
text
FUNCTION connectedComponents(graph)
    visited = SET()
    components = []
    
    FUNCTION dfs(node, component)
        visited.add(node)
        component.append(node)
        FOR neighbor, weight IN graph.vertices[node] DO
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
14. Bipartite Check (2-Coloring)
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
                FOR neighbor, weight IN graph.vertices[node] DO
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
15. Strongly Connected Components (Kosaraju's Algorithm)
text
FUNCTION kosaraju(graph)
    visited = SET()
    stack = STACK()
    
    // First DFS to fill stack by finish time
    FUNCTION dfs1(node)
        visited.add(node)
        FOR neighbor, weight IN graph.vertices[node] DO
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
    
    // Create reversed graph
    reversedGraph = Graph()
    reversedGraph.isDirected = true
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.vertices[u] DO
            reversedGraph.addEdge(v, u, weight)
        END FOR
    END FOR
    
    // Second DFS on reversed graph
    visited = SET()
    sccs = []
    
    FUNCTION dfs2(node, scc)
        visited.add(node)
        scc.append(node)
        FOR neighbor, weight IN reversedGraph.vertices[node] DO
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
16. Minimum Spanning Tree (Kruskal's Algorithm)
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
17. Minimum Spanning Tree (Prim's Algorithm)
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
        
        FOR v, edgeWeight IN graph.vertices[u] DO
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
18. Union-Find Data Structure
text
CLASS UnionFind
    parent = {}
    rank = {}
    
    FUNCTION makeSet(x)
        parent[x] = x
        rank[x] = 0
    END FUNCTION
    
    FUNCTION find(x)
        IF parent[x] != x THEN
            parent[x] = find(parent[x])  // Path compression
        END IF
        RETURN parent[x]
    END FUNCTION
    
    FUNCTION union(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN
        END IF
        
        // Union by rank
        IF rank[rootX] < rank[rootY] THEN
            parent[rootX] = rootY
        ELSE IF rank[rootX] > rank[rootY] THEN
            parent[rootY] = rootX
        ELSE
            parent[rootY] = rootX
            rank[rootX] += 1
        END IF
    END FUNCTION
END CLASS
19. Maximum Flow (Ford-Fulkerson with DFS)
text
FUNCTION fordFulkerson(graph, source, sink)
    // Create residual graph
    residualGraph = COPY(graph)
    maxFlow = 0
    
    WHILE true DO
        // Find augmenting path using DFS
        path = findAugmentingPathDFS(residualGraph, source, sink)
        
        IF path == null THEN
            BREAK
        END IF
        
        // Find minimum capacity along path
        minCapacity = INFINITY
        FOR i = 0 TO len(path)-2 DO
            u, v = path[i], path[i+1]
            FOR neighbor, capacity IN residualGraph.vertices[u] DO
                IF neighbor == v THEN
                    minCapacity = MIN(minCapacity, capacity)
                END IF
            END FOR
        END FOR
        
        // Update residual capacities
        FOR i = 0 TO len(path)-2 DO
            u, v = path[i], path[i+1]
            // Decrease forward edge
            FOR j = 0 TO len(residualGraph.vertices[u])-1 DO
                neighbor, capacity = residualGraph.vertices[u][j]
                IF neighbor == v THEN
                    residualGraph.vertices[u][j] = (neighbor, capacity - minCapacity)
                END IF
            END FOR
            // Increase backward edge (reverse edge)
            residualGraph.addEdge(v, u, minCapacity)
        END FOR
        
        maxFlow += minCapacity
    END WHILE
    
    RETURN maxFlow
END FUNCTION

FUNCTION findAugmentingPathDFS(graph, source, sink)
    visited = SET()
    path = []
    
    FUNCTION dfs(node)
        IF node == sink THEN
            RETURN true
        END IF
        visited.add(node)
        FOR neighbor, capacity IN graph.vertices[node] DO
            IF neighbor NOT IN visited AND capacity > 0 THEN
                path.append(neighbor)
                IF dfs(neighbor) THEN
                    RETURN true
                END IF
                path.pop()
            END IF
        END FOR
        RETURN false
    END FUNCTION
    
    path.append(source)
    IF dfs(source) THEN
        RETURN path
    ELSE
        RETURN null
    END IF
END FUNCTION
20. Tarjan's SCC Algorithm
text
FUNCTION tarjanSCC(graph)
    index = 0
    stack = STACK()
    indices = {}
    lowlinks = {}
    onStack = SET()
    sccs = []
    
    FUNCTION strongconnect(v)
        indices[v] = index
        lowlinks[v] = index
        index += 1
        stack.push(v)
        onStack.add(v)
        
        FOR w, weight IN graph.vertices[v] DO
            IF w NOT IN indices THEN
                strongconnect(w)
                lowlinks[v] = MIN(lowlinks[v], lowlinks[w])
            ELSE IF w IN onStack THEN
                lowlinks[v] = MIN(lowlinks[v], indices[w])
            END IF
        END FOR
        
        IF lowlinks[v] == indices[v] THEN
            scc = []
            WHILE true DO
                w = stack.pop()
                onStack.remove(w)
                scc.append(w)
                IF w == v THEN
                    BREAK
                END IF
            END WHILE
            sccs.append(scc)
        END IF
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN indices THEN
            strongconnect(v)
        END IF
    END FOR
    
    RETURN sccs
END FUNCTION
These pseudocode implementations provide the foundation for solving graph problems in interviews, competitions, and production systems!