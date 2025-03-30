# DFS

## adj List
```cpp
bool vis [n] = {};
vector<vector<ll>>v(n);
void dfs(int node){
vis[node] = true;
for(ll nx = 0; nx<v[node].sz;nx++){
	if(!vis[v[node][nx]]){
	dfs(v[node][nx]);
	}
  }
}
```

## adj Metrix
```cpp
bool vis [n] = {};
vector<vector<ll>>v(n);
	void dfs(int node){
		vis[node] = true;
		for(ll nx = 0; nx<v[node].sz;nx++){
			if(!vis[v[node][nx]] and v[node][i]==1){
				dfs(i);
			}
		}
	}
```

# BFS

```cpp
bool vis[n] = {};
queue<ll>q;
vector<vector<ll>>v(n);
void bfs(int node){
	vis[node] = true;
	q.push(node);
	while(!q.empty()){
		ll cur = q.front();
		q.pop();
		for(ll nx = 0; nx<v[cur].sz;nx++){
			if(!vis[v[cur][i]]){
				q.push(v[cur][i]);
			}
		}
	}
}
```

## Shortest Path
```cpp
bool vis[n] = {};
queue<ll>q;
vector<vector<ll>>v(n);
vector<ll>dis(n,0);
void bfs(int node){
	vis[node] = true;
	q.push(node);
	while(!q.empty()){
		ll cur = q.front();
		q.pop();
		for(ll nx = 0; nx<v[cur].sz;nx++){
			if(!vis[v[cur][i]]){
				q.push(v[cur][i]);
				dis[i] = dis[cur] + 1;
			}
		}
	}
}
```

# Cycle Detection

```cpp
bool dfs(int v) {
    visited[v] = 1;
    
    for (int neighbor : adj[v]) {
        if (visited[neighbor] == 0 && dfs(neighbor)) {
            return true;
        } else if (visited[neighbor] == 1) {
            return true;
        }
    }
    
    visited[v] = 2;
    return false;
}

bool hasCycle(int V) {
    visited.assign(V, 0);
    
    for (int i = 0; i < V; i++) {
        if (visited[i] == 0 && dfs(i)) {
            return true;
        }
    }
    return false;
}

```

```cpp
bool hasCycle(int V) {
    vector<int> inDegree(V, 0);
    for (int i = 0; i < V; i++) {
        for (int neighbor : adj[i]) {
            inDegree[neighbor]++;
        }
    }
    
    queue<int> q;
    for (int i = 0; i < V; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    int count = 0;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        count++;
        
        for (int neighbor : adj[node]) {
            if (--inDegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    return count != V;
}
```
# DAG and Topological sort
```cpp
vector<int> topologicalSort(int V) {
    vector<int> inDegree(V, 0);
    for (int i = 0; i < V; i++) {
        for (int neighbor : adj[i]) {
            inDegree[neighbor]++;
        }
    }
    
    queue<int> q;
    for (int i = 0; i < V; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    vector<int> topoOrder;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        topoOrder.push_back(node);
        
        for (int neighbor : adj[node]) {
            if (--inDegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    if (topoOrder.size() != V) {
        return {}; // Not a DAG, cycle detected
    }
    return topoOrder;
}
```

```cpp
vector<int> visited; // 0: not visited, 1: visiting, 2: visited
vector<int> topoOrder;

bool dfs(int v) {
    visited[v] = 1;
    for (int neighbor : adj[v]) {
        if (visited[neighbor] == 0) {
            if (dfs(neighbor)) return true;
        } else if (visited[neighbor] == 1) {
            return true; // Cycle detected
        }
    }
    visited[v] = 2;
    topoOrder.push_back(v);
    return false;
}

vector<int> topologicalSort(int V) {
    visited.assign(V, 0);
    topoOrder.clear();
    
    for (int i = 0; i < V; i++) {
        if (visited[i] == 0) {
            if (dfs(i)) {
                return {}; // Not a DAG, cycle detected
            }
        }
    }
    reverse(topoOrder.begin(), topoOrder.end());
    return topoOrder;
}
```