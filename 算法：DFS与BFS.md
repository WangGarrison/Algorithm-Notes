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

# 二叉树中和为某一值的路径

```cpp
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。
给定如下二叉树，以及目标和 target = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:
[
   [5,4,11,2],
   [5,8,4,5]
]

```

**我的思路：**

- 深度优先遍历二叉树，每次遍历的时候记录之前的结点，遍历到叶子结点的时候看之前的和是否为target
- 是的话将该路径保存，不是的话该路径删除一个元素，回溯

```cpp
class Solution {
private:
    vector<int> onepath; 		 //一条路径
    vector<vector<int>> sumpath; //所有路径

public:
    vector<vector<int>>& pathSum(TreeNode* root, int target)
    {
        depth(root, target);
        return sumpath;
    }

    //深度优先遍历二叉树，每次遍历的时候记录之前的结点，遍历到叶子的时候看之前的和是否为target
    //是的话将该路径保存，不是的话该路径删除一个元素，回溯
    void depth(TreeNode* root, int target)
    {      
        if(root == nullptr) return;
        
        onepath.push_back(root->val);

        //遍历到底的时候看之前的和是否为target
        if (root->left == nullptr && root->right == nullptr) 
        {
            int sum = 0;
            for (auto & it : onepath)
            {
                sum += it;
            }

            if (target == sum)  sumpath.push_back(onepath);
        }
        
        pathSum(root->left, target);
        pathSum(root->right, target);

        onepath.pop_back();
    }
};
```



