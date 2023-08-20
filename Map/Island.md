# Key

- BFS(Breadth first search) use queue
- DFS use stack

# 797

## DFS

```cpp
class Solution
{
public:
    vector<vector<int>> result;
    vector<int> path;
    void dfs(vector<vector<int>> &graph, int start)
    {
        if (path[path.size() - 1] == graph.size() - 1)
        {
            result.push_back(path);
            return;
        }

        for (int i = 0; i < graph[start].size(); i++)
        {
            path.push_back(graph[start][i]);
            dfs(graph, graph[start][i]);
            path.pop_back();
        }
    }
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>> &graph)
    {
        path.clear();
        result.clear();
        path.push_back(0);
        dfs(graph, 0);
        return result;
    }
};
```

# 200

## DFS
```cpp
// 版本二
class Solution {
private:
    int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
    void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int x, int y) {
        if (visited[x][y] || grid[x][y] == '0') return; // 终止条件：访问过的节点 或者 遇到海水
        visited[x][y] = true; // 标记访问过
        for (int i = 0; i < 4; i++) {
            int nextx = x + dir[i][0];
            int nexty = y + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            dfs(grid, visited, nextx, nexty);
        }
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited = vector<vector<bool>>(n, vector<bool>(m, false));

        int result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!visited[i][j] && grid[i][j] == '1') {
                    result++; // 遇到没访问过的陆地，+1
                    dfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
                }
            }
        }
        return result;
    }
};
```

## BFS

```cpp
class Solution {
private:
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> que;
    que.push({x, y});
    visited[x][y] = true; // 只要加入队列，立刻标记
    while(!que.empty()) {
        pair<int ,int> cur = que.front(); que.pop();
        int curx = cur.first;
        int cury = cur.second;
        for (int i = 0; i < 4; i++) {
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            if (!visited[nextx][nexty] && grid[nextx][nexty] == '1') {
                que.push({nextx, nexty});
                visited[nextx][nexty] = true; // 只要加入队列立刻标记
            }
        }
    }
}
public:
    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited = vector<vector<bool>>(n, vector<bool>(m, false));

        int result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!visited[i][j] && grid[i][j] == '1') {
                    result++; // 遇到没访问过的陆地，+1
                    bfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
                }
            }
        }
        return result;
    }
};
```


# 695

## DFS

```cpp
class Solution
{
public:
    int count;
    vector<pair<int, int>> dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    void dfs(vector<vector<int>> &grid, vector<vector<int>> &visited, int row, int col)
    {
        int rowNext;
        int colNext;
        for (int i = 0; i < 4; i++)
        {
            rowNext = row + dir[i].first;
            colNext = col + dir[i].second;
            if (rowNext < 0 || rowNext >= grid.size() || colNext < 0 || colNext >= grid[0].size())
            {
                continue;
            }
            if (grid[rowNext][colNext] == 1 && visited[rowNext][colNext] == 0)
            {
                count++;
                visited[rowNext][colNext] = 1;
                dfs(grid, visited, rowNext, colNext);
            }
        }
    }
    int maxAreaOfIsland(vector<vector<int>> &grid)
    {

        vector<vector<int>> visited(grid.size(), vector<int>(grid[0].size()));
        int result = 0;
        for (int row = 0; row < grid.size(); row++)
        {
            for (int col = 0; col < grid[0].size(); col++)
            {
                if (grid[row][col] == 1 && visited[row][col] == 0)
                {
                    count = 1;
                    visited[row][col] = 1;
                    dfs(grid, visited, row, col);
                    result = max(result, count);
                }
            }
        }
        return result;
    }
};
```

## BFS

```cpp
class Solution
{
public:
    vector<pair<int, int>> dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int count;
    int result = 0;
    void bfs(vector<vector<int>> &grid, vector<vector<int>> &visited, int row, int col)
    {
        queue<pair<int, int>> q;
        q.push({row, col});
        int colNext;
        int rowNext;
        count = 1;
        while (!q.empty())
        {
            int x = q.front().second;
            int y = q.front().first;
            q.pop();
            for (int i = 0; i < 4; i++)
            {
                colNext = x + dir[i].second;
                rowNext = y + dir[i].first;
                if (rowNext < 0 || rowNext >= grid.size() || colNext < 0 || colNext >= grid[0].size())
                {
                    continue;
                }
                if (grid[rowNext][colNext] == 1 && visited[rowNext][colNext] == 0)
                {
                    visited[rowNext][colNext] = 1;
                    q.push({rowNext, colNext});
                    count++;
                }
            }
        }
        result = max(result, count);
    }
    int maxAreaOfIsland(vector<vector<int>> &grid)
    {

        vector<vector<int>> visited(grid.size(), vector<int>(grid[0].size()));
        // int result = 0;
        queue<pair<int, int>> q;

        for (int row = 0; row < grid.size(); row++)
        {
            for (int col = 0; col < grid[0].size(); col++)
            {
                if (grid[row][col] == 1 && visited[row][col] == 0)
                {
                    visited[row][col] = 1;
                    bfs(grid, visited, row, col);
                }
            }
        }
        return result;
    }
};
```