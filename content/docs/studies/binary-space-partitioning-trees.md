---
title: "Binary Trees Applied to Procedural Generation"
weight: 1
---

{{< wip >}}
Updates Soon!
{{< /wip >}}

# Binary Trees Applied to Procedural Generation

While standard binary trees allow efficient searching and ordering of scalar data, partitioning trees generalize this concept to multidimensional space. It acts simultaneously as a hierarchical search structure and a precise geometric representation. The core mechanism involves recursive subdivision by hyperplanes. Unlike grid-based approaches, this method has no restriction on the orientation of cuts, allowing for the exact representation of polytopes and convex sets.

A hyperplane is the subspace of dimension that divides the space into two half-spaces. Mathematically, in {{< katex >}}\mathbb{R}^d{{< /katex >}}, it is defined by the implicit equation {{< katex >}} n \cdot x + d = 0 {{< /katex >}}, where {{< katex >}}n{{< /katex >}} is the normal vector and {{< katex >}}d{{< /katex >}} is the distance from origin. The classification of any point {{< katex >}}p{{< /katex >}} is determined by the dot product. If {{< katex >}}n \cdot p + d > 0{{< /katex >}}, the point is in the front child. Conversely, if the result is negative, it is in the back child.

## Procedural Application

If we have the intent to generate the map of a dungeon, the use of a partitioning tree is appropriate. The root node represents the entire map's canvas and the leaf nodes become potential zones for rooms. Because the split is binary, rooms generated in leaf nodes will never unintentionally overlap, creating mathematically guaranteed disjoint sets.

However, in practical engineering, the concept of disjoint sets faces challenges due to floating point arithmetic. Since computers approximate real numbers, a situation where {{< katex >}}0.0000001 \neq 0{{< /katex >}} can occur, creating "cracks" in the geometry. Robust implementations must replace direct equality checks with an epsilon threshold comparison. Furthermore, edge cases such as co-planar geometry, where objects lie exactly on the cut, require specific handling logic to assign the object to one side or duplicate it across both.

As a recursive limit, a size heuristic is necessary, such as stopping the split if a node is smaller than a certain unit size. For dungeon maps, the hyperplane is typically limited to axis-aligned cuts using orthogonal X and Y axes. This specific subset is known as a k-D Tree, which simplifies the dot product vector calculation into a fast scalar comparison, drastically improving generation speed for grid-based layouts.

To control the dungeon's architecture, we avoid uniform distribution for the split positions. Applying a Gaussian distribution allows us to bias the generation, creating larger, denser rooms at the center and smaller, sparser rooms at the peripheries, simulating a more organic structure as everything in the real life have tendency to follow non-constant patterns.

{{< plotly id="gaussian-viz" >}}
[
  {
    "x": [1, 10],
    "y": [1, 1],
    "type": "scatter",
    "mode": "lines",
    "name": "Uniform",
    "line": { "color": "#ff5555", "width": 4 },
    "hoverinfo": "none"
  },
  {
    "x": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    "y": [0.1, 0.5, 2, 5, 9, 5, 2, 0.5, 0.1, 0],
    "type": "scatter",
    "mode": "lines+markers",
    "name": "Gaussian",
    "line": { "shape": "spline", "color": "#00ffcc", "width": 4 },
    "marker": { "size": 6, "color": "#00ffcc" }
  },
  {
    "layout": {
        "title": "Probability Distribution",
        "xaxis": { 
            "title": "Distance from Center", 
            "range": [1, 10], 
            "fixedrange": true 
        },
        "yaxis": { 
            "title": "Spawn Chance", 
            "range": [0, 10], 
            "fixedrange": true 
        },
        "showlegend": true,
        "legend": { "x": 0.7, "y": 0.9 },
        "dragmode": false
    }
  }
]
{{< /plotly >}}

{{< details "Why Gaussian vs. Uniform Noise?" >}}
Uniform distribution (Standard Random) maximizes entropy or similar. In procedural topology, max entropy results in a "Swiss Cheese" layout—unstructured, sprawling, and lacking a focal point.

By using a Gaussian (Normal) distribution, we mathematically force a hierarchy. The "Bell Curve" ensures that ~68% of the splits happen within one standard deviation of the center. This naturally creates a density distribution, mimicking organic structures without requiring complex heuristic scripts.
{{< /details >}}

### Structural Limitations and Topology

Beyond numerical precision, the method imposes distinct topological constraints. By definition, a binary tree generates a hierarchical structure without cycles. In the dungeon example, this results in a branching map where the player must constantly backtrack to explore different paths, lacking the interconnected loops typical of organic level design.

Furthermore, axis-aligned partitioning inherently biases the layout towards long, strictly orthogonal corridors. Consequently, the BSP algorithm must be viewed strictly as a space allocation tool, not a complete level generator.
