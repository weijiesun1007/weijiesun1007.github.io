# [每日一题] 2404.出现最频繁的偶数元素


Leetcode 每日一题

2404.出现最频繁的偶数元素

Newcoder 类似题目

HJ23.删除字符串中出现次数最少的字符
<!--more-->

# 每日一题 2023.04.13

**题目描述：**

给你一个整数数组 `nums` ，返回出现最频繁的偶数元素。

如果存在多个满足条件的元素，只需要返回 **最小** 的一个。如果不存在这样的元素，返回 `-1` 。



**题目链接：**[1019.链表中的下一个更大节点](https://leetcode.cn/problems/most-frequent-even-element/)

示例 1：

    输入：nums = [0,1,2,2,4,4,1]
    输出：2
    解释：数组中的偶数元素为 0、2 和 4 ，在这些元素中，2 和 4 出现次数最多。返回最小的那个，即返回 2 。

示例 2：

    输入：nums = [4,4,4,9,2,4]
    输出：4
    解释：4 是出现最频繁的偶数元素。

示例 3：

    输入：nums = [29,47,21,41,13,37,25,7]
    输出：-1
    解释：不存在偶数元素。


**题目难度：** 简单

**题目解读：**

1. 需要记录偶数元素和元素出现个数对，类似于记录一个哈希表，于是通过最近刷题经验，可以想到可以利用STL的map容器解决；
  
2. 需要注意的是map容器的操作以及map容器是天然按照第一个值的（pair左边的值）字典序进行排列的，该值是map的主键，是不可以重复出现的，如果是int型的就会按照第一个值的大小从小到大排列。

3. 由于map是按主键大小从小到大排列的，所以根据题目要求输出value值相同的较大主键值，应当从右往左使用迭代器遍历map。

### 代码实现

```c++
class Solution {
public:
    int mostFrequentEven(vector<int>& nums) {
        map<int,int>m;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]%2==0)
            {
                m[nums[i]]++;
            }
        }
        map<int,int>::iterator it;
        int res=-1;
        int max=0;
        for(auto it=m.rbegin();it!=m.rend();it++)//使用迭代器从右往左遍历
        {
            //cout<<it->first<<" "<<endl;
            //例如对于示例1 map里是[[2,2],[4,2]]
            //对于示例2 map里存放的是[[2,1],[4,4]]
            if(it->second>=max)
            {
                max=it->second;
                res=it->first;
            }
        }
        return res;
    }
};
```

<br/>

### 类似题目

该题与Newcoder里的一题很像，放在一起看看叭！

**题目描述：**

实现删除字符串中出现次数最少的字符，若出现次数最少的字符有多个，则把出现次数最少的字符都删除。输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。

**输入描述：** 

保证输入的字符串仅出现小写字母，不考虑非法输入。

**输出描述：** 

删除字符串中出现次数最少的字符后的字符串。

**题目链接：**[HJ23.删除字符串中出现次数最少的字符 ](https://www.nowcoder.com/practice/05182d328eb848dda7fdd5e029a56da9?tpId=37&tqId=21246&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj&difficulty=undefined&judgeStatus=undefined&tags=&title=)

示例 1：

    输入： aabcddd
    输出： aaddd

**题目难度：** 简单

**解题思路：**

主要注意map容器的操作，这题是将出现次数最少的都删除，所以从左往右遍历即可。

### 代码实现

```c++
#include <algorithm>
#include <iostream>
#include <map>
#include <string>
#include <vector>
using namespace std;

bool cmp(const pair<char,int> a,const pair<char,int> b)
{
    return a.second>b.second;
}

int main() {
    string str;
    cin>>str;
    map<char,int> m;
    map<char,int>::iterator it;
    for(int i=0;i<str.size();i++)
    {
        m[str[i]]++;
    }
    int min=m[str[0]];
    for(int i=0;i<str.size();i++)
    {
        if(m[str[i]]<min)
            min=m[str[i]];//找到最小的次数
    }
    for(int i=0;i<str.size();i++)
    {
        if(m[str[i]]>min)
            cout<<str[i];
    }
    cout<<endl;
    return 0;

}
```
