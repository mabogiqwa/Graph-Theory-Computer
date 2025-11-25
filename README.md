# Graph Theory Computer

## Project Overview
An interactive web-based tool for drawing, analyzing, and comparing graphs. This tool provides comprehensive graph theory analysis including isomorphism checking, planarity testing, and various graph properties.

**Created with:** Claude Sonnet (Anthropic AI)

---

## Features Overview

### 1. Graph Drawing Interface
- **Dual Canvas System**: Draw and compare two graphs side by side
- **Three Interaction Modes:**
  - **Add Vertex**: Click anywhere on the canvas to place a vertex
  - **Add Edge**: Click two vertices sequentially to connect them
  - **Delete**: Click vertices to remove them (edges connected to them are also removed)

### 2. Graph Isomorphism Checker
Tests whether two graphs are structurally identical (same connectivity pattern, different layout).

**Algorithm Details:**
- Quick checks: vertex count, edge count, degree sequence
- For graphs ≤8 vertices: Exhaustive permutation testing
- For graphs >8 vertices: Partial verification with degree sequence matching

**Example Use Cases:**
- Compare two different drawings of the same graph structure
- Verify if two molecular structures are equivalent
- Educational tool for understanding graph isomorphism

### 3. Planarity Testing
Determines if a graph can be drawn on a plane without edge crossings.

**Based on Kuratowski's Theorem:** A graph is planar if and only if it does not contain K₅ or K₃,₃ as a subgraph.

**Implementation:**
- Euler's formula quick check: e ≤ 3v - 6
- K₅ detection (complete graph on 5 vertices)
- K₃,₃ detection (complete bipartite graph)

**Classic Examples:**
- K₄: Planar ✓
- K₅: Non-planar (too many edges)
- K₃,₃: Non-planar (utility graph problem)

### 4. Graph Properties Analysis

#### a) Complete Graph
- **Definition**: Every pair of vertices is connected by an edge
- **Formula**: A complete graph Kₙ has n(n-1)/2 edges
- **Algorithm**: Checks all vertex pairs for edges

#### b) Connected Graph
- **Definition**: There exists a path between every pair of vertices
- **Algorithm**: Depth-First Search (DFS) from any vertex
- **Time Complexity**: O(V + E)

#### c) Euler Cycle
- **Definition**: A cycle that visits every edge exactly once
- **Euler's Theorem**: A connected graph has an Euler cycle if and only if every vertex has even degree
- **Three Possible Outcomes:**
  - Euler Cycle: All vertices have even degree
  - Euler Path (not cycle): Exactly 2 vertices have odd degree
  - Neither: More than 2 vertices have odd degree

#### d) Hamiltonian Circuit
- **Definition**: A cycle that visits every vertex exactly once
- **NP-Complete Problem**: No known polynomial-time algorithm
- **Detection Methods:**
  - Small graphs (≤8 vertices): Backtracking algorithm to find actual circuit
  - Large graphs: Dirac's Theorem - if every vertex has degree ≥ n/2, then Hamiltonian
- **Note**: Finding Hamiltonian circuits is computationally hard for large graphs

#### e) Chromatic Number (χ)
- **Definition**: Minimum number of colors needed to color vertices so no adjacent vertices share a color
- **Applications**: Scheduling, register allocation, map coloring
- **Algorithm:**
  - Check if bipartite (χ = 2)
  - Check if complete (χ = n)
  - Greedy coloring with multiple vertex orderings:
    - Natural order
    - Degree-descending order (Welsh-Powell)
    - Random order
  - Return minimum colors found

**Special Cases:**
- Empty graph: χ = 0
- Single vertex: χ = 1
- Bipartite graphs: χ = 2
- Complete graphs: χ = n
- Cycle graphs: χ = 2 (even cycles) or 3 (odd cycles)

---

## User Interface Guide

### Control Modes

**Mode Selection Buttons:**
- Click to switch between Add Vertex, Add Edge, and Delete modes
- Active mode is highlighted in purple

**Graph Controls:**
- Clear Graph 1/2: Removes all vertices and edges from one graph
- Clear All: Resets both graphs completely

### Analysis Buttons

**Check Isomorphism**
- Compares Graph 1 and Graph 2
- Shows result with explanation
- Green = Isomorphic, Red = Not Isomorphic

**Check Planarity**
- Analyzes both graphs independently
- Shows which graph is planar/non-planar and why
- Identifies specific subgraphs (K₅ or K₃,₃) if non-planar

**Analyze Properties**
- Comprehensive analysis of both graphs
- Displays all properties in organized sections
- Color-coded results (green = yes, red = no)

### Visual Elements

**Vertices:**
- Circular nodes with numbers (0, 1, 2, ...)
- Purple when normal, red when selected (in edge mode)
- Radius: 20 pixels

**Edges:**
- Purple lines connecting vertices
- Width: 2 pixels
- Undirected (no arrows)

---

## Technical Implementation

### Technologies Used
- **HTML5**: Canvas API for drawing
- **CSS3**: Modern styling with gradients and animations
- **Vanilla JavaScript**: No external dependencies

### Key Data Structures

```javascript
// Graph storage
graphs = {
    1: { 
        vertices: [{x, y, id}, ...],
        edges: [[v1, v2], ...]
    },
    2: { ... }
}

// Adjacency matrix representation
adj[i][j] = 1 if edge exists, 0 otherwise
```

### Core Algorithms

**Graph Isomorphism (Permutation Testing):**
```
For each permutation P of vertices in Graph 2:
    Check if adj1[i][j] == adj2[P[i]][P[j]] for all i,j
    If match found, return true
Return false
```

**DFS for Connectivity:**
```
visited = [false, false, ...]
DFS(vertex 0)
Return true if all vertices visited
```

**Hamiltonian Backtracking:**
```
path = [start_vertex]
visited = [start_vertex marked true]

FindPath(current, path, visited):
    If path.length == n:
        Return edge exists from last to first
    For each neighbor of current:
        If not visited:
            Add to path, mark visited
            If FindPath succeeds, return true
            Backtrack
    Return false
```

**Greedy Coloring:**
```
For each vertex v:
    usedColors = colors of all neighbors
    Assign v the smallest color not in usedColors
```

### Algorithm Complexity Analysis

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Isomorphism (n≤8) | O(n! × n²) | O(n²) |
| Planarity (K₅/K₃,₃) | O(n⁶) | O(n²) |
| Connectivity (DFS) | O(V + E) | O(V) |
| Complete Check | O(V²) | O(1) |
| Euler Cycle | O(V + E) | O(V) |
| Hamiltonian (n≤8) | O(n! × n) | O(n) |
| Chromatic Number | O(V² × attempts) | O(V) |

*Note: V = vertices, E = edges*

---

## Example Scenarios

### Scenario 1: Complete Graph K₄
**Draw:**
- 4 vertices arranged in a square
- Connect all vertices to each other (6 edges total)

**Expected Results:**
- Complete: YES
- Connected: YES
- Planar: YES
- Euler Cycle: NO (all vertices have odd degree 3)
- Hamiltonian Circuit: YES
- Chromatic Number: 4

### Scenario 2: Cycle Graph C₅
**Draw:**
- 5 vertices in a pentagon
- Connect each vertex to its two neighbors (5 edges)

**Expected Results:**
- Complete: NO
- Connected: YES
- Planar: YES
- Euler Cycle: YES (all vertices have even degree 2)
- Hamiltonian Circuit: YES
- Chromatic Number: 3 (odd cycle)

### Scenario 3: Petersen Graph
**Draw:**
- Outer pentagon: 5 vertices connected in cycle
- Inner pentagram: 5 vertices forming a star
- Connect outer to inner (15 edges total)

**Expected Results:**
- Complete: NO
- Connected: YES
- Planar: NO (contains K₃,₃)
- Euler Cycle: NO (all vertices have degree 3)
- Hamiltonian Circuit: YES
- Chromatic Number: 3

### Scenario 4: Bipartite Graph K₃,₃
**Draw:**
- 3 vertices on left, 3 on right
- Connect each left vertex to all right vertices (9 edges)

**Expected Results:**
- Complete: NO
- Connected: YES
- Planar: NO (is K₃,₃ itself)
- Euler Cycle: NO (all vertices have odd degree 3)
- Hamiltonian Circuit: YES
- Chromatic Number: 2 (bipartite)

---

## Limitations and Considerations

### Computational Limits
- **Isomorphism**: Only handles graphs with ≤8 vertices exactly; larger graphs use heuristics
- **Hamiltonian Circuit**: Exact detection limited to ≤8 vertices (NP-complete problem)
- **Planarity**: Checks for K₅ and K₃,₃ only; doesn't detect all graph minors

### Known Issues
- Large graphs (>20 vertices) may have performance issues
- Chromatic number uses greedy heuristic, not guaranteed optimal
- No support for directed graphs or weighted edges
- No graph import/export functionality
- **⚠️ Bipartite Detection Bug**: The bipartite checking algorithm contains a critical logical bug that produces false positives. The algorithm incorrectly identifies graphs as bipartite even when vertices within the same partition set have edges between them. By definition, a bipartite graph must have all edges connecting vertices from different sets only - no edges should exist between vertices in the same partition. The current implementation fails to properly validate this constraint, leading to incorrect bipartite classifications.

### Browser Compatibility
- Requires HTML5 Canvas support
- Tested on modern browsers (Chrome, Firefox, Safari, Edge)
- Mobile touch support available but optimized for desktop

---

## Educational Applications

### Teaching Topics
- Graph Theory Fundamentals: Vertices, edges, adjacency
- Graph Isomorphism: Structural equivalence vs. visual representation
- Planarity: Kuratowski's theorem, graph minors
- Eulerian Paths: Seven Bridges of Königsberg problem
- Hamiltonian Cycles: Traveling Salesman Problem introduction
- Graph Coloring: Four Color Theorem, scheduling problems

### Classroom Activities
- Compare different drawings of the same graph
- Explore why K₅ and K₃,₃ are non-planar
- Find Hamiltonian circuits in small graphs
- Calculate chromatic numbers for various graph families
- Investigate the relationship between graph properties

---

## Future Enhancement Ideas

### Potential Features
- **Graph Import/Export**: Save and load graphs in standard formats (JSON, GraphML)
- **Directed Graph Support**: Add arrows and directed edge analysis
- **Weighted Graphs**: Edge weights with shortest path algorithms
- **Animation**: Visualize algorithm execution step-by-step
- **More Algorithms:**
  - Minimum spanning tree (Kruskal, Prim)
  - Shortest path (Dijkstra, Bellman-Ford)
  - Maximum flow (Ford-Fulkerson)
- **Graph Generators**: Automatically create common graph types
- **Better Hamiltonian Detection**: Approximation algorithms for larger graphs
- **Improved UI**: Drag vertices, curved edges, better mobile support
- **Fix Bipartite Detection**: Implement proper two-coloring algorithm with validation

### Advanced Features
- Graph embedding/layout algorithms (force-directed, hierarchical)
- Subgraph matching and pattern recognition
- Graph database integration
- Collaborative multi-user editing
- Export visualizations as images or videos

---

## Troubleshooting

### Common Issues

**Issue:** "Edges not appearing"
- **Solution**: Make sure you're in "Add Edge" mode and clicking exactly on vertex centers

**Issue:** "Isomorphism check says 'too large'"
- **Solution**: Graphs with >8 vertices use heuristics; reduce vertex count for exact results

**Issue:** "Canvas not responding"
- **Solution**: Refresh the page; browser may have limited canvas performance

**Issue:** "Hamiltonian result uncertain"
- **Solution**: For graphs >8 vertices, tool cannot verify exactly; try smaller graphs

**Issue:** "Bipartite detection incorrect"
- **Solution**: Known bug - the algorithm may incorrectly identify non-bipartite graphs as bipartite. Manually verify that no edges exist between vertices in the same partition set.

---

## Credits and References

### Graph Theory Resources
- **Kuratowski's Theorem**: K. Kuratowski (1930), "Sur le problème des courbes gauches en topologie"
- **Euler's Theorem**: Leonhard Euler (1736), Seven Bridges of Königsberg
- **Dirac's Theorem**: G. A. Dirac (1952), Hamiltonian circuits

### Algorithm References
- Graph Isomorphism: Permutation-based approach
- DFS/BFS: Classic graph traversal algorithms
- Greedy Coloring: Welsh-Powell algorithm variant

---

## License and Usage

This tool is created for educational purposes. Feel free to use, modify, and distribute for learning and teaching graph theory.

No warranties provided - use at your own risk for academic and educational purposes.

---

## Version History
- **v1.0**: Initial isomorphism checker
- **v2.0**: Added planarity testing
- **v3.0**: Added comprehensive property analysis (complete, connected, Euler, Hamiltonian, chromatic number)
- **v3.1**: Documentation created
- **v3.2**: Documentation updated to reflect bipartite detection bug

**Created**: 2024  
**Last Updated**: 2024  
**Developed with**: Claude Sonnet (Anthropic AI)

---

## About

Web user interface that determines the various properties of graphs

**Website**: [mabogiqwa.github.io/Graph-Theory-Computer/](https://mabogiqwa.github.io/Graph-Theory-Computer/)

### Repository Information
- **Stars**: 0 stars
- **Watchers**: 0 watching
- **Forks**: 0 forks
- **Releases**: No releases published
- **Packages**: No packages published
- **Deployments**: 2 (github-pages)
- **Languages**: HTML 100.0%

---

*© 2025 GitHub, Inc.*
