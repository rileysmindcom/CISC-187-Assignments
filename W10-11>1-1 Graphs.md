# Assignment W10 1-1 Graphs
## Lab Objective
My objective is understanding the relationships between connected items, and how a standard graph represents the traversal methods used in C++. This helped me identify vertices, edges, adjacency, distances, and pathways by studying breadth-first Search (BFS), and Depth-First Search (DPS), Dijkstra's shortest-path method, and comparing the differences between graphs, and trees. This lab also explains the time, and space complexity of the graph operations that can determine, and be used to analyze Big-O notation.

## Assignment Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <algorithm>

using namespace std;

class Graph
{
private:
    vector<vector<int>> adjacencyList;

public:

    Graph(int vertices)
    {
        adjacencyList.resize(vertices);
    }

    void addEdge(int source, int destination)
    {
        if (source < 0 || destination < 0 ||
            source >= adjacencyList.size() ||
            destination >= adjacencyList.size())
        {
            cout << "Invalid vertex!" << endl;
            return;
        }

        adjacencyList[source].push_back(destination);
        adjacencyList[destination].push_back(source);
    }

    void display() const
    {
        cout << "\nAdjacency List:\n";

        for (int i = 0; i < adjacencyList.size(); i++)
        {
            cout << char('A' + i) << ": ";

            for (int neighbor : adjacencyList[i])
            {
                cout << char('A' + neighbor) << " ";
            }

            cout << endl;
        }
    }

    void bfs(int start) const
    {
        if (start < 0 || start >= adjacencyList.size())
        {
            cout << "Invalid starting vertex!" << endl;
            return;
        }

        vector<bool> discovered(adjacencyList.size(), false);

        queue<int> q;

        discovered[start] = true;
        q.push(start);

        cout << "\nBFS Traversal: ";

        while (!q.empty())
        {
            int current = q.front();
            q.pop();

            cout << char('A' + current) << " ";

            vector<int> neighbors = adjacencyList[current];

            sort(neighbors.begin(), neighbors.end());

            for (int neighbor : neighbors)
            {
                if (!discovered[neighbor])
                {
                    discovered[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }

        cout << endl;
    }

    void dfs(int start) const
    {
        if (start < 0 || start >= adjacencyList.size())
        {
            cout << "Invalid starting vertex!" << endl;
            return;
        }

        vector<bool> visited(adjacencyList.size(), false);

        stack<int> s;

        s.push(start);

        cout << "\nDFS Traversal: ";

        while (!s.empty())
        {
            int current = s.top();
            s.pop();

            if (!visited[current])
            {
                visited[current] = true;

                cout << char('A' + current) << " ";

                vector<int> neighbors = adjacencyList[current];

                sort(neighbors.begin(), neighbors.end());

                for (int neighbor : neighbors)
                {
                    if (!visited[neighbor])
                    {
                        s.push(neighbor);
                    }
                }
            }
        }

        cout << endl;
    }
};

int main()
{
    
    Graph graph(6);

    graph.addEdge(0, 1);
    graph.addEdge(0, 3);
    graph.addEdge(1, 2); 
    graph.addEdge(1, 4); 
    graph.addEdge(3, 4);
    graph.addEdge(4, 5); 
    graph.addEdge(2, 5); 

    graph.display();

    graph.bfs(0);

    graph.dfs(0);

    return 0;
}
```

## Background, and Flowchart
<img width="572" height="562" alt="W10 - 11 1-1 Graphs" src="https://github.com/user-attachments/assets/e68973b5-29b1-4eb7-aba1-9bef2f50830b" />
This is a graph we can use to label the relationships, and connections with these items
The vertices, and edges join them together to make up a graph, and I was able break them down into elements used for this terminology

We can breakdown this terminology by describing the components used for this flowchart:
Vertex
- Is a single object or item in the graph.

Edge
- A connection between two vertices used by an edge. 

Adjacent Vertices
- When Two vertices are adjacent helps an edge to connect them together

Path
- A set of edges joining a starting vertex to its destination

Distance
- Is the quantity of edges in an unweighted graphs shortest path between two vertices.

## Graph Terminology
As example. Here is an undirected graph:
```
A-B
A-C
B-C
B-D
C-E
D-E
E-F
```

### 1. Listing Vertices
Since the vertices are:
```
A, B, C, D, E, F.
```
This means there are six vertices in total

### 2. Listing every vertex adjacent to B
The vertices next to B are
```
A, C, D
```
### 3. We List every vertex adjacent to E
The adjacent vertices to E are
```
C, D, F
```

### 4. We offer a valid path from A to F
An example for a valid path becomes:
```
A → B → C → E → F.
```
### 5. How do we determine the length of the path?
The path contains four edges
So then the path length is *4*

### 6. How do we determine the shortest distances from A to F
Since the shortest distance from A to F is 4 edges

One shortest path becomes:
```
A → B → C → E → F.
```
And, another shortest path becomes:
```
A → B → D → E → F.
```
### 7. What is the difference between path length, and distance
The number of edges to a specific path connects the two vertices refer to the path length. So the smallest number of edges would require to go between two vertices to the distance. Meaning the path would require more edges for reaching the target because it may be longer than the shortest path.

## Graphs vs Trees
All of the trees make up the vertices connecting from the edges because they're all graphs. But, not all graphs are trees. So a unique tree for a graph needs to be connected, and devoid the cycles.

### Cycles
When the path begins, and finishes at the same vertex without repeating other vertices, it belongs in a cycle. Because the connection cannot form closed loops, and trees cannot contain cycles.

### Connectivity
The pairs of the vertices link the graph with a path connecting them. So the vertex in the tree must be reachable from every vertex in order for it to be linked together.

### Disconnected Vertices
Vertices or groups of vertices that are unable to communicate with each other make up a disconnected graph. Because the tree must connect together of all its vertices, a disconnected graph cannot be considered a tree

### How do we determine whether the Structure is a Tree?
```
Structure                                                Tree?                  Reason
A graph connected with no cycles                          Yes              There is a connection containing no cycles
Connected graph that contains a cycle                      No               The Cycles don't exist in trees
The two disconnected components in an acyclic graph         No               The tree must be connected
```
So the disconnected acyclic graph is known as a forest because of its consistent of multiple separate trees

### Adjacency List Representation
We keep each vertex next to the vertices that are related directly to an adjacent list with networks tied together.

Here's a graph using Part 1:
```
A-B
A-C
B-C
B-D
C-E
D-E
E-F
```
#### Adjacency List
```
Vertex    Adjacent Vertices
A              B, C
B             A, C, D
C             A, B, E
D               B, E
E              C, D, F
F                 E
```

### 1. Why does every undirected edge show up twice.
The two vertices are connected in both directions from each undirected edge
As an example if the adjacency list of A, and B both contain the edge of A-b. Then the connection needs to be stored at both vertices

### 2. Why is the total storage O(V + E)?
All the vertices, and their relationships must be stored in an adjacency list

- V represents the number of vertices

- E represents the number of edges

Every undirected edge would be recorded twice so the vertex can have an entry.

This results, O(V + E) equaling the total storage

### 3. Why is the adjacency list useful for sparse graphs?
The adjacency list stores connections only that's currently in place, and are helpful for sparse graphs. So they don't need room for all vertex connections. When a graph has fewer edges the maximum number of possible edges helping its effectiveness in memory.

## Adjacency Matrix Representation
The adjacency matrix uses a two-dimensional array to represent a graph. The vertex is showed through each row, and column. So the edge between two vertices are viewed by the value of 1, while as the absence of an edge is showed through the value of 0.

#### Adjacency Matrix
We can use a graph for Part 1:

```
      A      B      C      D      E      F
A    0       1      1      0      0      0
B    1       0      1      1      0      0
C    1       1      0      0      1      0
D    0       1      0      0      1      0
E    0       0      1      1      0      1
F    0       0      0      0      1      0
```
1. Why is the matrix symmetric?
Because the graph is undirected so the matrix is symmetric. Vertex B connects vertex A if they are both connected. Resulting the values on either side of the major diagonal are equal.

2. How quickly can the adjacency matrix determine whether the two vertices are adjacent?
So O(1) time is an adjacency matrix, and can identify if the two vertices are nearby. So the only matching row, and column position would need to be accessed through the program. As example, the matrix[A][B] needs to be examined whether A is connected to B.

3. Why does the matrix require O(V^2) space?
Well there are V rows, and columns in an adjacency matrix. As a result V times V storage places are needed. For O(V) in its space complexity

4. Why is a representation wasteful for sparse graphs?
There are many cases when the majority of those connections don't exist so the adjacent matrix retains space for every connection. The matrix positions in sparse graphs contain zero. The connections that aren't real are stored in memory.

## 8. Implement an Adjacency-List Graph
We can use the graph to explain the use of vectors to represent an adjacency list. Here's the line:
```cpp
vector<vector<int>> adjacencyList;
```
So the index show the vertex, and its vector storing the adjacent vertices.

#### addEdge()
This function makes a connection between the two vertices
Both adjacency lists need to be updated because the graph is undirected
```
void addEdge(int source, int destination)
{
    adjacencyList[source].push_back(destination);
    adjacencyList[destination].push_back(source);
}
```
As an example when adding an edge:
0 - 1

The vertex 0, and 1 both update.

#### display()
The function is used to display each vertex following the adjacent vertices
```
void display() const
{
    for (int i = 0; i < adjacencyList.size(); i++)
    {
        cout << i << ": ";

        for (int neighbor : adjacencyList[i])
        {
            cout << neighbor << " ";
        }

        cout << endl;
    }
}
```
Making it easier to look at the connections in the graph

## 9. Breadth-First Search (BFS)
Its a traversal graph algorithm called Breadth-First Search visiting vertices through ascending order to the distance from the starting vertex. So the queue is used by BFS for monitoring vertices that are waiting to be visited. The First In, First Out (FIFO) rule governs the queues.

#### BFS Implementation
```cpp
void bfs(int start) const
{
    vector<bool> discovered(adjacencyList.size(), false);

    queue<int> q;

    discovered[start] = true;
    q.push(start);

    while (!q.empty())
    {
        int current = q.front();
        q.pop();

        cout << current << " ";

        for (int neighbor : adjacencyList[current])
        {
            if (!discovered[neighbor])
            {
                discovered[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

#### BFS Trace
We can re-view the graph again:
```
A-B
A-D
B-C
B-E
D-E
E-F
C-F
```
As we start from A, and process the adjacent vertices by each letter:
```
Step                 Visited Vertex                    Queue          Discovered Vertices
Initial                    -                             A                    A
1                          A                            B, D                A, B, D
2                          B                           D, C, E           A, B, C, D, E
3                          D                             C, E            A, B, C, D, E
4                          C                             E, F           A, B, C, D, E, F
5                          E                               F            A, B, C, D, E, F
6                          F                             Empty          A, B, C, D, E, F
```

#### BFS Traversal Order
We list out an order that works together in a pattern
```
A → B → D → C → E → F
```
The distance from A to every reachable vertex:
```
Vertex      Distance from A
A                  0
B                  1
C                  2
D                  1
E                  2
F                  3
```
BFS explains the vertices by each level, and can find the shortest distance by using an unweighted graph.

## Depth-First Search (DFS)
Its a traversal graph technique used to look at a path deeply as it can before turning around. The stack is used through these patterns. Then we use Last In, First Out (LIFO) principle governing those stacks.

#### DFS Implementation
```
void dfs(int start) const
{
    vector<bool> visited(adjacencyList.size(), false);

    stack<int> s;

    s.push(start);

    while (!s.empty())
    {
        int current = s.top();
        s.pop();

        if (!visited[current])
        {
            visited[current] = true;

            cout << current << " ";

            for (int neighbor : adjacencyList[current])
            {
                if (!visited[neighbor])
                {
                    s.push(neighbor);
                }
            }
        }
    }
}
```
#### DFS Trace
The adjacent vertices are ordered alphabetically. So the previous neighboring vertex pushes to the next one because the stack is LIFO. The implementation above, which indicates the vertices that has visited as they popped. Following the trace resulting in some vertices occurring more than once in the stack.
```
Step                    Popped Vertex          Stack (bottom to top)          Visited Vertices
Initial                       -                          A                          None
1                             A                         B, D                         A
2                             D                         B, E                        A, D
3                             E                      B, B, D, F                    A, D, E
4                             F                      B, B, D, C                   A, D, E, F
5                             C                      B, B, D, B                  A, D, E, F, C
6                             B                     B, B, D, E, C              A, D, E, F, C, B
7                             C                       B, B, D, E               A, D, E, F, C, B
8                             E                         B, B, D                A, D, E, F, C, B
9                             D                          B, B                  A, D, E, F, C, B
10                            B                            B                   A, D, E, F, C, B
11                            B                          Empty                 A, D, E, F, C, B
```

#### DFS Traversal Order
The depth-first search traversal order becomes a pattern of 
```
A → D → E → F → C → B
```

#### Why Can BFS, and DFS Visit the Same Vertices in Different Orders?
There are different data structures used by DFS, and BFS to control traversal. A queue is used by BFS for processing vertices through the order that is found. The most recent vertex is inserted is processed first through the DFS stack. So the algorithm would visit the same vertices in different orders as a result of those variations.

## 11. Directed Graphs
We can use a graph that contains these edges with a direction.
```
A -> B
A -> C
B -> D
C -> B
D -> C
D -> E
```
#### 1. Which vertices are adjacent from A?
The adjacent vertices from A are:

```
B, and C
```

#### 2. Is A adjacent from B?
```
Result: No
``` 
Because there is an edge from Graph A to B on the graph, but there isn't one from B to A. So the only direction to allow movement would be permitted by the directed edges.

#### 3. What is one directed path from A to E?
An example of a directed path is:
```
A → B → D → E.
```

#### 4. How do we identify whether the directed cycle exist
Because the graph has a directed cycle. We can use an example of
```
B → D → C → B.
```
Meaning the path starts, and ends at B to create a cycle

### 5. What is the reason why adding an edge to a directed adjacency list is different versus adding an undirected edge?
The edge in the directed graph only joins vertices in a single direction.
As an example. We look at the lines of:
```
adjacencyList[source].push_back(destination);
```
The vertex stores this destination. So in an undirected graph both vertices should store the connection:
```
adjacencyList[source].push_back(destination);
adjacencyList[destination].push_back(source);
```
This means that these graphs do not create reverse connections automatically.

## Weighted Graphs
Every edge in a weighted graph is given with a numerical value. The costs, distances, time, and other methods used by these numbers. To examine the weighted graph following:
```
A --4-- B
A --2-- C
B --5-- D
C --1-- B
C --8-- D
B --3-- E
D --2-- E
```

Then we can calculate by looking through each section of the graph:

#### 1. Calculate A → B → E.
```
A → B = 4
B → E = 3
Total cost:
4 + 3 = 7
```
#### 2. Calculate A → C → B → E.
```
A → C = 2
C → B = 1
B → E = 3
Total cost:
2 + 1 + 3 = 6
```
#### 3. Calculate A → C → D → E.
```
A → C = 2
C → D = 8
D → E = 2
Total cost:
2 + 8 + 2 = 12
```
### 4. What is the lowest total weight by which path?
We can see the path with the lowest total weight is. From using this pattern:
```
A → C → B → E.
```
The total cost is *6*

```
Path              Total Cost
A → B → E            7
A → C → B → E        6
A → C → D → E        12
```
### 5. Why is the path with its fewest edges aren't necessary compared to the path with the lowest cost?
We look at the path by its weight that it is not always determined by its edge count. The weight of each edge varies. So the individual costs would be higher for a path compared with fewer edges versus for a road with more edges. This gives us a result instead of only counting the edges, and lowest-cost path found by adding the weights of its edges.

## 13. Dijkstra's Shortest Path Algorithm
In a weighted network with negative weights. The algorithm finds the shortest paths from the beginning vertex. So the beginning vertex in this example is A.

#### Initial Values
```
Vertex        Initial Distance          Predecessor
A                     0                    Null
B                  Infinity                Null
C                  Infinity                Null
D                  Infinity                Null
E                  Infinity                Null
```

#### Distance Updates
Dijkstra's algorithm repeats, and selected vertex that hasn't visited with the least distance, and then looks for its neighboring vertices. Which we can calculate this by using:

```
Alternative Distance = Current Distance + Edge Weight
```
The distance, and predecessor are updated if the alternative distance is less than the current distance. As an example:
```
Current Vertex       Adjacent Vertex             Current Distance          Alternative Distance      Update?    Predecessor
A                          B                            0                            4                Yes          A
A                          C                            0                            2                Yes          A                          C
C                          B                            2                            3                Yes          C
C                          D                            2                            10               Yes          C
B                          D                            3                            8                Yes          B
B                          E                            3                            6                Yes          B
D                          E                            8                            10                No          B
```
The alternative distance of 8 results from the process of E's connection to D is equal to the current distance to D.

#### Final Shortest Distances
Then We look for the final shortest distance:
```
Vertex              Shortest Distance from A          Predecessor
A                              0                          Null
B                              3                            C
C                              2                            A
D                              8                            B
E                              6                            B
```

### 1. So the shortest distance from A to every vertex is:
```
A = 0
B = 3
C = 2
D = 8
E = 6
```

### 2. We reconstruct the shortest path from A to E
The predecessor of E is B.
The predecessor of B is C.
The predecessor of C is A.
If we were to work backwards:
```
E ← B ← C ← A.
```
Becomes the pattern. Versus reversing the sequence will give the shortest path
```
A → C → B → E
```
Then the total cost is:
2 + 1 + 3 = 6

### 3. Why are the values for the predecessor necessary?
The importance is the prior vertex used for getting to a vertex such as the shortest path stored through the predecessor values. So Dijkstra's algorithm helps us determine the shortest distance but doesn't reconstruct the actual sequence of vertices. So the program can work backwards from the destination to the beginning of the vertex by the predecessors.


## 14. When is Dijkstra's Algorithm Useful?
```
        Graph                                    Dijkstra's Algorithm Used              Reason
Non-negative edge weights in a weighted graph               Yes                  When edge weights are non-negative so the algorithm is used to explain the shortest pathways
All edges in an unweighted graph have the weight of 1       Yes                 Because Breath-First search becomes useful for weighted graphs when Dijkstra's method is used.
A directed graph with a negative edge weight                No                   The negative edge weights could invalidate the assumption of Dijkstra when the finalized distance doesn't improve.
```

This means that this algorithm works directed, and undirected graphs as long as the edge weights are non-negative so the graphs contain negative edge weights, Bellman-Ford becomes better because of its shortest-path algorithm for networks with negative edge weights.

## 15. Complexity Analysis
Big-O notation helps us understand the performance of the algorithm, and his change when the input of its size increases. Here's as a chart for example:
```
Complexity Table
Operation, and Representation                Big-O              Explanation
Adjacency-list storage                      O(V + E)         Every vertex requires storage so each edge is stored in the adjacency lists.
Adjacency-matrix storage                     O(V²)           The matrix contains V rows and V columns.
Matrix adjacency check                        O(1)           We check for a connection that requires the access of one matrix position
Dijkstra with list-based unvisited queue    O(V² + E)        We find the smallest unvisited distance requiring to scan vertices to examine edges needing other work
Dijkstra with binary-heap priority queue   O((V + E) log V)  The queue supports minimum distance selection, and distance updates.
```

## 16 Complete Program Test
Here's a complete program using six vertices
```
A, B, C, D, E, and F.
```

#### We make connections with the graph
```
A-B
A-D
B-C
B-E
D-E
E-F
C-F
```
The graph explains:
That a vertex has several neighboring vertices
With a cycle of
```
A → B → E → D → A.
```
Creating multiple possible paths between A, and F
#### Adjacency List Output
##### Adjacency List:
```
A: B D
B: A C E
C: B F
D: A E
E: B D F
F: E C
```
#### BFS Output
Starting from A:
BFS Traversal: A B D C E F

#### DFS Output
Starting from A:
DFS Traversal: A D E F C B


### An Explanation for My Results
BFS visits vertices based on how far they are from the initial vertex. Because it makes the use of a queue, vertices are handled in order until they are found. Before going back to the previous vertices, DFS dives deeper into the relationships. Because it employs a stack so the vertex was inserted recently during the first process. This explains how both algorithms visit six vertices effectively because of the traversal techniques used.


## 17. Analysis, and Reflection

### 1. Based on sparse graphs why are adjacency lists chosen.
The adjacency lists are required to keep the connections that are currently in placed compared to the adjacency matrices, which makes space for each potential connection using less memory.

### 2. Why do adjacency matrices offer quick verification?
The program may directly access the position that represents two vertices so the adjacency matrices offer O(1) through adjacency checking.

### 3. Why is a queue used by BFS?
Because BFS follows the methods of First In, and First out to use a queue. Helping the algorithm visit vertices one level at a time beginning with the closest vertices.

### 4. Why is a stack used by DFS?
Because DFS also follows Last In, and First Out methods using a stack. So the program can explore the most recent connections before going back to alternative routes

### 5. Why is it necessary for tracking vertices that are found or visited?
The algorithm is prevented from processing the same vertices more than once by keeping track of discovered or visited vertices. Meaning when a graph has cycles it stops unlimited traversal.

### 6. Why is it possible for BFS to find the shortest distance in an unweighted graph.
BFS reveals vertices from the starting vertex through ascending order of the number of edges. So BFS has to determine the shortest path of a vertex the first time its found.

### 7. Why does BFS doesn't fully solve weighted shortest-path problems?
Instead of considering the overall weight of the edges, BFS takes into account on the number of edges. So the overall cost of a path with fewer edges would be higher. The Algorithm would take edge weight such as Dijkstra's algorithm used when weights are non-negative, which become necessary for the weighted shortest-path issue.

### 8. Why should directed edges shouldn't be treated as bidirectional?
The connections that only move a single direction are represented by directed edges. The structure of the graph would be mistakenly altered if the reverse connection were added without an actual edge.

### 9. Why are the predecessors stored in Dijkstra's algorithm?
A vertex utilized goes to another vertex such as its shorted path, and recorded by the predecessors. So instead of figuring out only the total cost, the algorithm can use this knowledge for reconstructing the real shortest path.

### 10. Why should negative edge weights be avoided when using Dijkstra's algorithm
The technique is used to make an assumption that the vertex's distance cannot get any less after its been processed. So the assumption could be broken by negative edge weights because when negative edge weights are present. Bellman-Ford becomes the right option.


## 18. Challenges
Some of the challenges were understanding how adjacency lists, and matrices are expressed through the connection of the graph. While adjacency matrices use a two-dimensional array for describing the connections between the adjacency lists to record the vertices. Another was the differences between depth-first search, and breadth-first search. Because both algorithms visit the related vertices, but because DFS uses a stack, and BFS utilizes a queue their traversal orders are different. Because the graphs might contain cycles. Making it important to understand how to discovered, and visited vertices function. I also found an algorithm that could be used to process the same connections over again without keeping track of visited vertices. Dijkstra's algorithm was a challenge because of its method for calculating weighted shortest paths because we would have to update the predecessor values to evaluate the distances used. This all became easy at the end because it helped me explain how these graphs, and traversal algorithms all work together to understand their connections throughout the program.





