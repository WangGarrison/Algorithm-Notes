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

# 括号生成

给出n对括号，请编写一个函数来生成所有的由n对括号组成的合法组合。

例如，给出n=3，解集为：

"((()))", "(()())", "(())()", "()()()", "()(())"

**我的思路：**

- 深度优先遍历
- 当前左右括号都有大于 0 个可以使用的时候，才产生分支
- 产生左分支的时候，只看当前是否还有左括号可以使用
- 产生右分支的时候，还受到左分支的限制，右边剩余可以使用的括号数量一定得在严格大于左边剩余的数量的时候，才可以产生分支
- 在左边和右边剩余的括号数都等于 0 的时候结算

<img align='left' src="img/%E7%AE%97%E6%B3%95%EF%BC%9ADFS%E4%B8%8EBFS.img/image-20210516004655268.png" alt="image-20210516004655268" style="zoom:50%;" />

```cpp
class Solution {
private:
    vector<string> rt;
public:
    vector<string> generateParenthesis(int n) 
    {
        if(n == 0)  return vector<string>();
        dfs("", n, n);
        return rt;
    }

    void dfs(string str, int left, int right)  //left和right表示剩余的左右括号数量
    {
        if(left > right || left < 0 || right < 0)    return;

        if(left == 0 && right == 0)
        {
            rt.push_back(str);
        }

        if(left > 0)
        {
            dfs(str + "(", left-1, right);
        }
        
        if(left < right)
        {
            dfs(str + ")", left, right-1);
        }
    }
};
```

# 矩阵中的路径

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。

<img align='left' src="img/%E7%AE%97%E6%B3%95%EF%BC%9ADFS%E4%B8%8EBFS.img/image-20210618232041378.png" alt="image-20210618232041378" style="zoom:40%;" />

```shell
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**我的思路：**

- 遍历矩阵，对每一个字母进行深度优先遍历去找给定的单词
- 每一次深度优先遍历里，已经访问并记录下的字母赋值为'\0'，防止回溯的时候重复记录

```cpp
class Solution {
private:
    bool flag = false;  //false没找到，true找到了
public:
    bool exist(vector<vector<char>>& board, string word) 
    {
        if(word.empty())    return true;
        if(board.empty())   return false;

        //深度优先遍历，对每一个字母进行dfs遍历，看能不能找到该单词
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board[0].size(); ++j)
            {
                dfs(board, i, j, word, 0);
                if(flag)    return flag;
            }
        }
        return false;
    }

    void dfs(vector<vector<char>> & board, int i, int j, string & word, int index)
    {
        if(index == word.size()-1 && board[i][j] == word[index] || index >= word.size())     
        {
            flag = true;
            return;
        }
        
        if(board[i][j] == word[index])   
        {
            board[i][j] = '\0';  //访问过该位置，就置为\0

            //右
            if(!flag && j+1 < board[0].size() && board[i][j+1] != '\0')  dfs(board,i, j+1, word, index+1);

            //下
            if(!flag && i+1 < board.size() && board[i+1][j] != '\0') dfs(board, i+1, j, word, index+1);

            //左
            if(!flag && j-1 >= 0 && board[i][j-1] != '\0')   dfs(board, i, j-1, word, index+1);

            //上
            if(!flag && i-1 >= 0 && board[i-1][j] != '\0')   dfs(board, i-1, j, word, index+1);

            board[i][j] = word[index];  //之前赋值成了\0，回溯的时候还原
        }
        //else if(board[i][j] != word[index]) return; 
    }
};
```

# 机器人的运动范围

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

```cpp
输入：m = 2, n = 3, k = 1
输出：3
    
输入：m = 3, n = 1, k = 0
输出：1
```

**我的思路：**

- 深度优先遍历，访问过一个位置就标记一下并计数，依此对四个方向进行访问

```cpp
class Solution {
private:
    int count = 0;
public:
    int movingCount(int m, int n, int k) 
    {
        vector<vector<bool>> vvec(m, vector<bool>(n));  //初始化是false
        dfs(0, 0, m, n, k, vvec);
        return count;
    }

    void dfs(int i, int j, int m, int n, int k, vector<vector<bool>> & vvec)
    {
        if(i >= m || j >= n)    return;

        if(funAdd(i,j) <= k && !vvec[i][j])
        {
            count++;
            vvec[i][j] = true;

            //右
            if(j+1 < n)     dfs(i, j+1, m, n, k, vvec);

            //下
            if(i+1 < m)     dfs(i+1, j, m, n, k, vvec);

            //左
            if(j-1 >= 0)    dfs(i, j-1, m, n, k, vvec);

            //上
            if(i-1 >= 0)    dfs(i-1, j, m, n, k, vvec);
        }
    }

    //求两个数字的数位之和
    int funAdd(int a, int b)
    {
        int sum1 = 0, sum2 = 0;
        while(a != 0)
        {
            sum1 += a % 10;
            a /= 10;
        }
        while(b != 0)
        {
            sum2 += b % 10;
            b /= 10;
        }
        return sum1 + sum2;
    }
};
```

