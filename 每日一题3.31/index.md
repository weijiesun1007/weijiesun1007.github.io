# [每日一题] 2367.算术三元组的数目


Leetcode 每日一题

2367.算术三元组的数目
<!--more-->

# 每日一题 2023.03.31

**题目描述：**

给你一个下标从 `0` 开始、严格递增 的整数数组 `nums` 和一个正整数 `diff` 。

如果满足下述全部条件，则三元组 `(i, j, k)` 就是一个 `算术三元组` ：

+ i < j < k ，
+ nums[j] - nums[i] == diff 
+ nums[k] - nums[j] == diff

返回不同 `算术三元组` 的数目。


**题目链接：**[2367.算术三元组的数目](https://leetcode.cn/problems/number-of-arithmetic-triplets/)

示例 1：

    输入：nums = [0,1,4,6,7,10], diff = 3
    输出：2
    解释：
    (1, 2, 4) 是算术三元组：7 - 4 == 3 且 4 - 1 == 3 。
    (2, 4, 5) 是算术三元组：10 - 7 == 3 且 7 - 4 == 3 。

示例 2：

    输入：nums = [4,5,6,7,8,9], diff = 2
    输出：2
    解释：
    (0, 2, 4) 是算术三元组：8 - 6 == 2 且 6 - 4 == 2 。
    (1, 3, 5) 是算术三元组：9 - 7 == 2 且 7 - 5 == 2 。

**题目难度：** 简单

**题目解读：**

因为本题为严格递增数组，不存在重复元素，故只需要遍历一遍数组，寻找该列表中是否有该元素对应的后两个等差数即可

即对`nums`中各个元素`n`，判断是否存在`n+diff`和`n+2*diff`，如有则计数`res`加`1`

### 代码实现

```python
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        res = 0
        for n in nums:
            if n+diff in nums and n+diff+diff in nums:
                res += 1
        return res
```


