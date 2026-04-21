🟢 Interactive Logging: Graphs – Step-by-Step Algorithm Traces
1. Graph Creation and Initialization
text
[Create Graph] graph = Graph()
[Add Vertices] vertices = {1, 2, 3, 4}
[Add Edges]
  graph.addEdge(1, 2, 5)
  graph.addEdge(1, 3, 3)
  graph.addEdge(2, 4, 1)
  graph.addEdge(3, 4, 2)

[Graph State]
Adjacency List:
  1 → [(2, 5), (3, 3)]
  2 → [(1, 5), (4, 1)]
  3 → [(1, 3), (4, 2)]
  4 → [(2, 1), (3, 2)]
2. Depth-First Search (DFS) Trace
text
[Start] DFS from vertex 1
[Visited] = {}, [Stack] = [1], [Result] = []

[Iteration 1]
  [Pop] 1
  [Mark Visited] 1
  [Result] [1]
  [Stack] Push neighbors: 3, 2 → [3, 2]

[Iteration 2]
  [Pop] 2
  [Mark Visited] 2
  [Result] [1, 2]
  [Neighbors] 1 (visited), 4
  [Stack] Push 4 → [3, 4]

[Iteration 3]
  [Pop] 4
  [Mark Visited] 4
  [Result] [1, 2, 4]
  [Neighbors] 2 (visited), 3 (visited)
  [Stack] → [3]

[Iteration 4]
  [Pop] 3
  [Mark Visited] 3
  [Result] [1, 2, 4, 3]
  [Neighbors] 1 (visited), 4 (visited)
  [Stack] → []

[Final Result] [1, 2, 4, 3]
3. Breadth-First Search (BFS) Trace
text
[Start] BFS from vertex 1
[Visited] = {1}, [Queue] = [1], [Result] = []

[Iteration 1]
  [Dequeue] 1
  [Result] [1]
  [Neighbors] 2 (not visited), 3 (not visited)
  [Mark Visited] 2, 3
  [Enqueue] 2, 3 → [2, 3]

[Iteration 2]
  [Dequeue] 2
  [Result] [1, 2]
  [Neighbors] 1 (visited), 4 (not visited)
  [Mark Visited] 4
  [Enqueue] 4 → [3, 4]

[Iteration 3]
  [Dequeue] 3
  [Result] [1, 2, 3]
  [Neighbors] 1 (visited), 4 (visited)
  [Queue] → [4]

[Iteration 4]
  [Dequeue] 4
  [Result] [1, 2, 3, 4]
  [Neighbors] 2 (visited), 3 (visited)
  [Queue] → []

[Final Result] [1, 2, 3, 4]
[Level Order] Level 0: [1], Level 1: [2, 3], Level 2: [4]
4. Dijkstra's Algorithm Trace
text
[Graph with Weights]
  1 --5--> 2
  1 --3--> 3
  2 --1--> 4
  3 --2--> 4

[Initialize]
  distances = {1: 0, 2: ∞, 3: ∞, 4: ∞}
  minHeap = [(0, 1)]
  visited = {}

[Iteration 1]
  [Extract] (0, 1)
  [Mark Visited] 1
  [Process Neighbors]
    [2] newDist = 0 + 5 = 5, update distances[2] = 5
    [3] newDist = 0 + 3 = 3, update distances[3] = 3
  [Heap] [(3, 3), (5, 2)]

[Iteration 2]
  [Extract] (3, 3)
  [Mark Visited] 3
  [Process Neighbors]
    [1] visited, skip
    [4] newDist = 3 + 2 = 5, update distances[4] = 5
  [Heap] [(5, 2), (5, 4)]

[Iteration 3]
  [Extract] (5, 2)
  [Mark Visited] 2
  [Process Neighbors]
    [1] visited, skip
    [4] newDist = 5 + 1 = 6 > distances[4] = 5, no update
  [Heap] [(5, 4)]

[Iteration 4]
  [Extract] (5, 4)
  [Mark Visited] 4
  [Process Neighbors] all visited
  [Heap] []

[Final Shortest Distances]
  1: 0
  2: 5
  3: 3
  4: 5
[Paths]
  1 → 1 (cost 0)
  1 → 2 (cost 5, path: 1→2)
  1 → 3 (cost 3, path: 1→3)
  1 → 4 (cost 5, path: 1→3→4)
5. Bellman-Ford Algorithm Trace
text
[Graph with Potential Negative Weights]
  1 --4--> 2
  1 --2--> 3
  2 --(-3)--> 3
  3 --2--> 4

[Initialize]
  distances = {1: 0, 2: ∞, 3: ∞, 4: ∞}

[Relaxation Pass 1]
  [Edge 1→2] 0 + 4 < ∞ → distances[2] = 4
  [Edge 1→3] 0 + 2 < ∞ → distances[3] = 2
  [Edge 2→3] 4 + (-3) < 2 → distances[3] = 1
  [Edge 3→4] 1 + 2 < ∞ → distances[4] = 3

[Relaxation Pass 2]
  [Edge 1→2] 0 + 4 = 4, no update
  [Edge 1→3] 0 + 2 > 1, no update
  [Edge 2→3] 4 + (-3) = 1, no update
  [Edge 3→4] 1 + 2 = 3, no update

[Relaxation Pass 3] (same, no changes)

[Check Negative Cycle]
  All edges remain unchanged → No negative cycle

[Final Shortest Distances]
  1: 0
  2: 4
  3: 1
  4: 3
6. Topological Sort (DFS-based) Trace
text
[DAG: Course Prerequisites]
  1 → 2
  1 → 3
  2 → 3
  3 → 4

[DFS Stack-based]
[Call] dfs(1)
  [Visit] 1, visited = {1}
  [Neighbor] 2 (not visited)
    [Call] dfs(2)
      [Visit] 2, visited = {1, 2}
      [Neighbor] 3 (not visited)
        [Call] dfs(3)
          [Visit] 3, visited = {1, 2, 3}
          [Neighbor] 4 (not visited)
            [Call] dfs(4)
              [Visit] 4, visited = {1, 2, 3, 4}
              [No neighbors]
              [Push to stack] 4 → stack: [4]
            [Return]
          [Push to stack] 3 → stack: [4, 3]
        [Return]
      [Push to stack] 2 → stack: [4, 3, 2]
    [Return]
  [Neighbor] 3 (visited)
  [Push to stack] 1 → stack: [4, 3, 2, 1]

[Reverse stack] topological order = [1, 2, 3, 4]
7. Cycle Detection (Undirected Graph) Trace
text
[Graph]
  1 -- 2
  2 -- 3
  3 -- 1 (creates cycle: 1-2-3-1)
  4 -- 5

[DFS from 1]
  [Visit] 1, parent = null
  [Neighbor] 2 (not visited, parent is null)
    [Visit] 2, parent = 1
    [Neighbor] 1 (parent, skip)
    [Neighbor] 3 (not visited)
      [Visit] 3, parent = 2
      [Neighbor] 2 (parent, skip)
      [Neighbor] 1 (not parent, visited)
        [Cycle Found!] Edge 3-1 with parent 2
        [Return] true

[Result] Cycle detected: 1-2-3-1
8. Cycle Detection (Directed Graph – Color-based) Trace
text
[Directed Graph with Cycle]
  1 → 2
  2 → 3
  3 → 1 (creates cycle)
  3 → 4

[Colors] WHITE=0, GRAY=1, BLACK=2

[DFS from 1]
  [1] color[1] = GRAY (1)
  [Process 2]
    [2] color[2] = GRAY (1)
    [Process 3]
      [3] color[3] = GRAY (1)
      [Process 1]
        [1 is GRAY] → Back edge detected!
        [Cycle Found] 1 → 2 → 3 → 1
        [Return] true

[Result] Cycle detected in directed graph
9. Connected Components Trace
text
[Graph with Multiple Components]
Component 1: 1-2, 2-3
Component 2: 4-5
Component 3: 6 (isolated)

[DFS from 1]
  [Visit] 1
  [Neighbor] 2 → [Visit] 2
  [Neighbor] 3 → [Visit] 3
  [Component 1] {1, 2, 3}

[DFS from 4] (1, 2, 3 visited)
  [Visit] 4
  [Neighbor] 5 → [Visit] 5
  [Component 2] {4, 5}

[DFS from 6] (1, 2, 3, 4, 5 visited)
  [Visit] 6
  [No neighbors]
  [Component 3] {6}

[Result] 3 connected components: [{1, 2, 3}, {4, 5}, {6}]
10. Bipartite Check (2-Coloring) Trace
text
[Bipartite Graph]
  1 -- 3
  1 -- 4
  2 -- 3
  2 -- 4

[BFS from 1]
  [Color] 1 = 0 (RED)
  [Neighbors] 3, 4
    [Color] 3 = 1 (BLUE)
    [Color] 4 = 1 (BLUE)
  [Queue] [3, 4]

[Process 3]
  [Color] 3 = 1 (BLUE)
  [Neighbors] 1 (RED=0), 2 (not colored)
    [1 is RED] ✓ Different color
    [Color] 2 = 0 (RED)
  [Queue] [4, 2]

[Process 4]
  [Color] 4 = 1 (BLUE)
  [Neighbors] 1 (RED=0), 2 (RED=0)
    Both RED ✓ Different color

[Process 2]
  [Color] 2 = 0 (RED)
  [Neighbors] 3 (BLUE=1), 4 (BLUE=1)
    Both BLUE ✓ Different color

[Result] Graph is bipartite
[Sets] Set A: {1, 2}, Set B: {3, 4}
11. Non-Bipartite Detection Trace
text
[Triangle: 1-2-3-1]
  1 -- 2
  2 -- 3
  3 -- 1

[BFS from 1]
  [Color] 1 = 0 (RED)
  [Neighbors] 2, 3
    [Color] 2 = 1 (BLUE)
    [Color] 3 = 1 (BLUE)
  [Queue] [2, 3]

[Process 2]
  [Color] 2 = 1 (BLUE)
  [Neighbors] 1 (RED ✓), 3 (BLUE)
    [3 is already BLUE, same color] ✗ Conflict!
    [Not bipartite] return false

[Result] Graph is NOT bipartite (contains odd cycle)
12. Kruskal's MST Trace
text
[Edges sorted by weight]
  (2-3, 1)
  (2-4, 1)
  (1-2, 2)
  (1-3, 3)

[Union-Find initialized]
  sets = {{1}, {2}, {3}, {4}}

[Process edge (2-3, 1)]
  [Find] 2 ∈ {2}, 3 ∈ {3}
  [Different sets] Add to MST
  [Union] {2, 3}
  [MST] [(2-3, 1)]

[Process edge (2-4, 1)]
  [Find] 2 ∈ {2, 3}, 4 ∈ {4}
  [Different sets] Add to MST
  [Union] {2, 3, 4}
  [MST] [(2-3, 1), (2-4, 1)]

[Process edge (1-2, 2)]
  [Find] 1 ∈ {1}, 2 ∈ {2, 3, 4}
  [Different sets] Add to MST
  [Union] {1, 2, 3, 4}
  [MST] [(2-3, 1), (2-4, 1), (1-2, 2)]

[Process edge (1-3, 3)]
  [Find] 1 ∈ {1, 2, 3, 4}, 3 ∈ {1, 2, 3, 4}
  [Same set] Skip (would create cycle)

[Result] MST Total Weight = 1 + 1 + 2 = 4
[Edges] {(2-3), (2-4), (1-2)}
13. Prim's MST Trace
text
[Start from vertex 1]
[inMST] = {}, [keys] = {1: 0, 2: ∞, 3: ∞, 4: ∞}

[Extract min] (0, 1)
  [Add to MST] 1
  [Update neighbors]
    [2] key = min(∞, 2) = 2
    [3] key = min(∞, 3) = 3
  [Heap] [(2, 2), (3, 3)]

[Extract min] (2, 2)
  [Add to MST] 2
  [Update neighbors]
    [1] in MST, skip
    [4] key = min(∞, 1) = 1
  [Heap] [(1, 4), (3, 3)]

[Extract min] (1, 4)
  [Add to MST] 4
  [Update neighbors]
    [2] in MST, skip
    [3] key = min(3, 2) = 2
  [Heap] [(2, 3)]

[Extract min] (2, 3)
  [Add to MST] 3
  [Update neighbors] all in MST
  [Heap] []

[Result] MST Total Weight = 0 + 2 + 1 + 2 = 5
[Edges] {(1, 2), (2, 4), (4, 3)}
14. Topological Sort (Kahn's Algorithm) Trace
text
[DAG with in-degrees]
  1: in-degree = 0
  2: in-degree = 1 (from 1)
  3: in-degree = 2 (from 1, 2)
  4: in-degree = 1 (from 3)

[Initialize queue with in-degree 0]
  [Queue] [1]

[Process 1]
  [Add to result] [1]
  [Reduce in-degrees of neighbors]
    [2] in-degree 1 - 1 = 0 → Enqueue
  [Queue] [2]

[Process 2]
  [Add to result] [1, 2]
  [Reduce in-degrees]
    [3] in-degree 2 - 1 = 1
  [Queue] []

[Process 3]
  [No nodes with in-degree 0]
  [Wait for another dependency]
  [But 3 has in-degree 1, so next iteration:]

[After processing node that reduces 3's in-degree]
  [3] in-degree becomes 0 → Enqueue
  [Queue] [3]

[Process 3]
  [Add to result] [1, 2, 3]
  [Reduce in-degrees]
    [4] in-degree 1 - 1 = 0 → Enqueue
  [Queue] [4]

[Process 4]
  [Add to result] [1, 2, 3, 4]
  [Queue] []

[Result] Topological order: [1, 2, 3, 4]
15. Strongly Connected Components (Kosaraju) Trace
text
[Directed Graph]
  1 → 2
  2 → 3
  3 → 1 (SCC: {1, 2, 3})
  3 → 4
  4 → 5
  5 → 4 (SCC: {4, 5})

[Phase 1: DFS to fill stack by finish time]
  [DFS 1] → 1 → 2 → 3 → 4 → 5
    [Finish times] stack: [5, 4, 3, 2, 1]

[Phase 2: Create reversed graph]
  Reversed:
    2 → 1
    3 → 2
    1 → 3 (SCC edge reversed)
    4 → 3
    5 → 4
    4 → 5 (SCC edge reversed)

[Phase 3: DFS on reversed graph in stack order]
  [Pop 5]
    [DFS] 5 → 4 → 5 (cycle)
    [SCC 1] {5, 4}
  [Pop 4] already visited
  [Pop 3]
    [DFS] 3 → 2 → 1 → 3 (cycle)
    [SCC 2] {3, 2, 1}
  [Pop 2] already visited
  [Pop 1] already visited

[Result] SCCs: [{1, 2, 3}, {4, 5}]
These interactive traces illuminate the internal mechanics of graph algorithms—see how state evolves, data structures change, and algorithms solve complex problems step by step. Master these patterns for graph mastery!