# 记录

------

[TOC]



### 21/4/18

关于C++ <font color='red'>set | unorderedset</font> 学习

```c++
set<int> set_test;//支持二分查找, 插入 删除
set_test.lower_bound(begin, end, num);//找到set内部第一个 大于num的值 没有符合条件的则找到set_test.end();
set_test.lower_bound(begin, end, num);//找到set内部第一个 大于num的值
set_test.insert(num);//插入一个
set_test.erase(num| iterator)//根据数值或者 地址来删除
    //set -> 顺序存储 unorderedset ->非顺序存储(hash表)
    
```

set 用于优化 <font color='green'>滑动窗口</font> 

![Difference](20191106171908375.png)

#### Leetcode 220 重复元素: 

https://leetcode-cn.com/problems/contains-duplicate-iii/