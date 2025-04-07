# Graph Algorithms in C++   
**Email:** orel2744@gmail.com  

In this project, we implemented several fundamental graph algorithms using C++ without relying on the STL library. The project includes a custom-built graph data structure, as well as helper structures like a queue and a disjoint set. Additionally, a full testing framework is included using the `doctest` library.

---

##  Project Structure

```
C++_ex1/

── graph.hpp / graph.cpp           # Graph class - main data structure
── algorithms.hpp / algorithms.cpp # Implementations of BFS, DFS, Dijkstra, Prim, Kruskal
── helpers.hpp / helpers.cpp       # Helper structures: Queue and DisjointSet

── test_algo.cpp                   # Unit tests for algorithms
── test_graph.cpp                  # Unit tests for Graph class
── test_helper.cpp                 # Unit tests for helpers

── main.cpp                        # Optional main for manual testing
── doctest.h                       # Unit testing framework (single header)
── Makefile                        # Build system
── README.md                       # Project documentation (this file)
```

---

##  Namespaces

All classes and functions are wrapped under the `graph` namespace.

---

##  Class Overviews

### `Graph`

Represents a weighted, undirected graph using an **adjacency matrix**. No STL is used.

**Key Methods:**
- `addEdge(u, v, weight)` – Adds an edge from `u` to `v` with the given weight.
- `isEdge(u, v)` – Checks whether an edge exists between `u` and `v`.
- `weight(u, v)` – Returns the weight of the edge.
- `printGraph(start)` – Recursively prints the structure of the graph starting from a vertex.
- `getNumVertices()` – Returns the total number of vertices.

---

### `Algorithms`

A static class that implements the following algorithms:

- `bfs(Graph g, int src)` – Breadth-First Search from source node `src`, returns a BFS tree.
- `dfs(Graph g, int src)` – Depth-First Search from source, returns a DFS tree.
- `dijkstra(Graph g, int src)` – Builds a **shortest-path tree** from the source using Dijkstra's algorithm (no negative weights allowed).
- `prim(Graph g)` – Computes a **Minimum Spanning Tree** using Prim’s algorithm.
- `kruskal(Graph g)` – Computes a **Minimum Spanning Tree** using Kruskal’s algorithm with DisjointSet.

---

### `helpers.hpp / helpers.cpp`

This file contains two essential helper classes:

#### `Queue`

A circular queue implemented manually without STL.

**Methods:**
- `push(int)` — Inserts an element at the rear.
- `pop()` — Removes and returns the front element.
- `peek()` — Returns the front element without removing it.
- `isEmpty()` — Checks if the queue is empty.
- `isFull()` — Checks if the queue is full.

**Used by:** `Algorithms::bfs()`

#### `DisjointSet`

Implements Union-Find with path compression and union by rank.

**Methods:**
- `find(x)` — Returns the root of the set containing `x`.
- `unite(x, y)` — Unites two sets.

**Used by:** `Algorithms::kruskal()` and cycle-checking tests.

---

##  Build & Run Instructions

You can build and run different test suites or the main program using the provided Makefile.

### Build All

```bash
make
```

### Clean Build

```bash
make clean
```

### Run Tests

```bash
make run_algo_tests     # Run algorithm tests (BFS, DFS, Dijkstra, Prim, Kruskal)
make run_graph_tests    # Run Graph class tests
make run_helper_tests   # Run helpers (Queue + DisjointSet) tests
```

### Run All Tests

```bash
make run_all_tests
```

### Run Main

```bash
make run_main
```

---

##  Algorithms Summary

| Algorithm | Goal | Output |
|----------|------|--------|
| **BFS** | Visit all reachable nodes level-by-level from source | BFS tree |
| **DFS** | Explore depth-first paths from source | DFS tree |
| **Dijkstra** | Find shortest paths from source | Shortest Path Tree  |
| **Prim** | Minimum Spanning Tree (MST) | MST returns a graph showing the tree. |
| **Kruskal** | Minimum Spanning Tree | MST using edge sorting + disjoint and returns a graph showing the tree. |

---

##  Test Example: Dijkstra Cycle Prevention

```cpp
TEST_CASE("Dijkstra - Result tree has no cycles") {
    Graph g(5);
    g.addEdge(0, 1, 2);
    g.addEdge(1, 2, 3);
    g.addEdge(1, 3, 1);
    g.addEdge(3, 4, 4);
    g.addEdge(0, 4, 100); // will not be selected

    Graph result = Algorithms::dijkstra(g, 0);

    DisjointSet ds(result.getNumVertices());
    bool hasCycle = false;
    for (int u = 0; u < 5; u++) {
        for (int v = u + 1; v < 5; v++) {
            if (result.isEdge(u, v)) {
                if (ds.find(u) == ds.find(v)) hasCycle = true;
                else ds.unite(u, v);
            }
        }
    }
    CHECK_FALSE(hasCycle);
}
```

---

##  Notes

- STL is not used.
- Manual memory management using `new` / `delete`.
- Easy to extend and well modularized.
