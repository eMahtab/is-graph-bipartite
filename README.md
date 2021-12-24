# Is graph bipartite
## https://leetcode.com/problems/is-graph-bipartite


# Implementation 1 : BFS
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int vertices = graph.length;
        int[] visited = new int[vertices];
        Arrays.fill(visited, -1);
        
        for(int vertex = 0; vertex < vertices; vertex++) {
            if(visited[vertex] == -1) {
                boolean isBipartite = isBipartite(graph, vertex, visited);
                if(!isBipartite)
                    return false;
            }
        }
        return true;
    }
    
    private boolean isBipartite(int[][] graph, int vertex, int[] visited) {
        Queue<int[]> q = new ArrayDeque<>();
        q.add(new int[]{vertex, 0});
        
        while(!q.isEmpty()) {
            int[] node = q.remove();
            int v = node[0];
            if(visited[v] != -1) {
                if(visited[v] != node[1])
                    return false;
            } else {
                visited[v] = node[1];
            }
            
            for(int neighbor : graph[v]) {
                if(visited[neighbor] == -1) {
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
