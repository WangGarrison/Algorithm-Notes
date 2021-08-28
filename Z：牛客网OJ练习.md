# 牛客网OJ在线编程练习场

OJ：online judge，在线判题系统

https://ac.nowcoder.com/acm/contest/5657

# cin与getline

**cin：**当 cin 读取数据时，它会传递并忽略任何前导白色空格字符（空格、制表符或换行符）。一旦它接触到第一个非空格字符即开始阅读，当它读取到下一个空白字符时，它将停止读取

**gelline：**此函数可读取整行，包括前导和嵌入的空格，并将其存储在字符串对象中。

```CPP
getline(cin, str);  
getline(cin, str, ",");  //将分隔符之前的字符拷贝到缓冲区中，但分隔符本身不拷贝进去，并且下次读操作将从分隔符后的下一个字符开始。
```

# 不定行数据输入

**输入描述:**

```
输入数据有多组, 每行表示一组输入数据。
每行不定有n个整数，空格隔开。(1 <= n <= 100)。
1 2 3
4 5
0 0 0 0 0
```

**输出描述:**

```
每组数据输出求和的结果
6
9
0
```

```CPP
#include <iostream>
using namespace std;

int main()
{
    int a;
    while(cin>>a)
    {
        int sum = 0;
        while(cin.get() == ' ')
        {
            sum += a;
            cin>>a;
        }
        sum += a;
        cout<<sum<<endl;
    }
}
```

# 对输入的字符串进行排序后输出

链接：https://ac.nowcoder.com/acm/contest/5657/H
来源：牛客网

**输入描述:**

```
多个测试用例，每个测试用例一行。
每行通过空格隔开，有n个字符，n＜100
a c bb
f dddd
nowcoder
```

**输出描述:**

```
对于每组测试用例，输出一行排序过的字符串，每个字符串通过空格隔开
a bb c
dddd f
nowcoder
```

```CPP
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

int main()
{
    string str;
    while(cin>>str)
    {
        vector<string> vec;
        vec.push_back(str);
        while(cin.get() == ' ')
        {
            cin>>str;
            vec.push_back(str);
        }
        sort(vec.begin(), vec.end());
        for(auto & str : vec)
        {
            cout<<str<<" ";
        }
        cout<<endl;
        str.clear();
    }
}
```

# 对输入的字符串进行排序后输出(逗号分割)

**输入描述:**

```
多个测试用例，每个测试用例一行。
每行通过,隔开，有n个字符，n＜100
a,c,bb
f,dddd
nowcoder
```

**输出描述:**

```
对于每组用例输出一行排序后的字符串，用','隔开，无结尾空格
a,bb,c
dddd,f
nowcoder
```

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <algorithm>
using namespace std;

int main()
{
    string str;
    while(getline(cin, str, '\n'))  //读取cin中换行符之前的
    {
        stringstream ss(str);  //初始化流对象ss
        vector<string> vec;
        while(getline(ss, str, ','))  //读取ss中,之前的放到str里
        {
            vec.push_back(str);
        }
        sort(vec.begin(), vec.end());
        for(int i = 0; i < vec.size()-1; ++i)
        {
            cout<<vec[i]<<",";
        }
        cout<<vec[vec.size()-1]<<endl;
    }
}
```

# int换成long类型，通过率提升

```cpp
/*
输入有多组测试用例，每组空格隔开两个整数
对于每组数据输出一行两个整数的和
*/
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    long a, b;
    while(cin>>a>>b)
    {
        cout<<a+b<<endl;
    }
}
```



