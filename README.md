# Is graph bipartite
## https://leetcode.com/problems/is-graph-bipartite

There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:

**There are no self-edges (graph[u] does not contain u).**
There are no parallel edges (graph[u] does not contain duplicate values).
**If v is in graph[u], then u is in graph[v] (the graph is undirected).**
**The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.**
A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

**Return true if and only if it is bipartite.**

![Is Graph Bipartite](example.JPG?raw=true)

## Approach :
Start BFS from vertex 0, level of vertex 0 will be 0.

Add unvisited neighbors to the queue, the neighbors will be visited at `level+1`.

If we found any node in the queue which is already visited but whose level is different than its already visited level, then it means the graph has a odd length cycle and its definitely not a bipartite graph. 

**Since the graph can have disconnected components we will have to check all the components, if any component have odd length cycle, then the graph is not a bipartite graph.**

# Implementation 1 : BFS
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] level = new int[n];
        Arrays.fill(level, -1);
        Set<Integer> visited = new HashSet<>();
        for(int i = 0; i < n; i++) {
         if(!visited.contains(i)) {
             boolean result = isBipartite(i, graph, level, visited);
             if(result == false)
                 return false;
         }   
        }
        return true;
    }
    
    private boolean isBipartite(int vertex, int[][] graph, int[] level, Set<Integer> visited) {
        Queue<int[]> q = new ArrayDeque<>();
        q.add(new int[]{vertex,0});
        level[vertex] = 0;
        
        while(!q.isEmpty()) {
            int[] node = q.remove();
            if(visited.contains(node[0]) && level[node[0]] != node[1])
                return false;
            if(!visited.contains(node[0])) {
                visited.add(node[0]);
                level[node[0]] = node[1];
            }
            // add to queue unvisited neighbors
            int[] neighbors = graph[node[0]];
            for(int neighbor : neighbors) {
                    if(!visited.contains(neighbor)) {
                        q.add(new int[]{neighbor, node[1] + 1});
                    }
            }
        }
        return true;
    }
}

```

### Implementation 2 : BFS (using only level array, we can get both, level of a vertex and whether a vertex was already visited or not)

If visited[i] is not equal to -1 it means the vertex is already visited at level visited[i].

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] level = new int[n];
        Arrays.fill(level, -1);
        
        for(int i = 0; i < n; i++) {
         if(level[i] == -1) {
             boolean result = isBipartite(i, graph, level);
             if(result == false)
                 return false;
         }   
        }
        return true;
    }
    
    private boolean isBipartite(int vertex, int[][] graph, int[] level) {
        Queue<int[]> q = new ArrayDeque<>();
        q.add(new int[]{vertex,0});
        level[vertex] = 0;
        
        while(!q.isEmpty()) {
            int[] node = q.remove();
            if(level[node[0]] != -1 && level[node[0]] != node[1])
                return false;
            if(level[node[0]] == -1) {
                level[node[0]] = node[1];
            }
            // add to queue unvisited neighbors
            int[] neighbors = graph[node[0]];
            for(int neighbor : neighbors) {
                    if(level[neighbor] == -1) {
                        q.add(new int[]{neighbor, node[1] + 1});
                    }
            }
        }
        return true;
    }
}
```

# References :
https://www.youtube.com/watch?v=ZBhZ1DXGrhA (Hindi, good explanation)
