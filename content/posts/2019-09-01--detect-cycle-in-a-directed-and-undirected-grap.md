---
title: Detect cycle in a directed and undirected graph
date: "2019-09-01T10:00:00Z"
template: "post"
draft: false
slug: "/posts/detect-cycle-in-a-directed-and-undirected-grap/"
category: "programming"
tags:
  - "python"
  - "algorithm"
description: "Cycle in a graph means that it has path which certain vertex is connected via other vertices. Directed and undirected graph have different type of cycle as the following image, and algorithm to detect cycle for them are also different. Here is to introduce recursive version of depth-first search implementation."
socialImage: "/media/detect-cycle-in-a-directed-and-undirected-grap/has_cycle_directed_graph.jpg"
---

Cycle in a graph means that it has path which certain vertex is connected via other vertices. Directed and undirected graph have different type of cycle as the following image, and algorithm to detect cycle for them are also different. Here is to introduce recursive version of depth-first search implementation.

![graph type](/media/detect-cycle-in-a-directed-and-undirected-grap/graph_type.png)

## for undirected graph

When control to traverse a target graph reaches certain vertex and if the vertex was already found before, the graph has a cycle. If it is not found before, the control moves to done to neighbor vertices of the vertex. But one vertex ago is ignored as a neighbor.

Result of detection will propagate from a vertex in which a cycle found, or leaf vertex.

`gist:8ukkari/ef1c405ca17b3bd49d6e74567636ab2b?file=has_cycle_for_directed_graph.py&highlights=8`

**behavior with a cycle:**

![has cycle in undirected graph](/media/detect-cycle-in-a-directed-and-undirected-grap/has_cycle_undirected_graph.png)

## for directed graph

When control to traverse a target graph reaches certain vertex and if a subtree rooted from the vertex is not confirmed that it does not have any cycles but it was found before, the graph has a cycle. If it is not, the control moves to done to neighbor vertices of the vertex.

`gist:ef1c405ca17b3bd49d6e74567636ab2b`

**behavior with a cycle:**

![has cycle in directed graph graph](/media/detect-cycle-in-a-directed-and-undirected-grap/has_cycle_directed_graph.png)

**behavior without cycles:**

![has no cycle in directed graph graph](/media/detect-cycle-in-a-directed-and-undirected-grap/has_no_cycle_directed_graph.png)
