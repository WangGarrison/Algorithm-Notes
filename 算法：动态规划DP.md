# T61

# 动规简介

> 动态规划（Dynamic programming，即DP）是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，**通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。**
>
> 基本思想：若要解一个给定的问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。
>
> 通常许多子问题非常相似，==一旦某个给定的子问题的解已经算出，则将其记忆化存储==，以便下次需要同一个子问题解之时能直接查表。动态规划试图仅仅解决每个子问题一次，具有天然剪枝的功能，从而减少计算量。这种做法在重复子问题的数目关于输入的规模呈指数增长时特别有用
>
> 动态规划常常适用于==有重叠子问题和最优子结构==性质的问题，并且记录所有子问题的结果，因此动态规划方法所耗时间往往远少于朴素解法。
>
> 动态规划有自底向上和自顶向下两种解决问题的方式。
>
> - 自底向上：递推。
>
> - 自顶向下：记忆化递归
>
> 摘自：[Leetcode动态规划tag](https://leetcode-cn.com/tag/dynamic-programming/problemset/)

算法的基本思想与分治算法类似，也是将待求解问题分为若干子问题，按划分的顺序求解子阶段问题，前一个子问题的解，为后一子问题的求解提供了有用的信息（最优子结构）。在求解任一子问题时，列出各种可能的局部解，通过决策保留那些有可能达到最优的局部解，丢弃其他局部解。依次解决各个子问题，最后求出原问题的最优解

与分治算法最大的区别是：适合用于动态规划算法求解的问题，经分解后得到的**子问题往往不是互相独立**的

求解动归问题一般是求出状态与状态转移方程：

- 子问题的解 ==》状态
- 子问题的解怎么求出原问题的解 ==》状态转移方程

**动规所处理的问题是一个多阶段决策问题，一般由初始状态开始，通过对中间阶段决策的选择，达到结束状态。**

动规一般模式：初始状态 =》决策1=》决策2=》...决策n=》结束状态

- 找出问题最优解的性质，并刻画其结构特征（找问题状态）
- 递归地定义最优值（找状态转移方程）
- 自底向上地方式计算最优值
- 根据计算最优值时得到的信息，构造最优解

# 硬币选择问题

有1, 3, 5分面额的硬币，给定一个面值11，问组成给定面值所需要的最少的硬币数量是多少？

**解法一：分治算法**

![image-20210502103523206](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502103523206.png)

![image-20210502103553567](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502103553567.png)

**递归优化**

上述解法有一些子问题被重复求解了，==可以定义一个数组，存储每个子问题的结果（记忆化存储），再遇到相同的子问题时直接查表==

![image-20210502104937800](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502104937800.png)

**非递归解法**

![image-20210502113221018](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502113221018.png)

![image-20210502113938337](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502113938337.png)

# 斐波那契数列 √

0  1  1  2  3

```cpp
//迭代
int Fibonacci(int n) 
{
    //0 1 1 2
    //i j k
    int i = 0;
    int j = 1;
    if(n <= 1)    return n;
    int k = i + j;
    for(int m = 2; m <= n; ++m)
    {
        k = i + j;
        i = j;
        j = k;
    }
    return k;
}

//递归
int Fibonacci(int n)
{
    if(n <= 1)    return n;
    return Fibonacci(n-1) + Fibonacci(n-2);
}

//优化递归
int Fib(int n, int first, int second)  //调用n次Fib，每次更新first与second
{
    if(n <= 1)    return second;
    return Fib(n-1, second, first+second);
}
int Fibonacci(int n)
{
    if(n <= 1)    return n;
    return Fib(n, 0, 1);
}
```

**注意：施的数列是：1  1  2  3  ...**

![image-20210502120052528](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502120052528.png)

**递归（记忆化存储）**

![image-20210502120307892](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502120307892.png)

**非递归**

![image-20210502120617309](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502120617309.png)



# 最大子序和问题

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```cpp
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**我的思路：**

- 若当前元素之前的和小于0，则丢弃之前的和，重新计算和, 每次更新最大值

  ![image-20210502121606941](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502121606941.png)

```cpp
int maxSubArray(vector<int>& nums) 
{
    if(nums.empty())    return 0;

    int preSum = 0;
    int maxSum = nums[0];

    for(int i = 0; i < nums.size(); ++i)
    {
        if(preSum <= 0)
        { 
            preSum = nums[i];  //丢弃之前的和，重新计算和
        }
        else
        {
            preSum += nums[i];
        } 
        maxSum = max(preSum, maxSum);
    }
    return maxSum;
}
```

**注意：施的题目和上面的不完全一样**

![image-20210502123457078](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502123457078.png)

![image-20210502123213652](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210502123213652.png)

# 最长递增子序列长度LIS√

```shell
输入：5 3 4 1 8 7 9
输出：4 （3 4 8 9）
```

**我的思路：**

- dp[i]: 以第i个元素结尾的递增子序列的长度

- dp[0] = 1;

- dp[1] = max{1, 1+dp[0]} ar[0] < ar[1]

- dp[2] = max{1, 1+dp[1]} ar[1] < ar[2]

- dp[i] = max{1, 1+dp[i-1]}

  <img align='left' src="img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210504120115773.png" alt="image-20210504120115773" style="zoom:30%;" />

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& ar) 
    {
        /*
          dp[i]: 以第i个元素结尾的递增子序列的长度
          dp[0] = 1;
          dp[1] = max{1, 1+dp[0]} ar[0] < ar[1]
          dp[2] = max{1, 1+dp[1]} ar[1] < ar[2]
          ...
          dp[i] = max{1, 1+dp[i-1]}
        */

        int maxlen = 0;
        vector<int> dp(ar.size(), 1); //dp初始化大小并赋值为1

        for(int i = 0; i < ar.size(); ++i)
        {
            //j遍历从0到i
            for(int j = 0; j < i; ++j)
            {
                if(ar[j] < ar[i] && 1+dp[j] > dp[i])
                {
                    dp[i] = 1 + dp[j];
                }
            }
        
            maxlen = maxlen > dp[i] ? maxlen : dp[i];
        }
        return maxlen;
    }
};
```

**扩充：**怎么输出最长子序列具体内容？加一个数组用来保存当前元素的前一个元素的下标，输出的时候从尾部跳着往前找

```cpp
vector<int> LIS(vector<int>& arr)
{
    int size = arr.size();
    vector<int> dp(size, 1);
    vector<int> path(size, -1);  //记录路径（前一个元素的下标）
    int endindex = 0;
    int maxlen = 0;

    for (int i = 0; i < size; ++i)
    {
        for (int j = 0; j < i; ++j)
        {
            if (arr[j] < arr[i] && 1 + dp[j] > dp[i])
            {
                dp[i] = 1 + dp[j];
                path[i] = j;  //保存前一个元素的下标
            }
        }
        maxlen = maxlen > dp[i] ? maxlen : dp[i];
        if (maxlen == dp[i])  endindex = i;
    }

    vector<int> rt;
    int i;
    for (i = endindex; path[i] != -1; i = path[i])//跳着往前找
    {
        rt.push_back(arr[i]);
    }
    rt.push_back(arr[i]);
    reverse(rt.begin(), rt.end());
    return rt;
}
```

# 最长公共子序列LCS √

LCS：longest common subsequence（注意，子序列可以不连续，子串要求连续）

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

```shell
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**我的思路：**

- 若Xn == Ym：LCS(X[1...n], Y[1...m]) = LCS(X[1...n-1], Y[1...m-1]);
- 若Xn != Ym：LCS(X[1...n], Y[1...m]) = max{  LCS(X[1...n], Y[1...m-1]),  LCS(X[1...n-1], Y[1...m]) } 
- 动态规划，并使用dp数组进行记忆化存储来优化

![image-20210503180145128](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210503180145128.png)

```cpp
class Solution {
public:
    int longestCommonSubsequence(string str1, string str2) 
    {
        //初始化dp数组
        //dp[n][m]：n表示第一个串的长度，m是第二个串的长度
        //dp[n][m]记录的就是当尾部下标分别为n和m时，此时LCS的长度
        vector<vector<int>> dp;  
        dp.resize(str1.size());
        for(int i = 0; i < str1.size(); ++i)
        {
            dp[i].resize(str2.size(), -1);
        }

        //动态规划，从尾部开始计算
        return LCS(str1, str1.size() - 1, str2, str2.size() -1, dp);
    }

    int LCS(string & str1, int n, string & str2, int m, vector<vector<int>> & dp)
    {
        if(n < 0 || m < 0)  return 0;

        if(dp[n][m] >= 0)   return dp[n][m];  //查表，子问题已被求解过

        if(str1[n] == str2[m])
        {
            dp[n][m] = LCS(str1, n-1, str2, m-1, dp) + 1;
            return dp[n][m];
        }
        else
        {
            int len1 = LCS(str1, n, str2, m-1, dp);
            int len2 = LCS(str1, n-1, str2, m, dp);
            dp[n][m] = len1 > len2 ? len1 : len2;
            return dp[n][m];
        }
    }
};
```

扩展：怎么找出最长回文子序列，找出该串和该串逆置后的串，这两个串的最长子序列就是最长回文子序列，（找回文子序列可以用该思路，回文子串用该思路时，有的用例通不过（输入：

"aacabdkacaa"

输出：

"aaca"

预期结果：

"aca"），因此这时还需判断找到的字符串是否来自同一个字符串）

# 输出最长公共子序列 √

上述最长公共子序列题目只输出了LCS的长度，那LCS内容是什么呢？

**我的思路：**

- 在上题的基础上，多用一个path数组记录每次的路径，根据该数组输出字符

```cpp
#include <vector>
#include <iostream>
#include <stdio.h>
using namespace std;

class Solution {
public:
	vector<vector<int>> path;  //记录路径，1对角线，2左边，3上边

    //根据path数组，输出LCS
	void showLCS(string & str1, int n, int m)
	{
		if (n < 0 || m < 0)	return;

		if (path[n][m] == 1)
		{
			showLCS(str1, n - 1, m - 1);
			cout<<str1[n]<<" ";	
		}
		else if (path[n][m] == 2)
		{
			showLCS(str1, n , m-1);
		}
		else
		{
			showLCS(str1, n - 1, m);
		}
	}

	int longestCommonSubsequence(string str1, string str2)
	{
		//初始化dp数组
		//dp[n][m]：n表示第一个串的长度，m是第二个串的长度
		//dp[n][m]记录的就是当尾部下标分别为n和m时，此时LCS的长度
		vector<vector<int>> dp;
		dp.resize(str1.size());
		for (int i = 0; i < str1.size(); ++i)
		{
			dp[i].resize(str2.size(), -1);
		}
		
		path = dp;

		//动态规划，从尾部开始计算
		return LCS(str1, str1.size() - 1, str2, str2.size() - 1, dp);
	}

	int LCS(string & str1, int n, string & str2, int m, vector<vector<int>> & dp)
	{
		if (n < 0 || m < 0)  return 0;

		if (dp[n][m] >= 0)   return dp[n][m];  //查表，子问题已被求解过

		if (str1[n] == str2[m])
		{
			dp[n][m] = LCS(str1, n - 1, str2, m - 1, dp) + 1;
			path[n][m] = 1;  //对角线
			return dp[n][m];
		}
		else
		{
			int len1 = LCS(str1, n, str2, m - 1, dp);
			int len2 = LCS(str1, n - 1, str2, m, dp);
			dp[n][m] = len1 > len2 ? len1 : len2;

			if (dp[n][m] == len1)	path[n][m] = 2;//左边
			else path[n][m] = 3;//上边

			return dp[n][m];
		}
	}
};

int main()
{
	string str1("abcde");
	string str2("ace");

	Solution sol;
	cout << sol.longestCommonSubsequence(str1, str2) << endl<<endl;

	//打印路径数组
	for (int i = 0; i < sol.path.size(); ++i)
	{
		for (int j = 0; j < sol.path[i].size(); ++j)
		{
			printf("%2d  ", sol.path[i][j]);
			//cout << sol.path[i][j] << "  ";
		}
		cout << endl;
	}
	cout << endl << endl;

	sol.showLCS(str1, str1.size()-1, str2.size()-1);
}
```

![image-20210503190914498](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210503190914498.png)

# 最长回文子序列 √

```shell
给定一个字符串 s ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 s 的最大长度为 1000 。

输入: "bbbab"
输出:  4
一个可能的最长回文子序列为 "bbbb"。
```

**我的思路：**

- 找出该串和该串逆置后的串，这两个串的最长子序列就是原串最长回文子序列

```cpp
class Solution {
public:
    //最长回文子序列
    int longestPalindromeSubseq(string s) 
    {
        string str1 = s;
        reverse(s.begin(), s.end());
        string str2 = s;

        return LCS(str1, str2);
    }

    //最长公共子序列
    int LCS(string str1, string str2) 
    {
        //初始化dp数组
        //dp[n][m]：n表示第一个串的长度，m是第二个串的长度
        //dp[n][m]记录的就是当尾部下标分别为n和m时，此时LCS的长度
        vector<vector<int>> dp;  
        dp.resize(str1.size());
        for(int i = 0; i < str1.size(); ++i)
        {
            dp[i].resize(str2.size(), -1);
        }

        //动态规划，从尾部开始计算
        return LCS(str1, str1.size() - 1, str2, str2.size() -1, dp);
    }

    int LCS(string & str1, int n, string & str2, int m, vector<vector<int>> & dp)
    {
        if(n < 0 || m < 0)  return 0;

        if(dp[n][m] >= 0)   return dp[n][m];  //查表，子问题已被求解过

        if(str1[n] == str2[m])
        {
            dp[n][m] = LCS(str1, n-1, str2, m-1, dp) + 1;
            return dp[n][m];
        }
        else
        {
            int len1 = LCS(str1, n, str2, m-1, dp);
            int len2 = LCS(str1, n-1, str2, m, dp);
            dp[n][m] = len1 > len2 ? len1 : len2;
            return dp[n][m];
        }
    }
};
```

# 最长公共子串 √

```shell
给定两个字符串str1和str2,输出两个字符串的最长公共子串
题目保证str1和str2的最长公共子串存在且唯一。

输入："abcbced","acbcbcef"
返回值："bcbc"
```

**我的思路：**（[求两个字符串的最长公共子串](https://blog.csdn.net/qq_25800311/article/details/81607168)）

- 把两个字符串分别以行和列组成一个二维矩阵。

- 比较二维矩阵中每个点对应行列字符中否相等，相等的话值设置为1，否则设置为0。

- 通过查找出值为1的最长对角线就能找到最长公共子串

  <img align='left' src="img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210503222609250.png" alt="image-20210503222609250" style="zoom:50%;" />

```cpp
string LCS(string str1, string str2) 
{
    //用二维数组记录str1与str2，行列相等进行记录
    int maxlen = 0;    //子串最大长度
    int endindex = 0;  //子串结束下标
    vector<vector<int>> matrix(str1.size(),vector<int>(str2.size()));
    for(int i = 0; i < str1.size(); ++i)
    {
        for(int j = 0; j < str2.size(); ++j)
        {
            if(str1[i] == str2[j])
            {
                if(i==0 || j == 0)    matrix[i][j] = 1;
                else	matrix[i][j] = matrix[i-1][j-1] + 1;
            }
            else
            {
                matrix[i][j] = 0;
            }

            if(matrix[i][j] > maxlen)
            {
                maxlen = matrix[i][j];
                endindex = i;
            }
        }
    }
    //substr(pos, size)：从pos开始拷贝size个字符返回
    return str1.substr(endindex-maxlen+1, maxlen);//（起始位置，几个字符）
}
```

# 最长回文子串 √

```shell
给你一个字符串 s，找到 s 中最长的回文子串。

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
      012345678910
输入："aacabdkacaa"
输出："aca"
```

**我的思路：**

- 找出该串和该串逆置后的串，这两个串的最长子串就是原串最长回文串

- 注意：需判断找到的字符串是否来自同一个字符串

- <img align='left' src="img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210503220746132.png" alt="image-20210503220746132" style="zoom:40%;" />

- 怎么判断是否来自同一个字符串？

  ![image-20210503223048544](img/%E7%AE%97%E6%B3%95%EF%BC%9A%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92DP.img/image-20210503223048544.png)

```cpp
class Solution {
public:
    string longestPalindrome(string s) 
    {
        string str1 = s;
        reverse(s.begin(), s.end());
        string str2 = s;

        return LCS(str1, str2);
    }
    string LCS(string str1, string str2) 
    {
        //用二维数组记录str1与str2，行列相等进行记录
        int maxlen = 0;    //子串最大长度
        int endindex = 0;  //子串结束下标
        vector<vector<int>> matrix(str1.size(),vector<int>(str2.size()));
        for(int i = 0; i < str1.size(); ++i)
        {
            for(int j = 0; j < str2.size(); ++j)
            {
                if(str1[i] == str2[j])
                {
                    if(i==0 || j == 0)    matrix[i][j] = 1;
                    else	matrix[i][j] = matrix[i-1][j-1] + 1;
                }
                else
                {
                    matrix[i][j] = 0;
                }

                if(matrix[i][j] > maxlen)
                {
                    //判断比较的字符串是不是来源自同一个字符串
                    int pre = i - matrix[i][j] + 1;  //之前串的起始下标
                    int now = j - matrix[i][j] + 1;  //现在串的起始下标
                    if(now == str1.size()-pre-matrix[i][j])
                    {
                        maxlen = matrix[i][j];
                        endindex = i;
                    }
                    
                }
            }
        }
        //substr(pos, size)：从pos开始拷贝size个字符返回
        return str1.substr(endindex-maxlen+1, maxlen);//（起始位置，几个字符）
    }
};
```



# T70