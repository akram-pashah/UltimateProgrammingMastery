🟢 Examples: Graphs – Essential, Advanced, and FAANG-Level Mastery
1. Basic Graph Creation and Adjacency List
text
CLASS Graph
    vertices = {}  // Map: vertex -> list of neighbors
    
    FUNCTION addVertex(v)
        IF v NOT IN vertices THEN
            vertices[v] = []
        END IF
    END FUNCTION
    
    FUNCTION addEdge(u, v, weight = 1)
        addVertex(u)
        addVertex(v)
        vertices[u].append((v, weight))
        // For undirected: vertices[v].append((u, weight))
    END FUNCTION
    
    FUNCTION getNeighbors(v)
        RETURN vertices[v]
    END FUNCTION
END CLASS

// Usage
graph = Graph()
graph.addEdge(1, 2, 5)
graph.addEdge(1, 3, 3)
graph.addEdge(2, 4, 1)
2. Depth-First Search (DFS) – Recursive
text
FUNCTION dfsRecursive(graph, start)
    visited = set()
    result = []
    
    FUNCTION dfs(node)
        visited.add(node)
        result.append(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs(neighbor)
            END IF
        END FOR
    END FUNCTION
    
    dfs(start)
    RETURN result
END FUNCTION
Use Case: Topological sort, cycle detection, connected components.

3. DFS – Iterative (with Stack)
text
FUNCTION dfsIterative(graph, start)
    visited = set()
    stack = [start]
    result = []
    
    WHILE NOT stack.isEmpty() DO
        node = stack.pop()
        IF node NOT IN visited THEN
            visited.add(node)
            result.append(node)
            FOR neighbor, weight IN graph.getNeighbors(node) DO
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
    visited = set()
    queue = Queue()
    queue.enqueue(start)
    visited.add(start)
    result = []
    
    WHILE NOT queue.isEmpty() DO
        node = queue.dequeue()
        result.append(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                visited.add(neighbor)
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    RETURN result
END FUNCTION
Use Case: Shortest path (unweighted), level-order exploration, social network radius.

5. Dijkstra's Algorithm (Shortest Path – Positive Weights)
text
FUNCTION dijkstra(graph, start)
    distances = {}
    distances[start] = 0
    FOR v IN graph.vertices DO
        IF v != start THEN
            distances[v] = INFINITY
        END IF
    END FOR
    
    minHeap = MinHeap()
    minHeap.push((0, start))  // (distance, node)
    visited = set()
    
    WHILE NOT minHeap.isEmpty() DO
        dist, node = minHeap.pop()
        IF node IN visited THEN
            CONTINUE
        END IF
        visited.add(node)
        
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                newDist = dist + weight
                IF newDist < distances[neighbor] THEN
                    distances[neighbor] = newDist
                    minHeap.push((newDist, neighbor))
                END IF
            END IF
        END FOR
    END WHILE
    
    RETURN distances
END FUNCTION
Interviewed at: Google, Amazon, Uber, Microsoft.
Real-world: GPS navigation, network routing.

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
            FOR v, weight IN graph.getNeighbors(u) DO
                IF distances[u] + weight < distances[v] THEN
                    distances[v] = distances[u] + weight
                END IF
            END FOR
        END FOR
    END FOR
    
    // Check for negative cycles
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.getNeighbors(u) DO
            IF distances[u] + weight < distances[v] THEN
                RETURN "Negative cycle detected"
            END IF
        END FOR
    END FOR
    
    RETURN distances
END FUNCTION
Use Case: Currency exchange, negative weights allowed.

7. Topological Sort (DAG only) – DFS
text
FUNCTION topologicalSort(graph)
    visited = set()
    stack = []
    
    FUNCTION dfs(node)
        visited.add(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs(neighbor)
            END IF
        END FOR
        stack.append(node)
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            dfs(v)
        END IF
    END FOR
    
    REVERSE(stack)
    RETURN stack
END FUNCTION
Interviewed at: Google, Facebook, Amazon.
Use Case: Course prerequisites, task scheduling, build systems.

8. Cycle Detection – Undirected Graph (DFS)
text
FUNCTION hasCycleUndirected(graph)
    visited = set()
    
    FUNCTION dfs(node, parent)
        visited.add(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                IF dfs(neighbor, node) THEN
                    RETURN true
                END IF
            ELSE IF neighbor != parent THEN
                RETURN true  // Found cycle
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
9. Cycle Detection – Directed Graph (DFS with Colors)
text
FUNCTION hasCycleDirected(graph)
    // Colors: WHITE=0 (unvisited), GRAY=1 (visiting), BLACK=2 (visited)
    color = {}
    FOR v IN graph.vertices DO
        color[v] = 0
    END FOR
    
    FUNCTION dfs(node)
        color[node] = 1  // GRAY
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF color[neighbor] == 0 THEN
                IF dfs(neighbor) THEN
                    RETURN true
                END IF
            ELSE IF color[neighbor] == 1 THEN
                RETURN true  // Back edge = cycle
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
10. Connected Components (DFS)
text
FUNCTION connectedComponents(graph)
    visited = set()
    components = []
    
    FUNCTION dfs(node, component)
        visited.add(node)
        component.append(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
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
Interviewed at: Facebook, Google.
Use Case: Number of islands, social network clusters.

11. Bipartite Check (2-Coloring with BFS)
text
FUNCTION isBipartite(graph)
    color = {}
    FOR v IN graph.vertices DO
        color[v] = -1
    END FOR
    
    FOR start IN graph.vertices DO
        IF color[start] == -1 THEN
            queue = Queue()
            queue.enqueue(start)
            color[start] = 0
            
            WHILE NOT queue.isEmpty() DO
                node = queue.dequeue()
                FOR neighbor, weight IN graph.getNeighbors(node) DO
                    IF color[neighbor] == -1 THEN
                        color[neighbor] = 1 - color[node]
                        queue.enqueue(neighbor)
                    ELSE IF color[neighbor] == color[node] THEN
                        RETURN false  // Same color = not bipartite
                    END IF
                END FOR
            END WHILE
        END IF
    END FOR
    
    RETURN true
END FUNCTION
Interviewed at: Microsoft, Facebook.

12. Strongly Connected Components (Kosaraju's Algorithm)
text
FUNCTION sccKosaraju(graph)
    // Step 1: DFS on original graph, record finish times
    visited = set()
    stack = []
    
    FUNCTION dfs1(node)
        visited.add(node)
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                dfs1(neighbor)
            END IF
        END FOR
        stack.append(node)
    END FUNCTION
    
    FOR v IN graph.vertices DO
        IF v NOT IN visited THEN
            dfs1(v)
    END FOR
    
    // Step 2: Create reversed graph
    reversedGraph = Graph()
    FOR u IN graph.vertices DO
        FOR v, weight IN graph.getNeighbors(u) DO
            reversedGraph.addEdge(v, u, weight)
        END FOR
    END FOR
    
    // Step 3: DFS on reversed graph in reverse finish order
    visited = set()
    sccs = []
    
    FUNCTION dfs2(node, scc)
        visited.add(node)
        scc.append(node)
        FOR neighbor, weight IN reversedGraph.getNeighbors(node) DO
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
Interviewed at: Google, Amazon, LinkedIn.
Use Case: Social network analysis, compiler design.

13. Minimum Spanning Tree (Kruskal's Algorithm)
text
FUNCTION kruskalMST(graph, edges)
    // edges = [(u, v, weight), ...]
    SORT(edges BY weight)
    
    uf = UnionFind()
    FOR v IN graph.vertices DO
        uf.makeSet(v)
    END FOR
    
    mst = []
    FOR u, v, weight IN edges DO
        IF uf.find(u) != uf.find(v) THEN
            mst.append((u, v, weight))
            uf.union(u, v)
        END IF
    END FOR
    
    RETURN mst
END FUNCTION
Complexity: O(E log E) for sorting.
Use Case: Network design, clustering.

14. Minimum Spanning Tree (Prim's Algorithm)
text
FUNCTION primMST(graph, start)
    inMST = set()
    key = {}  // Minimum weight to connect to MST
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
    
    RETURN parent, key
END FUNCTION
Complexity: O(E log V) with heap.

15. Number of Islands (DFS/BFS - Interview Classic)
text
FUNCTION numIslands(grid)
    // grid[i][j] = '1' for land, '0' for water
    visited = set()
    count = 0
    
    FUNCTION dfs(i, j)
        IF i < 0 OR i >= len(grid) OR j < 0 OR j >= len(grid[0]) THEN
            RETURN
        END IF
        IF (i, j) IN visited OR grid[i][j] == '0' THEN
            RETURN
        END IF
        visited.add((i, j))
        dfs(i+1, j)
        dfs(i-1, j)
        dfs(i, j+1)
        dfs(i, j-1)
    END FUNCTION
    
    FOR i = 0 TO len(grid)-1 DO
        FOR j = 0 TO len(grid[0])-1 DO
            IF grid[i][j] == '1' AND (i, j) NOT IN visited THEN
                dfs(i, j)
                count += 1
            END IF
        END FOR
    END FOR
    
    RETURN count
END FUNCTION
Interviewed at: Google, Amazon, Facebook, Apple.

16. Graph Clone (Deep Copy)
text
FUNCTION cloneGraph(node)
    IF node == null THEN
        RETURN null
    END IF
    
    cloneMap = {}  // Original node -> Cloned node
    
    FUNCTION dfs(original)
        IF original IN cloneMap THEN
            RETURN cloneMap[original]
        END IF
        
        clone = Node(original.value)
        cloneMap[original] = clone
        
        FOR neighbor IN original.neighbors DO
            clone.neighbors.append(dfs(neighbor))
        END FOR
        
        RETURN clone
    END FUNCTION
    
    RETURN dfs(node)
END FUNCTION
Interviewed at: LinkedIn, Google, Amazon.

17. Alien Dictionary (Topological Sort)
text
FUNCTION alienOrder(words)
    // Build graph from word ordering
    graph = {}
    inDegree = {}
    
    // Initialize all characters
    FOR word IN words DO
        FOR char IN word DO
            IF char NOT IN graph THEN
                graph[char] = []
                inDegree[char] = 0
            END IF
        END FOR
    END FOR
    
    // Build edges
    FOR i = 0 TO len(words)-2 DO
        word1, word2 = words[i], words[i+1]
        minLen = MIN(len(word1), len(word2))
        FOR j = 0 TO minLen-1 DO
            IF word1[j] != word2[j] THEN
                IF word2[j] NOT IN graph[word1[j]] THEN
                    graph[word1[j]].append(word2[j])
                    inDegree[word2[j]] += 1
                END IF
                BREAK
            END IF
        END FOR
    END FOR
    
    // Topological sort with Kahn's algorithm
    queue = Queue()
    FOR char IN graph DO
        IF inDegree[char] == 0 THEN
            queue.enqueue(char)
        END IF
    END FOR
    
    result = []
    WHILE NOT queue.isEmpty() DO
        char = queue.dequeue()
        result.append(char)
        FOR neighbor IN graph[char] DO
            inDegree[neighbor] -= 1
            IF inDegree[neighbor] == 0 THEN
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    RETURN "".join(result)
END FUNCTION
Interviewed at: Google, Facebook.

18. Course Schedule (Cycle Detection in DAG)
text
FUNCTION canFinish(numCourses, prerequisites)
    // prerequisites = [[course, prerequisite], ...]
    graph = [[] FOR i IN 0 TO numCourses-1]
    inDegree = [0 FOR i IN 0 TO numCourses-1]
    
    FOR course, prereq IN prerequisites DO
        graph[prereq].append(course)
        inDegree[course] += 1
    END FOR
    
    queue = Queue()
    FOR i = 0 TO numCourses-1 DO
        IF inDegree[i] == 0 THEN
            queue.enqueue(i)
        END IF
    END FOR
    
    count = 0
    WHILE NOT queue.isEmpty() DO
        course = queue.dequeue()
        count += 1
        FOR nextCourse IN graph[course] DO
            inDegree[nextCourse] -= 1
            IF inDegree[nextCourse] == 0 THEN
                queue.enqueue(nextCourse)
            END IF
        END FOR
    END WHILE
    
    RETURN count == numCourses
END FUNCTION
Interviewed at: Google, Amazon, Apple.

19. Word Ladder (BFS Shortest Path)
text
FUNCTION ladderLength(beginWord, endWord, wordList)
    wordSet = SET(wordList)
    IF endWord NOT IN wordSet THEN
        RETURN 0
    END IF
    
    queue = Queue()
    queue.enqueue((beginWord, 1))
    wordSet.remove(beginWord)
    
    WHILE NOT queue.isEmpty() DO
        word, level = queue.dequeue()
        FOR nextWord IN getNextWords(word, wordSet) DO
            IF nextWord == endWord THEN
                RETURN level + 1
            END IF
            queue.enqueue((nextWord, level + 1))
            wordSet.remove(nextWord)
        END FOR
    END WHILE
    
    RETURN 0
END FUNCTION
Interviewed at: Google, Amazon, Microsoft.

These examples cover foundational to advanced graph patterns—master them for FAANG interviews, system design, and real-world engineering challenges!