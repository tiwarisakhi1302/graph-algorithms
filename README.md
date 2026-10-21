# Graph Algorithms

Oct 21, 2026 · @Sakshi

## Cycle Detection in directed and undirected graph

### Undirected Graph

```cpp
class Solution {
    bool detectCyctelUndirectionalGraph(int i, unordered_map<int, vector<int>>&adj, vector<int> &vis, int parent){
        vis[i]=true;

        for(auto j : adj[i]){
            if(j!=parent){
                if(vis[j]) return true;
                else {
                    if(detectCyctelUndirectionalGraph(j, adj, vis, i)) return true;
                }
            }
        }

        return false;
    }
  public:
    bool isCycle(int V, vector<vector<int>>& edges) {
        // Code here
        unordered_map<int, vector<int>> adj;
        for(auto edge : edges){
            int u = edge[0];
            int v = edge[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        
        vector<int> vis(V, 0);
        for(int i=0; i<V; i++){
            if(!vis[i] && ){
                return true;
            }
        }
        
        return false;
    }
};
```

### Directed Graph

```cpp
class Solution {
    bool dfs(int i, vector<vector<int>>& adj, vector<int> &vis, vector<int> path){
        vis[i]=1;
        path[i]=1;
        
        for(auto j : adj[i]){
            if(vis[j] && path[j]) return true;
            else{
                if(dfs(j, adj, vis, path)) return true;
            }
        }
        return false;
    }
    bool bfs(int V, vector<vector<int>>& adj){
        vector<int> indegree(V, 0);
        for(auto v : adj){
            for(auto i : v){
                indegree[i]++;
            }
        }
        
        queue<int> q;
        
        int visted_nodes = 0;
        
        for(int i=0; i<V; i++){
            if(indegree[i]==0) q.push(i);
        }
        
        while(!q.empty()){
            auto f = q.front();
            q.pop();
            
            visted_nodes++;
            for(auto i : adj[f]){
                indegree[i]--;
                if(indegree[i]==0) q.push(i);
            }
        }
        
        if(visted_nodes != V) return true;
        return false;
    }
public:
    bool isCyclic(int V, vector<vector<int>>& adj) {
        vector<int> vis(V, 0), path(V, 0);
//         for(int i=0; i<V; i++){
//             if(!vis[i] && dfs(i, adj, vis, path)) return true;
//         }
//         return false;
        return bfs(V, adj);
    }
};
```

## All cycles length in directed and undirected graph

### Undirected Graph

```cpp
class Solution {
    vector<int> cycles;
    void dfs(int i, vector<vector<int>> &adj, vector<int> &len, int parent){
        for(auto j : adj[i]){
            if(j==parent) continue;
            if(len[j]==-1){
                len[j]=len[i]+1;
                dfs(j, adj, len, i);
            }else if(len[j] < len[i]){
                cycles.push_back(len[i]-len[j]+1);
            }
        }
    }
public:
    void cycleLengths(vector<vector<int>>& graph) {
        int V = graph.size();
        vector<int> len(V, -1);
        
        for(int i=0; i<V; i++){
            if(len[i]==-1) {
                len[i]=0;
                dfs(i, graph, len, -1);
            }
        }
        
        int ans = true;
        for(int i=0; i<cycles.size(); i++){
            cout<<cycles[i]<<endl;
        }
        
        return ans;
    }
};
```

### Directed Graph

```cpp
class Solution {
    vector<int> cycles;
    void dfs(int i, vector<int>& edges, vector<int> &len, vector<int> &path){
        path[i]=1;

        int j = edges[i];
        if(j!=-1){
            if(len[j]==-1){
                len[j]=len[i]+1;
                dfs(j, edges, len, path);
            }else if(path[j]){
                cycles.push_back(len[i]-len[j]+1);
            }
        }

        path[i]=0;
    }
public:
    int cycleLengths(vector<int>& edges) {
        int V = edges.size();
        
        vector<int> len(V, -1), path(V, 0);

        for(int i=0; i<V; i++){
            if(len[i]==-1) {
                len[i]=0;
                dfs(i, edges, len, path);
            }
        }

        int ans = -1;
        for(int i=0; i<cycles.size(); i++){
            cout<<cycles[i]<<endl;
        }

    }
};
```

## Bipartite Graph

**Method1 :** A graph who has odd length cycle

We have done this part in All cycle length detect Algo, just we need to find out if there is any odd length cycle.

**Method2:** Can we color the graph using 2 colours

```cpp
class Solution {
    vector<int> cycles;
    bool dfs(int i, vector<vector<int>> &adj, vector<int> &color, int parent){
        for(auto j : adj[i]){
            if(j==parent) continue;
            if(color[j]==-1){
                color[j] = (color[i]==1) ? 0 : 1;
                if(!dfs(j, adj, color, i)) return false;
            }else{
                if(color[i]==color[j]) return false;
            }
        }
        return true;
    }
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int V = graph.size();
        vector<int> color(V, -1);
        
        for(int i=0; i<V; i++){
            if(color[i]==-1) {
                if(!dfs(i, graph, color, -1)) return false;
            }
        }
        return true;
    }
};
```

## Topological Sort

```cpp
class Solution {
public:
    vector<int> topoSort(int V, vector<vector<int>>& adj) {
        vector<int> indegree(V, 0);
        for(auto v : adj){
            for(auto i : v){
                indegree[i]++;
            }
        }
        queue<int> q;
        for(int i=0; i<V; i++){
            if(indegree[i]==0)q.push(i);
        }
        vector<int> topo;
        while(!q.empty()){
            int f = q.front();
            q.pop();
            topo.push_back(f);
            
            for(auto i : adj[f]){
                indegree[i]--;
                if(indegree[i]==0){
                    q.push(i);
                }
            }
        }
        return topo;
    }
};
```

## DSU

- Its used for undirected graph
- Tracks connected components
- Designed to find strongly connected components in undirected graph. In case of directed graph we use Kosaraju or Trajan’s algo
- Used for cycle detection, building MST(Minimum Spanning Tree), finding strongly connected components.

```cpp
class DisjointSet {
    vector<int>parent, size;
public:
    DisjointSet(int n) {
        parent.resize(n);
        size.resize(n, 1);
        for(int i=0; i<n; i++) parent[i]=i;
    }

    int find(int x) {
        if(parent[x]==x){
            return x;
        }
        return find(parent[x]);
    }

    void unionBySize(int u, int v) {
        int par_u = find(u);
        int par_v = find(v);
        
        if(par_u==par_v) return;
        
        if(size[par_v] <= size[par_u]){
            parent[par_v]=par_u;
            size[par_u]+=size[par_v];
        }else{
            parent[par_u]=par_v;
            size[par_v]+=size[par_u];
        }
    }

    bool connected(int u, int v) {
        return (find(u)==find(v));
    }

    int componentSize(int x) {
        return size[find(x)];
    }
};
```

## Kosarju’s Algorithm

- Help to find Strongly connected component in the directed graph.
- Algo
  1. **Step 1 :** Find the topo-sort of the graph.
  2. **Step 2 :** Find the transpose of the graph. Make a new graph with the reverse edges.
  3. **Step 3 :** Now apply the dis on transposed graph in the order of topo-sort.

```cpp
class Solution {
    void topofill(int i, vector<vector<int>>& adj, vector<int> &vis, stack<int> &st){
        vis[i]=1;
        for(auto j : adj[i]){
            if(!vis[j]){
                topofill(j, adj, vis, st);
            }
        }
        st.push(i);
    }
    void dfsTransposeGraph(int i, vector<vector<int>> &adj, vector<int> &vis){
        vis[i]=1;
        for(auto j : adj[i]){
            if(!vis[j]){
                dfsTransposeGraph(j, adj, vis);
            }
        }
    }
public:
    int kosaraju(int V, vector<vector<int>>& adj) {
        stack<int> topo;
        vector<int> vis(V, 0);
        for(int i=0; i<V; i++){
            if(!vis[i]){
                topofill(i, adj, vis, topo);
            }
        }
        vector<vector<int>> trans_adj(V);
        for(int i=0; i<V; i++){
            for(auto j : adj[i]){
                trans_adj[j].push_back(i);
            }
        }
        vis.assign(V, 0);
        int count = 0;
        while(!topo.empty()){
            int t = topo.top();
            topo.pop();
            if(!vis[t]){
                count++;
                dfsTransposeGraph(t, trans_adj, vis);
            }
        }
        return count;
    }
};
```

## Minimum Spanning Tree

- Valid for undirected weighted connected graph

### Kruskal’s Algorithm

- Sort the eggs in the increasing order of weights
- Just need to iterate over edges and if some edge is forming cycle we cannot include that.
- We can do this using dsu because in that we can know if the edge is forming cycle or not using union function.
- **Time Complexity :** O(ElogE)
- **Space Complexity :** O(V)

```cpp
   class Solution {
    static bool custom_sort(vector<int> &v1, vector<int> &v2){
        if(v1[2]!=v2[2]) return v1[2]<v2[2];
        return false;
    }
    class disjointSet{
        vector<int> parent, size;
    public:
        disjointSet(int n){
            parent.resize(n);
            size.resize(n, 1);
            for(int i=0; i<n; i++) parent[i]=i;
        }
        int find(int i){
            if(parent[i]==i) return i;
            else return parent[i]=find(parent[i]);
        }
        bool unionBySize(int u, int v){
            int par_u = find(u);
            int par_v = find(v);
            
            if(par_u==par_v) return false;
            
            if(size[par_u]>size[par_v]){
                parent[par_v]=par_u;
                size[par_u]+=size[par_v];
            }else{
                parent[par_u]=par_v;
                size[par_v]+=size[par_u];
            }
            return true;
        }
    };
public:
    int spanningTree(int V, vector<vector<int>>& edges) {
        sort(edges.begin(), edges.end(), custom_sort);
        
        disjointSet dsu(V);
        
        long long mst = 0;
        
        for(auto edge : edges){
            int u = edge[0], v = edge[1], w = edge[2];
            
            if(dsu.unionBySize(u, v)){
                mst+=w;
            }
        }
        
        return mst;
    }
};
```

### Prim’s Algorithm

- We will make a priority queue with minimum element at the top (distance from its parent, vertex) and visited array in which we will keep if we have visited this element.
- We will add one vertex ex:{0,0} there is no cost from its parent as this is starting point.
- We will add the top cost to most and push its adjacent to the heap if they are to the node which not visited.
- Make a continue call in the heap by checking the node if visited as the same node can be pushed by two or more different elements which is not visited yet.

```cpp
class Solution {
public:
    int spanningTree(int V, vector<vector<int>>& edges) {
        unordered_map<int, vector<pair<int, int>>> adj;
        
        for(auto edge : edges){
            int u=edge[0], v=edge[1], w=edge[2];
            adj[u].push_back({v, w});
            adj[v].push_back({u, w});  
        }
        
        vector<int> visited(V, 0);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minh;
        
        minh.push({0, 0});
        
        
        long long mst=0;
        
        while(!minh.empty()){
            auto f = minh.top();
            int w = f.first, node = f.second;
            minh.pop();
            
            if(visited[node]) continue;
            
            visited[node]=1;
            mst+=w;
            
            for(auto j : adj[node]){
                if(!visited[j.first]) minh.push({j.second, j.first});
            }
                
        }
        
        return mst;
    }
};
```

## Shortest Path Algorithms

- Dijkstra and Bellmen Ford Algos are used to find the shorted path from single source.

### Dijkstra Algorithm

- It is used to find the shortest path from single source.
- It doesn’t work for the negative edge weights.

```cpp
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        // Code here
        unordered_map<int, vector<vector<int>>> adj;
        for(auto edge : edges){
            int u = edge[0], v = edge[1], w = edge[2];
            adj[u].push_back({v, w});
            adj[v].push_back({u, w});
        }
        vector<int> dist(V, INT_MAX), visited(V, 0);
        dist[src]=0;

        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minh;

        minh.push({0, src});

        while(!minh.empty()){
            auto t = minh.top();
            minh.pop();

            int node = t.second;
            if(visited[node]) continue;
            
            visited[node]=1;

            for(auto v : adj[node]){
                int child = v[0];
                int d = v[1];                    
                if(dist[child] > dist[node]+d){
                    dist[child]=dist[node]+d;
                    minh.push({dist[child], child});
                }
            }
        }
        for(int i=0; i<V; i++) if(dist[i]==INT_MAX) dist[i]=-1;
        return dist;
    }
};
```

### Bellman Ford Algorithm

- Work’s for both directed and undirected graph.
- It is single source shortest path algorithm.
- It works with negative edge weights
- In case of negative weight cycle the short path doesn’t exist as it will keep on decreasing on each traversal.
- **Time Complexity :** O(V^E)
- **Space Complexity :** O(V)
- Dijkstra Fails on graph with negative edges as it doesn’t revisit the visited nodes. If a shorter path exist through a longer route with negative edges. It will fail to handle this.
- Algo :

**Step1 :** Check this V-1 time for each edge

```cpp
if(dist[v] > dist[u] + w){
    dist[v]=dist[u]+w;
}
```

**Step2 :** For all edges check if this one more time if the updation happens then it has neg cycle.

Add this as the current node should be visited before comparing it with the child As the upper condition will work for neg edges.

```cpp
if (dist[u] == 100000000) continue;
```

```cpp
class Solution {
public:
    vector<int> bellmanFord(int V, vector<vector<int>>& edges, int src) {
        vector<int> dist(V, 100000000);
        dist[src]=0;
        bool negCycle = false;
        for(int i=0; i<V; i++){
            for(auto edge : edges){
                int u = edge[0], v = edge[1], w = edge[2];
                if (dist[u] == 100000000) continue;
                if(dist[v] > dist[u] + w){
                    if(i==V-1) negCycle = true;
                    dist[v]=dist[u]+w;
                }
            }
        }
        if(negCycle) return {-1};
        return dist;
    }
};
```

### Floyd Warshall Algorithm

- It is multiple source shortest path algorithm.
- It works with negative edge weights and can also detect negative cycles.
- If we use Bellman Ford for multiple sources then TC will O(V\*V\*E) which is too slow.
- **Time Complexity :** O(V^3)
- **Space Complexity :** O(V^2)

```cpp
class Solution {
public:
    vector<vector<int>> floydWarshall(int n, vector<vector<int>>& edges) {
        vector<vector<int>> dist(n, vector<int>(n, INT_MAX));
        for(int i=0; i<n; i++) dist[i][i]=0;
        for (auto& e : edges) dist[e[0]][e[1]] = min(dist[e[0]][e[1]], e[2]);
        
        for(int k=0; k<n; k++){
            for(int i=0; i<n; i++){
                for(int j=0; j<n; j++){
                    if(dist[i][k]==INT_MAX || dist[k][j]==INT_MAX) continue;
                    
                    dist[i][j]=min(dist[i][j], dist[i][k]+dist[k][j]);
                }
            }
        }
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++) if(dist[i][j]==INT_MAX) dist[i][j]=-1;
        }
        return dist;
    }
};
```

**Follow-up:** How would you detect a negative cycle using the same table?

**Ans :** After the three loops finish, a negative cycle shows up as a negative value on the diagonal: if any dist\[i\]\[i\] < 0, node i lies on a negative cycle.

```cpp
bool hasNegativeCycle(vector<vector<int>>& dist, int n) { 
    for (int i = 0; i < n; i++) 
        if (dist[i][i] < 0) return true; 
    return false; 
}
```

## Graph Question Patterns

| # | Pattern | Use |
| --- | --- | --- |
| 1 | Normal traversal question | DFS/BFS → If it asks for shortest path in an unweighted graph, use BFS. |
| 2 | Minimum / Shortest path — Unweighted | BFS |
| 2 | Minimum / Shortest path — Weighted (no negative weight) | Dijkstra (priority queue) |
| 2 | Minimum / Shortest path — Negative weights | Bellman-Ford |
| 3 | If ordering/dependency matters | Topological Sort |
| 4 | Grouping / connected components | Union-Find (DSU) |
| 5 | Convert to tree + find minimum cost to connect all nodes | MST (Prim’s / Kruskal’s) |
| 6 | Exactly two groups / checking if graph can be divided into 2 sets | Bipartite graph (BFS/DFS coloring) |
