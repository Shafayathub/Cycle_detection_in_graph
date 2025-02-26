# Cycle Detection in Directed and Undirected Graphs using DFS and BFS in C++

## Overview
This repository contains C++ implementations for detecting cycles in both directed and undirected graphs using **Depth First Search (DFS)** and **Breadth First Search (BFS)**.

## Algorithms Used
### 1. Cycle Detection in an Undirected Graph
- **Using DFS**: Detects back edges in a DFS traversal.
- **Using BFS**: Uses BFS with parent tracking to detect cycles.

### 2. Cycle Detection in a Directed Graph
- **Using DFS (Recursion Stack)**: Checks for back edges by maintaining a recursion stack.
- **Using BFS (Kahn’s Algorithm - Topological Sorting)**: If a topological sort is not possible (i.e., there is a cycle), the graph contains a cycle.

## Code Implementations
### Cycle Detection in an Undirected Graph using DFS
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> adj_list[105];
bool vis[105];
int parent[105];
bool cycle;

void dfs(int src)
{
    vis[src] = true;
    for (int child : adj_list[src])
    {
        if (vis[child] && parent[src] != child)
        {
            cycle = true;
        }
        if (!vis[child])
        {
            parent[child] = src;
            dfs(child);
        }
    }
}

int main()
{
    int n, e;
    cin >> n >> e;

    while (e--)
    {
        int a, b;
        cin >> a >> b;
        adj_list[a].push_back(b);
        adj_list[b].push_back(a);
    }

    memset(vis, false, sizeof(vis));
    memset(parent, -1, sizeof(parent));
    cycle = false;
    for (int i = 0; i < n; i++)
    {
        if (!vis[i])
        {
            dfs(i);
        }
    }
    if (cycle)
    {
        cout << "Cycle Detected\n";
    }
    else
    {
        cout << "No Cycle\n";
    }

    return 0;
}
```

### Cycle Detection in an Undirected Graph using BFS
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> adj_list[105];
bool vis[105];
int parent[105];
bool cycle;

void bfs(int src)
{
    queue<int> q;
    q.push(src);
    vis[src] = true;
    while (!q.empty())
    {
        int par = q.front();
        q.pop();
        for (int child : adj_list[par])
        {
            if (vis[child] && parent[par] != child)
            {
                cycle = true;
            }
            if (!vis[child])
            {
                q.push(child);
                vis[child] = true;
                parent[child] = par;
            }
        }
    }
}

int main()
{
    int n, e;
    cin >> n >> e;

    while (e--)
    {
        int a, b;
        cin >> a >> b;
        adj_list[a].push_back(b);
        adj_list[b].push_back(a);
    }

    memset(vis, false, sizeof(vis));
    memset(parent, -1, sizeof(parent));
    cycle = false;
    for (int i = 0; i < n; i++)
    {
        if (!vis[i])
        {
            bfs(i);
        }
    }
    if (cycle)
    {
        cout << "Cycle Detected\n";
    }
    else
    {
        cout << "No Cycle\n";
    }

    return 0;
}
```

### Cycle Detection in a Directed Graph using DFS
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> adj_list[105];
bool vis[105], in_stack[105];
bool cycle;

void dfs(int src)
{
    vis[src] = true;
    in_stack[src] = true;
    for (int child : adj_list[src])
    {
        if (!vis[child])
        {
            dfs(child);
        }
        else if (in_stack[child])
        {
            cycle = true;
        }
    }
    in_stack[src] = false;
}

int main()
{
    int n, e;
    cin >> n >> e;

    while (e--)
    {
        int a, b;
        cin >> a >> b;
        adj_list[a].push_back(b);
    }

    memset(vis, false, sizeof(vis));
    memset(in_stack, false, sizeof(in_stack));
    cycle = false;
    for (int i = 0; i < n; i++)
    {
        if (!vis[i])
        {
            dfs(i);
        }
    }
    if (cycle)
    {
        cout << "Cycle Detected\n";
    }
    else
    {
        cout << "No Cycle\n";
    }

    return 0;
}
```

### Cycle Detection in a Directed Graph using BFS (Kahn’s Algorithm)
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> adj_list[105];
int in_degree[105];

bool detect_cycle(int n)
{
    queue<int> q;
    int count = 0;

    for (int i = 0; i < n; i++)
    {
        if (in_degree[i] == 0)
        {
            q.push(i);
        }
    }

    while (!q.empty())
    {
        int node = q.front();
        q.pop();
        count++;
        for (int child : adj_list[node])
        {
            if (--in_degree[child] == 0)
            {
                q.push(child);
            }
        }
    }
    return count != n;
}

int main()
{
    int n, e;
    cin >> n >> e;

    memset(in_degree, 0, sizeof(in_degree));
    for (int i = 0; i < e; i++)
    {
        int a, b;
        cin >> a >> b;
        adj_list[a].push_back(b);
        in_degree[b]++;
    }

    if (detect_cycle(n))
    {
        cout << "Cycle Detected\n";
    }
    else
    {
        cout << "No Cycle\n";
    }
    return 0;
}
```

## How to Run
1. Compile the C++ program using a compiler like `g++`:
   ```sh
   g++ cycle_detection.cpp -o cycle_detection
   ```
2. Run the executable:
   ```sh
   ./cycle_detection
   ```

## Applications of Cycle Detection
- Deadlock detection in operating systems
- Dependency resolution in package managers
- Circuit analysis in electrical engineering
- Pathfinding and network flow problems


## License
This project is licensed under the MIT License.
