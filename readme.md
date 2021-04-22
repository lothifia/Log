# 记录

------

[TOC]



### 21/4/18



#### Leetcode 220 重复元素: 

https://leetcode-cn.com/problems/contains-duplicate-iii/

#### <font color='red'>set | unorderedset</font> 

```c++
set<int> set_test;//支持二分查找, 插入 删除
set_test.lower_bound(begin, end, num);//找到set内部第一个 大于或等于num的值的位置 没有符合条件的则找到set_test.end();
set_test.upper_bound(begin, end, num);//找到set内部最后一个 大于num的值的位置
set_test.insert(num);//插入一个
set_test.erase(num| iterator)//根据数值或者 地址来删除
    //set -> 顺序存储 unorderedset ->非顺序存储(hash表)
    
```

set 用于优化 <font color='green'>滑动窗口</font> (二分查找 可以让时间从$n^2$ 降到 $nlog(n)$)

![Image text](https://github.com/lothifia/Log/blob/main/readme.assets/20191106171908375-1618682427723.png)



### 4/19

#### Leetcode 27 移除数组

https://leetcode-cn.com/problems/remove-element/

#### 双指针

通过维护左右两个指针, 完成对数组内部值的筛选.

左指针做结果记录, 右指针进性遍历.



#### LC 28 字符串匹配

https://leetcode-cn.com/problems/implement-strstr/

#### kmp算法

通过记录匹配串中<font color='red'>字串</font>的前后缀匹配长度来改变当前位置不匹配时， 下一次的开始匹配位置

##### next 数组(双指针?)

$model : adgihaofdjoasdifjoasdij$

$pattern: a, b, a, b, a, c, a$ -> 构造$next$

(-1 代表 不存在匹配前后缀, 0表示<font color='red'>前后缀匹配长度</font>为 1, 1 $means$2...)

| 位置       | next[0] | 1    | 2    | 3    | 4     | 5      | 6       |      |
| ---------- | ------- | ---- | ---- | ---- | ----- | ------ | ------- | ---- |
| 部分字符串 | a       | ab   | aba  | abab | ababa | ababac | ababaca |      |
| 位置       | -1      | -1   | 0    | 1    | -1    | 0      | 1       |      |

|                                            | 直接跳过(0) | 1    | 2                                 | 3                                 | 4    | 5    | 6    |
| ------------------------------------------ | ----------- | ---- | --------------------------------- | --------------------------------- | ---- | ---- | ---- |
| $i$ :<font color='red'>后缀位置</font>     | 0           | 1    | 2                                 | 3                                 | 4    | 5    |      |
| $j + 1$(<font color='red'>在判断时</font>) | 0           | 0    | 0<font color='red'>(a = a)</font> | 1<font color='red'>(b = b)</font> | 2    | ?    |      |
| $j$ : <font color='red'>前缀位置</font>    | -1          | -1   | 0                                 | 1                                 | 2    | ?    |      |
| next[i]                                    | -1          | -1   | 0                                 | 1                                 | 2    | ?    |      |
|                                            | a           | b    | a                                 | b                                 | a    | c    | a    |



```c++
void  get_next(string str, vector<int> next){
    next[0] = -1; int n = next.size();
    int j = - 1;
    //第一位不用判断
    for(int i = 1; i < n; i++){
        while(str[i] != str[j + 1] && j >= 0){
            j = next[j];
        }
        if(str[i] == str[j + 1]) j++;
        next[i] = j;
    }
}
```

**保证下一步匹配的前缀和后缀，只有最后一位不相同即可。**

for循环 到$ i = 5$  此时str中 $ c!= a$ 在  $j$ 处回溯, 

因为 由于此时 $next[5] = c$ 与 $next[2] = a$ 不等 , 而 $next[i - 1]$ (即<font color='red'>前一位的前后缀匹配长度</font>)已知 需要在<font color='red'>前一位子串中</font>找 <font color='red'>匹配位置</font> 的<font color='green'>下一项</font>, 即可回溯. 

当j = - 1 时说明不存在前后缀匹配了, 判断与初始位置是否相同后赋值即可.

**不断回溯找前缀的 后一位 比较**

![Image text](https://github.com/lothifia/Log/blob/main/readme.assets/61B2DA6A4975E890E866C3114DCCB72A.png)

### 4.22 前缀和

#### LC [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)(一维前缀)

**前缀和   :  **$Sum_{n} = Sum_{n - 1} + a[n]$

```c++
int prefix[10000]{0}// 从i = 1 开始, 将i = 0 时记为0,简化处理边界条件时判断条件
for(int i = 1; i < nums.size() + 1; i++){
	prefix[i] = nums[i - 1];//
	if(i - 2 >= 0) prefix[i] += prefix[i - 1];
}
```



#### LC [363. 矩形区域不超过 K 的最大数值和](https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k/)(二维前缀)

(**将第一(0)行和第一(0)列初始化0**)

```C++
	int tree[1000][1000]{0};//第0行/列初始0, 便于边界计算
    NumMatrix(vector<vector<int>>& matrix) {
        //方法1 求每行一维前缀和, 每列递加
        for(int i = 1; i <= matrix.size() ; i++){
            int s = 0;
            for(int j = 1; j <= matrix[0].size(); j++){
                s += matrix[i - 1][j - 1];
                tree[i][j] = tree[i - 1][j] + s;
            }
        }
    }
	//方法2
    int tree[1000][1000]{0};
    NumMatrix(vector<vector<int>>& matrix) {
        for(int i = 1; i <= matrix.size() ; i++){
            for(int j = 1; j <= matrix[0].size(); j++){
                tree[i][j] = matrix[i - 1][j - 1] + tree[i - 1][j] + tree[i][j - 1] - tree[i - 1][j - 1];//图解
            }
        }
    }
```

方法2 图

[Image text](https://github.com/lothifia/Log/blob/main/readme.assets/2devision_updata.png)



**求目标区域函数**

```c++

int sumRegion(int row1, int col1, int row2, int col2) {
   int res = 0;
   res = tree[row2 + 1][col2 + 1] - tree[row1][col2 + 1] - tree[row2 + 1][col1] + tree[row1][col1];
    return res;}
```

解法 图

[Image text](https://github.com/lothifia/Log/blob/main/readme.assets/2devision.png)

#### LC [304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)(二维前缀, 遍历优化)

##### 

