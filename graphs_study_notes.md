# Graphs & NetworkX — Study Notes

---

## 🏏 What is a Graph?

A **graph** is a way to represent **relationships between things**.

**Cricket Team Analogy:**
- **Nodes (vertices)** = Players (Suriya, Ravi, Kiran...)
- **Edges** = "has played together with"

```
Suriya ── Ravi
  │         │
Kiran ── Arjun
```

---

## 📌 Types of Graphs

### Undirected Graph
- Friendship — if I'm your friend, you're mine
- Edge has **no direction**
```
Suriya ── Ravi
```

### Directed Graph
- Follow on Instagram — I follow you, doesn't mean you follow me
- Edge has a **direction**
```
Suriya → Ravi   (Suriya follows Ravi, not the other way)
```
> ⚠️ In a directed graph, A→B does **NOT** mean B→A exists!

### Weighted Graph
- Edges have a **cost or distance**
```
Suriya ──5── Ravi   # played 5 matches together
Suriya ──2── Kiran  # played 2 matches together
```

---

## 🗺️ Google Maps Analogy

| Graph Concept | Google Maps Equivalent |
|---|---|
| Node | A place (city, landmark) |
| Edge | Road connecting two places |
| Directed edge | One-way street |
| Weighted edge | Distance or travel time |
| Path | Route from A to B |

---

## 📚 Key Terms

| Term | Simple Meaning | Analogy |
|---|---|---|
| **Degree** | Number of edges a node has | How many teammates Suriya has played with |
| **Path** | Sequence of nodes connected by edges | Route from A to B on Google Maps |
| **Connected Graph** | Every node can reach every other node | Every player is somehow connected to every other |
| **Neighbor** | Node directly connected by an edge | Suriya's direct teammates |
| **Cycle** | A path that starts and ends at the same node | A→B→C→A |

---

## ❓ Practice Questions & Answers

### Q1. In a directed graph, if there's an edge A→B, does B→A exist?
**No!** Directed edges are one-way. Like following someone on Instagram — they don't have to follow back.

---

### Q2. What do nodes and edges represent in Google Maps?
- **Nodes** = Places (A, B, C...)
- **Edges** = Roads connecting them (can be directed for one-way streets)

---

### Q3. What is the degree of node B?
```
A ── B ── C
     │
     D
```
**Answer: 3** — B connects to A, C, and D.

---

### Q4. Is this graph directed or undirected?
```
A → B → C
↑       │
└───────┘
```
**Answer: Directed** — arrows indicate direction. Also notice it forms a **cycle**: A→B→C→A.

---

### Q5. How many edges does this graph have? Is it connected?
```
A ── B
│    │
C ── D
```
**Edges: 4** (A-B, A-C, B-D, C-D)

**Connected: Yes!** — even though A and D are not directly connected, A can reach D via:
- A→B→D
- A→C→D

> ⚠️ **Connected ≠ Directly connected.** Connected just means every node can **reach** every other node through some path. Like Google Maps — no direct road needed, just a route that gets you there!

---

## 🐍 NetworkX Basics (No code writing needed — just understand!)

```python
import networkx as nx

G = nx.Graph()           # Undirected graph
G = nx.DiGraph()         # Directed graph

G.add_node('A')          # Add a node
G.add_edge('A', 'B')     # Add an edge (also adds nodes if missing)
G.add_edge('B', 'C', weight=5)  # Weighted edge

G.degree('B')            # Degree of node B
G.neighbors('B')         # Neighbors of node B
nx.is_connected(G)       # Is the graph connected?
```

---

## 💡 Important Behaviours to Remember

### add_edge() auto-creates nodes
No need to add nodes separately — edges create them automatically!
```python
G = nx.Graph()
G.add_edge('A', 'B')     # A and B didn't exist before — auto created!
print(list(G.nodes))     # ['A', 'B']
```

| Approach | Code |
|---|---|
| Explicit — nodes first, then edges | `G.add_node('A')` then `G.add_edge('A', 'B')` |
| Implicit — edges auto-create nodes | `G.add_edge('A', 'B')` directly |

> 📱 **Analogy:** Like a WhatsApp group — the moment you add a conversation between two people, both contacts get automatically added to your phonebook!

---

### ⚠️ Duplicate nodes/edges are silently ignored!
```python
G = nx.Graph()
G.add_edges_from([(1, 2), (1, 3)])  # Creates nodes 1,2,3 AND edges
G.add_node(1)        # Already exists — ignored!
G.add_edge(1, 2)     # Already exists — ignored!
print(G.number_of_nodes())  # 3 (not 4)
print(G.number_of_edges())  # 2 (not 3)
```
> ⚠️ **No error is raised** — NetworkX just quietly skips duplicates. Watch out for MCQs on this!

---

### nx.Graph() vs nx.DiGraph()

| | `nx.Graph()` | `nx.DiGraph()` |
|---|---|---|
| Type | Undirected | Directed |
| Edge A→B means B→A? | ✅ Yes (two-way) | ❌ No (one-way) |
| Use case | Friendships | Instagram follows |
| Connected check | `nx.is_connected(G)` | `nx.is_weakly_connected(G)` |

---

### ⚠️ None cannot be a node!
```python
G.add_node(None)   # ❌ NOT allowed — raises an error!
G.add_node(0)      # ✅ Fine — 0 is not None
G.add_node('')     # ✅ Fine — empty string is not None
```
> `None` is reserved by Python/NetworkX internally to check if optional arguments are set. Every other hashable object (int, string, tuple...) is valid as a node!

---

## 🔍 Practice Questions — DiGraph, Shortest Path, Remove Node

### Q1. DiGraph — in/out degree, successors, predecessors
```python
DG = nx.DiGraph()
DG.add_edges_from([(1, 2), (3, 1), (1, 4)])

print(DG.in_degree(1))           # 1 — only node 3 points TO 1
print(DG.out_degree(1))          # 2 — node 1 points to 2 and 4
print(list(DG.successors(1)))    # [2, 4] — who 1 points TO
print(list(DG.predecessors(1)))  # [3] — who points TO 1
```
> 📱 **Instagram analogy:** `successors` = people YOU follow. `predecessors` = people who follow YOU.

| Method | Meaning |
|---|---|
| `in_degree` | Edges coming IN to node |
| `out_degree` | Edges going OUT from node |
| `successors` | Nodes this node points TO |
| `predecessors` | Nodes that point TO this node |

---

### Q2. Shortest Path
```python
G = nx.Graph()
G.add_edges_from([(1, 2), (2, 3), (3, 4)])

print(nx.shortest_path(G, 1, 4))        # [1, 2, 3, 4]
print(nx.shortest_path_length(G, 1, 4)) # 3 (number of hops/edges)
```
> 🗺️ **Maps analogy:** Shortest path = the route. Length = number of roads you cross (not places you visit!). 4 places = 3 roads.

---

### Q3. Removing a Node removes its Edges too!
```python
G = nx.Graph()
G.add_edges_from([(1, 2), (1, 3), (2, 4)])
G.remove_node(1)

print(list(G.nodes))  # [2, 3, 4] — node 1 gone
print(list(G.edges))  # [(2, 4)] — edges (1,2) and (1,3) gone too!
```
> ⚠️ Node 3 still EXISTS even though it lost its only edge — it becomes an **isolated node** (degree 0). Removing a node ≠ removing its neighbors!

---

## 🎯 Quick Cheat Sheet

| Concept | Remember |
|---|---|
| Node | Thing |
| Edge | Relationship between things |
| Undirected | Two-way relationship |
| Directed | One-way relationship |
| Degree | Number of connections |
| Connected | Can reach everyone via some path |
| Weighted | Edges have cost/distance |