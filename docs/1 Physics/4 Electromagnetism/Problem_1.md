# Problem 1


---

## 🧠 Algorithm Overview (Graph-Theoretic Equivalent Resistance)

### 🧩 Circuit Representation:

* Represent the circuit as an **undirected graph** `G = (V, E)`

  * **Nodes (V)** are electrical junctions.
  * **Edges (E)** are resistors, with weights representing resistance in ohms.

---

### 🔁 Reduction Strategy:

1. **Series Detection**:

   * If a node has **degree 2**, and is not a terminal node, and the two adjacent resistors are not part of a cycle → combine them in series:

     $$
     R_{\text{eq}} = R_1 + R_2
     $$

2. **Parallel Detection**:

   * If multiple resistors connect **the same pair of nodes** → combine in parallel:

     $$
     \frac{1}{R_{\text{eq}}} = \sum_i \frac{1}{R_i}
     $$

3. **Repeat** reductions until only **two nodes remain** (source and sink).

---

### 📜 Pseudocode Summary

```text
function compute_equivalent_resistance(graph, node_start, node_end):
    while graph has more than two nodes:
        for each node in graph:
            if node is not start or end:
                if degree(node) == 2 and not in a cycle:
                    combine series resistors
        for each pair of nodes with multiple edges:
            combine parallel resistors
    return resistance between node_start and node_end
```

---

## 🧪 Full Python Implementation

```python
import networkx as nx

def combine_parallel_edges(G):
    """Combine all parallel resistors between the same two nodes."""
    edges = list(G.edges(data=True))
    seen = set()
    for u, v, data in edges:
        if (u, v) in seen or (v, u) in seen:
            continue
        parallels = [(a, b, d) for a, b, d in edges if (a == u and b == v) or (a == v and b == u)]
        if len(parallels) > 1:
            # Combine using reciprocal rule
            inv_sum = sum(1 / d['resistance'] for _, _, d in parallels)
            R_eq = 1 / inv_sum
            G.remove_edges_from([(a, b) for a, b, _ in parallels])
            G.add_edge(u, v, resistance=R_eq)
        seen.add((u, v))

def combine_series_nodes(G, start, end):
    """Combine series resistors at non-terminal, degree-2 nodes not in cycles."""
    nodes_to_check = [n for n in G.nodes() if n not in (start, end) and G.degree[n] == 2]
    for node in nodes_to_check:
        if not nx.cycle_basis(G, node):
            neighbors = list(G.neighbors(node))
            if len(neighbors) == 2:
                u, v = neighbors
                R1 = G[u][node]['resistance']
                R2 = G[node][v]['resistance']
                R_eq = R1 + R2
                G.remove_node(node)
                G.add_edge(u, v, resistance=R_eq)

def equivalent_resistance(G, start, end):
    """Reduce graph to compute equivalent resistance between start and end."""
    while True:
        prev_num_nodes = G.number_of_nodes()
        prev_num_edges = G.number_of_edges()

        combine_parallel_edges(G)
        combine_series_nodes(G, start, end)

        if G.number_of_nodes() == 2 and G.has_edge(start, end):
            return G[start][end]['resistance']
        if G.number_of_nodes() == prev_num_nodes and G.number_of_edges() == prev_num_edges:
            break  # No further simplification possible

    # If no direct edge, use resistance computation via network
    # Use Kirchhoff-based method:
    return resistance_via_laplacian(G, start, end)

def resistance_via_laplacian(G, start, end):
    """Use Laplacian matrix to compute effective resistance between nodes."""
    import numpy as np
    L = nx.laplacian_matrix(G, weight='conductance').toarray()
    L = np.array(L, dtype=float)

    # Add conductance = 1/R
    for u, v, d in G.edges(data=True):
        G[u][v]['conductance'] = 1 / d['resistance']

    # Remove one row/column to make Laplacian invertible
    L = L[:-1, :-1]
    L_inv = np.linalg.pinv(L)

    # Compute potential difference
    b = np.zeros(len(L) + 1)
    b[start] = 1
    b[end] = -1
    b = b[:-1]  # same shape as L_inv
    V = L_inv @ b

    return abs(V[start] - V[end])
```

---

## 🧪 Test Examples

```python
# Example 1: Simple Series
G1 = nx.Graph()
G1.add_edge(0, 1, resistance=5)
G1.add_edge(1, 2, resistance=10)
print("Series (5Ω + 10Ω):", equivalent_resistance(G1, 0, 2))  # Expect 15Ω

# Example 2: Simple Parallel
G2 = nx.Graph()
G2.add_edge(0, 1, resistance=5)
G2.add_edge(0, 1, resistance=10)
print("Parallel (5Ω || 10Ω):", equivalent_resistance(G2, 0, 1))  # Expect 3.33Ω

# Example 3: Nested Network
G3 = nx.Graph()
G3.add_edge(0, 1, resistance=3)
G3.add_edge(1, 2, resistance=6)
G3.add_edge(2, 3, resistance=3)
G3.add_edge(0, 3, resistance=6)
G3.add_edge(1, 3, resistance=2)
print("Complex network:", equivalent_resistance(G3, 0, 3))  # Combines nested series + parallel
```

---

## 📊 Efficiency & Improvements

### Time Complexity

* **Series/parallel reductions**: linear in number of nodes/edges
* **Cycle detection**: DFS or union-find for optimization
* **Laplacian resistance**: O(n³) (matrix pseudoinverse) — expensive for large graphs

### Improvements

* Use **disjoint-set (Union-Find)** for faster cycle detection
* Optimize with **graph contraction** strategies
* Integrate **symbolic computation** for algebraic solutions (with `sympy`)

---

## ✅ Summary

* Graph theory simplifies the modeling and analysis of electrical circuits.
* This method is **modular**, **scalable**, and works with **arbitrary topologies**.
* The algorithm handles **series**, **parallel**, and **nested** configurations with minimal code changes.
* Future improvements could support symbolic calculations or real-time simulation.

---



