# Graphs in C++
 
## Part 1 — Graph Terminology  
1.) A, B, C, D, E, and F
2.) A, C, and D
3.) C, D, and F
4.) A->C->E->F
5.) 3
6.) 3
7.) Path length is the number of edges in a specific path. Distance is the number of edges in the shortest path between two vertices.

## Part 2 — Graphs vs. Trees  
1.) In essence, trees are not "allowed" to have cycles. Cycles are a series of nodes that are all connected. If a graph has a cycle then its not a tree. Trees also have it so that every node is somehow connected to every other node even if indirectly. However, its possible for a graph to no be fully connected. 

Connected graph with no cycles: Yes it can be a tree because it is connected and no cycles exist  
Connected graph containing a cycle: The existence of a cycle means it is not a tree. 
Acyclic graph with two disconnected components: This can't be a tree because a tree needs to have every vertices connected to each other directly or indirectly. 

## Part 3 — Adjacency List Representation  

A: B, and C,  
B: A, C, and D  
C: A, B, and E  
D: B and E  
E: C, D, and F  
F: E    

1.) Each undirected edge appears twice because the connection works in both directions. If A is connected to B, then B is also connected to A.  
2.) The total storage is O(V + E) because we store each vertex and only the edges that actually exist.  
3.) Adjacency lists are useful for sparse graphs because they do not waste space storing connections that do not exist.  

## Part 4 — Adjacency Matrix Representation
```

|   | A | B | C | D | E | F |  
|---|---|---|---|---|---|---|  
| A | 0 | 1 | 1 | 0 | 0 | 0 |  
| B | 1 | 0 | 1 | 1 | 0 | 0 |  
| C | 1 | 1 | 0 | 0 | 1 | 0 |  
| D | 0 | 1 | 0 | 0 | 1 | 0 |  
| E | 0 | 0 | 1 | 1 | 0 | 1 |  
| F | 0 | 0 | 0 | 0 | 1 | 0 |  

```

1.) The matrix is symmetric because the graph is undirected. If A connects to B, then B also connects to A.  
2.) An adjacency matrix can check whether two vertices are adjacent in O(1) time because it can directly check one position in the matrix.  
3.) The matrix requires O(V²) space because it needs one row and one column for every vertex.  
4.) This can waste space for sparse graphs because most of the entries may be 0 when only a small number of edges actually exist.  

## Part 5 — Implement an Adjacency-List Graph

## Part 6 — Breadth-First Search

```

Step | Visited Vertex | Queue      | Discovered Vertices  
-----|----------------|------------|-----------------------  
1    | A              | B, D       | A, B, D  
2    | B              | D, C, E    | A, B, C, D, E  
3    | D              | C, E       | A, B, C, D, E  
4    | C              | E, F       | A, B, C, D, E, F  
5    | E              | F          | A, B, C, D, E, F  
6    | F              | Empty      | A, B, C, D, E, F  

BFS Visitation Order:  
A -> B -> D -> C -> E -> F  

```

Distance from A:  
A: 0  
B: 1  
C: 2  
D: 1  
E: 2  
F: 3  

## Part 7 — Depth-First Search

```

Step | Popped Vertex | Stack (bottom -> top) | Visited Vertices  
-----|---------------|-----------------------|----------------------  
1    | A             | B, D                  | A  
2    | D             | B, A, E               | A, D   
3    | E             | B, A, B, D, F         | A, D, E  
4    | F             | B, A, B, D, C, E      | A, D, E, F  
5    | C             | B, A, B, D, B, F      | A, D, E, F, C  
6    | B             | B, A, B, D, A, C, E   | A, D, E, F, C, B  

```

BFS and DFS can visit the same vertices in different orders because BFS uses a queue while DFS uses a stack. BFS visits vertices based on their distance from the starting vertex, while DFS follows one path as deeply as possible before backtracking.  

## Part 8 — Directed Graphs

## Part 8 — Directed Graphs

1.) B and C  
2.) No. B has an edge to D, but there is no edge from B to A.  
3.) A -> B -> D -> E  
4.) Yes. One directed cycle is B -> D -> C -> B.  
5.) In a directed graph, adding an edge only adds the destination to the source's adjacency list. In an undirected graph, the connection must be added to both vertices' adjacency lists.  

## Part 9 — Weighted Graphs

1.) A -> B -> E : 4 + 3 = 7  
2.) A -> C -> B -> E : 2 + 1 + 3 = 6  
3.) A -> C -> D -> E : 2 + 8 + 2 = 12  

The path with the lowest total weight is: 2 at a cost of 6  

Each edge can have a different weight. A path with more edges can still have a lower total cost if those edges have smaller weights.  

## Part 10 — Dijkstra's Shortest Path Algorithm  

```

Current Vertex | Adjacent Vertex | Current Distance | Alternative Distance | Update? | Predecessor  
---------------|-----------------|------------------|----------------------|---------|------------  
A              | B               | Infinity         | 4                    | Yes     | A  
A              | C               | Infinity         | 2                    | Yes     | A  
C              | B               | 4                | 3                    | Yes     | C  
C              | D               | Infinity         | 10                   | Yes     | C  
B              | D               | 10               | 8                    | Yes     | B  
B              | E               | Infinity         | 6                    | Yes     | B  
E              | D               | 8                | 8                    | No      | B  

```

1.) Shortest distance from A to each vertex:    
A: 0  
B: 3  
C: 2  
D: 8  
E: 6  

2.) Shortest Path from A to E using predecessor information: A -> C -> B -> E  Cost: 6
3.) Predecessor values are needed because the distance only tells us the total cost. The predecessors allow us to work backward from the destination to determine the actual path that was taken.

## Part 11 — When Should Dijkstra's Algorithm Be Used?

1.) Weighted graph with only non-negative edge weights: Yes - Dijkstra is designed to find shortest paths when edge weights are non-negative.  
2.) Unweighted graph treated as all edges having weight 1: Yes - Dijkstra would still work because all edge weights are non-negative, although BFS would be simpler.  
3.) Directed graph containing a negative edge weight: No - Dijkstra should not be used with negative edge weights because it may determine a shortest distance before discovering a cheaper path later.  

## Part 12 — Complexity Analysis

1.) Adjacency-list storage: O(V + E) - Each vertex is stored once and only the edges that actually exist are stored.  
2.) Adjacency-matrix storage: O(V²) - The matrix needs a row and column for every vertex, creating V × V positions.  
3.) Matrix adjacency check: O(1) - We can directly check one position in the matrix to see whether two vertices are connected.  
4.) Dijkstra with list-based unvisited queue: O(V²) - The algorithm processes every vertex, and finding the unvisited vertex with the smallest distance can require searching through the list each time.  
5.) Dijkstra with binary-heap priority queue: O((E + V) log V) - Using a binary heap makes selecting and updating vertices more efficient while the algorithm processes the vertices and edges.  
