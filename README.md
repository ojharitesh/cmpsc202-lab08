# Lab 08: Navigating Weighted Graphs with Dijkstra's Algorithm

Name(s): Ritesh Ojha

## Introduction
You are already familiar with Breadth-First Search (BFS) and Depth-First Search (DFS) for exploring graphs. However, what happens when all edges are not created equal? In this lab, we will explore Dijkstra's Algorithm, which allows us to find the shortest path between two vertices in a *weighted* graph.

---

## Part 1: BFS with Weights

BFS is excellent for finding the shortest path in an unweighted graph. Let's see why it fails when weights are introduced. Consider the following weighted graph:

![Part 1 Graph](./part1.png)

**Questions:**
1. If you run a standard BFS starting at Node $A$, looking for Node $D$, which path will the BFS algorithm discover first? Why?
   > BFS will discover A → D first because BFS explores nodes level by level and finds the path with the fewest edges. It ignores edge weights, so it chooses the direct path from A to D.

2. What is the total weight of the path BFS found? What is the total weight of the alternative path? 
   > - BFS path: A → D = 100
   > - Alternative path: A → B → C → D = 2 + 3 + 4 = 9
The alternative path has a much lower total weight, even though it uses more edges.

3. How does the definition of "shortest path" change when moving from unweighted graphs to weighted graphs?
   > In an unweighted graph, the shortest path is the one with the fewest edges. In a weighted graph, the shortest path is the one with the smallest total sum of edge weights, regardless of how many edges it contains. This is why standard BFS works for unweighted graphs but does not always find the shortest path in weighted graphs.

---

## Part 2: Dijkstra's Algorithm

Dijkstra's algorithm uses a greedy approach. It maintains a set of "visited" (finalized) nodes and an array of the shortest known distances to all other nodes. Consider the following weighted graph:

![Part 2 Graph](./part2.png)

**Instructions:**
Starting at Node $A$, trace Dijkstra's algorithm. Fill in the tracker table below. 
*   **Current Node:** The unvisited node with the smallest current distance.
*   **Neighbors to Check:** The neighbors of the current node whose distances might need updating.
*   **`dist` Array:** The current known shortest distance from $A$ to each node. Use $\infty$ for unreachable/unknown nodes.
*   **`visited` Set:** The set of nodes that have been finalized and whose shortest distances are known.

| Step | Current Node | Neighbors to Check | `dist` Map | `visited` Set |
| :--- | :--- | :--- | :--- | :--- |
| 0 | $A$ | $B$, $C$ | $A$:0, $B$:4, $C$:2 | { } |
| 1 | $C$ | $B$, $D$, $E$ | $A$:0, $B$:3, $C$:2, $D$:10, $E$:12 | { $A$ } |
| 2 | $B$ | $D$ | $A$:0, $B$:3, $C$:2, $D$:8, $E$:12 | { $A$, $C$} |
| 3 | $D$ | $E$ | $A$:0, $B$:3, $C$:2, $D$:8, $E$:10 | { $A$, $C$, $B$} |
| 4 | $E$ | None | $A$:0, $B$:3, $C$:2, $D$:8, $E$:10 | { $A$, $C$, $B$, $D$} |

**Conceptual Question:**
When we add a vertex to the "visited" set, why are we 100% certain we have found the absolute shortest path to that node? 
> We are certain because Dijkstra's algorithm always chooses the unvisited node with the smallest known distance. Since all edge weights are nonnegative, no other path through unvisited nodes can have a smaller total weight. Therefore, once a node is added to the visited set, its shortest distance is guaranteed to be correct.

---

## Part 3: Assumptions

Dijkstra's algorithm relies on an assumption about the weights. Let's see what happens when we break it. Consider the following weighted graph, which contains a negative edge:

![Part 3 Graph](./part3.png)

**Instructions:**
Trace Dijkstra's algorithm starting from Node $A$ to find the shortest path to Node $C$.

1. Which node does Dijkstra's algorithm visit first after $A$?
   > Dijkstra's algorithm visits **B** first because its distance from A is 2, which is smaller than C's distance of 5.

2. What will Dijkstra's algorithm declare as the shortest path to $C$, and what is its weight?
   > Dijkstra's algorithm finds **A → B → C**, with a total weight of **2 + (-4) = -2**.

3. Look at the graph manually. What is the *actual* shortest path from $A$ to $C$, and what is its total weight? 
   > The shortest simple path is **A → B → C**, with a total weight of **-2**, which is smaller than the direct path A → C (weight 5).

4. **The Trap:** Explain exactly *why* Dijkstra's algorithm failed to find the correct shortest path in this scenario.
   > Dijkstra's algorithm does not actually fail in this particular example. It visits B first and updates C's distance from 5 to -2, finding the correct shortest simple path.

However, Dijkstra's algorithm assumes all edge weights are nonnegative. Negative weights can cause it to finalize a node before discovering a shorter path.

Additionally, since the graph is undirected, the negative edge between B and C creates a negative cycle (B → C → B) with a total weight of -8. Repeating this cycle allows the path weight to decrease indefinitely, meaning no finite shortest walk exists.

---

## Part 4: Implementing Dijkstra's Algorithm

In the real world, problems rarely look like neat mathematical graphs right away. You often have to translate real-world constraints into a standard graph format so that an algorithm like Dijkstra's can process it.

**The Scenario:**
You are building a navigation app for delivery robots on a college campus. 

* Moving between any two adjacent sidewalk intersections takes exactly $2$ minutes.

* Some paths have stairs. The robots *cannot* use stairs.

* Certain intersections are busy crosswalks. The robot must wait an additional $3$ minutes at these specific intersections before it can safely leave them and continue on its path.

**The Problem:**
Standard Dijkstra's algorithm only understands *edges* and *edge weights*. It does not natively understand "node weights" (like waiting at an intersection) or "impassable terrain".

**Questions:**

1. How should you represent the paths with stairs in your graph so the algorithm never tries to use them?

   > *Your answer here*

2. How can you account for the $3$-minute waiting time at busy crosswalk intersections using *only* edge weights? (Hint: Think about how you could modify the weights of edges connecting to that intersection).

   > *Your answer here*
