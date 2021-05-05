# 岛屿数量

```shell
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**我的思路1：**DFS

- 是1就深度优先遍历，每次把这次遍历上的1都置为0，即访问过的置为0
- 遍历的次数就是岛屿的数量
- `O(M*N)，S(M*N)`

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) 
    {
        if(grid.empty())    return 0;
        int count = 0;

        //是1就深度优先遍历，每次把这次遍历上的1都置为0
        //遍历的次数就是岛屿的数量
        for(int i = 0; i < grid.size(); ++i)
        {
            for(int j = 0; j < grid[0].size(); ++j)
            {
                if(grid[i][j] == '1')
                {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }

    //是1就进行深度优先遍历
    void dfs(vector<vector<char>> & grid, int i, int j)
    {
        grid[i][j] = '0';
        //右
        if(j+1 < grid[0].size() && grid[i][j+1] == '1')    dfs(grid, i, j+1);
        //下
        if(i+1 < grid.size() && grid[i+1][j] == '1')    dfs(grid, i+1, j);
        //左
        if(j-1 >= 0 && grid[i][j-1] == '1')   dfs(grid, i, j-1);
        //上
        if(i-1 >= 0 && grid[i-1][j] == '1') dfs(grid, i-1, j);
    }
};
```

**我的思路2：**BFS，队列

- 是1就广度优先遍历，每次把这次遍历上的1都置为0，即访问过的置为0
- 遍历的次数就是岛屿的数量

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) 
    {
        if(grid.empty())    return 0;
        int count = 0;

        //是1就广度优先遍历，每次把这次遍历上的1都置为0
        //遍历的次数就是岛屿的数量
        for(int i = 0; i < grid.size(); ++i)
        {
            for(int j = 0; j < grid[0].size(); ++j)
            {
                if(grid[i][j] == '1')
                {
                    count++;
                    bfs(grid, i, j);
                }
            }
        }
        return count;
    }

    //广度优先遍历
    void bfs(vector<vector<char>> & grid, int i, int j)
    {
        grid[i][j] = '0';
        queue<pair<int,int>> que;  //队列存坐标
        que.push({i, j});

        while(!que.empty())
        {
            int i = que.front().first;
            int j = que.front().second;
            que.pop();

            //右
            if(j+1 < grid[0].size() && grid[i][j+1] == '1')    
            {
                que.push({i, j+1});
                grid[i][j+1] = '0';
            }
            //下
            if(i+1 < grid.size() && grid[i+1][j] == '1')    
            {
                que.push({i+1, j});
                grid[i+1][j] = '0';
            }
            //左
            if(j-1 >= 0 && grid[i][j-1] == '1')    
            {
                que.push({i, j-1});
                grid[i][j-1] = '0';
            }
            //上
            if(i-1 >= 0 && grid[i-1][j] == '1')    
            {
                que.push({i-1, j});
                grid[i-1][j] = '0';
            }
        }       
    }
};
```

