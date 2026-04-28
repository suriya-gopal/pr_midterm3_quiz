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
