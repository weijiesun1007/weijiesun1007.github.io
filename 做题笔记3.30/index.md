# 快来跟小猪崽一起做题叭！

小猪崽来做题啦！

904.水果成篮——滑动窗口

11.盛最多水的容器——双指针

9.回文数——数字翻转


<!--more-->

# 904. 水果成篮

**题目描述：**

你正在探访一家农场，农场从左到右种植了一排果树。

这些树用一个整数数组 `fruits` 表示，其中 `fruits[i]` 是第 `i` 棵树上的水果种类 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

你只有 `两个` 篮子，并且每个篮子只能装 `单一类型` 的水果。每个篮子能够装的水果总量没有限制。

你可以选择任意一棵树开始采摘，你必须从 每棵 树（包括开始采摘的树）上 恰好摘一个水果 。

采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。

一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 `fruits`，返回你可以收集的水果的 最大 数目。


**题目链接：** [904. 水果成篮 - 力扣（LeetCode）](https://leetcode.cn/problems/fruit-into-baskets/)

注意：

对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。

如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

**题目难度：** 中等

示例 1：

    输入：fruits = [1,2,1]
    输出：3
    解释：可以采摘全部 3 棵树。

示例 2：

    输入：fruits = [0,1,2,2]
    输出：3
    解释：可以采摘 [1,2,2] 这三棵树。
    如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。

示例 3：

    输入：fruits = [1,2,3,2,2]
    输出：4
    解释：可以采摘 [2,3,2,2] 这四棵树。
    如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。

示例 4：

    输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
    输出：5
    解释：可以采摘 [1,2,1,1,2] 这五棵树。
 

**参考链接：**
[代码随想录 (programmercarl.com)](https://www.programmercarl.com/0209.长度最小的子数组.html#滑动窗口)

### 思路分析
滑动窗口的思想，以终止指针为循环遍历索引，依据水果种类数量是否达到上限作为数据更新依据

当子序列中水果种类小于`2`时，直接采摘当前水果；

当子序列中水果种类等于`2`时，如果已有当前水果种类，继续采摘；

如果没有，记录此时水果数量，左边界进行收缩，起始指针右移，直至水果种类数量减少

### 代码实现
```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        i = 0
        res = 0
        classCnt = 0
        fruitClass = {}
        for j in range(len(fruits)):
            if fruits[j] in fruitClass.keys():
                fruitClass[fruits[j]] += 1
            elif classCnt < 2:
                fruitClass[fruits[j]] = 1
                classCnt += 1
            else:
                res = max(res, j-i)
                while classCnt == 2:
                    fruitClass[fruits[i]] -= 1
                    if fruitClass[fruits[i]] == 0:
                        del fruitClass[fruits[i]]
                        classCnt -= 1
                    i += 1
                fruitClass[fruits[j]] = 1
                classCnt += 1           
        return max(res, j-i+1)
```

### 本题知识点
+ `Counter()` 函数 类似于此前接触到的`collecions.defaultdict(int)`，可对字典进行整型默认赋值，还可以将list中各元素按照计次进行排序
+ 滑动窗口进行起始指针更新时，常用`while循环`进行收缩
+ 不考虑优化的情况下，可以在每一次循环最后，进行一次结果`res`的更新


# 11. 盛最多水的容器

**题目描述：**

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

说明：你不能倾斜容器。

**题目链接：** [11. 盛最多水的容器 - 力扣（LeetCode）](https://leetcode.cn/problems/container-with-most-water)


**题目难度：** 中等

示例 1：

    输入：[1,8,6,2,5,4,8,3,7]
    输出：49 
    解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例 2：

    输入：height = [1,1]
    输出：1。

### 思路分析
相较于暴力求解每种可能，在左、右边界设置双指针，进行剪枝排除不可能的情况更优

解题的核心思想即是在每个状态下，无论长板或短板向中间收窄一格，都会导致水槽 底边宽度 变短：

若**向内**移动**短板** ，水槽的短板`min(h[i],h[j])` 可能变大，因此下个水槽的面积*可能增大*。

若**向内**移动**长板** ，水槽的短板`min(h[i],h[j])​` 不变或变小，因此下个水槽的面积*一定变小*。

因此，初始化双指针分列水槽左右两端，循环每轮将短板向内移动一格，并更新面积最大值，直到两指针相遇时跳出；即可获得最大面积。

**参考链接：**
https://leetcode.cn/problems/container-with-most-water/solution/container-with-most-water-shuang-zhi-zhen-fa-yi-do/

### 代码实现
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        i = 0
        j = len(height)-1
        res = 0
        while i < j:
            if height[i] <= height[j]:
                res = max(res, height[i]*(j-i))
                i += 1
            else:
                res = max(res, height[j]*(j-i))
                j -= 1
            
        return res
```
### 本题知识点
+ 双指针置于左右边界 向内收缩 与 将包含正负数的有序数组的各元素的平方进行排序 异曲同工


# 9. 回文数

**题目描述：**

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

例如，`121` 是回文，而 `123` 不是。

**题目链接：** [9. 回文数 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-number)

**题目难度：** 简单
 
示例 1：

    输入：x = 121
    输出：true

示例 2：

    输入：x = -121
    输出：false
    解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

示例 3：

    输入：x = 10
    输出：false
    解释：从右向左读, 为 01 。因此它不是一个回文数。
 

### 思路分析
如果没有限制，直接将整形转换为字符串，字符串转换列表，列表翻转，相同即为回文数。

### 代码实现
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        else:
            s = str(x)
            l = list(s)
            t = []
            for e in l:
                t.append(e)
            l.reverse()
            if t == l:
                return True
            else:
                return False
```

### 进阶限制
如果考虑限制，不可以将整型转为字符串，则考虑数学方法进行各数位提取，考虑到优化和部分语言溢出的可能，只需翻转一半即可

如何判断是否已经翻转一半，可以依据剩余数字是否小于已翻转数字

考虑到回文数数字个数奇偶性，在最后结果的判定上，考虑奇数个数位的情况

**参考链接：**https://leetcode.cn/problems/palindrome-number/solution/dong-hua-hui-wen-shu-de-san-chong-jie-fa-fa-jie-ch/

### 代码实现
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if (x < 0 or (x % 10 == 0 and x != 0)):
            return False
        revertedNumber = 0
        while (x > revertedNumber):
            revertedNumber = revertedNumber * 10 + x % 10
            x = x // 10
        return x == revertedNumber or x == revertedNumber // 10;
```

### 本题知识点
+ 数字对10求模，是提取整型各数位数字的常用方法
+ 依据剩余数字是否小于已翻转数字，很巧妙的判断方法！
+ 最后考虑到回文数字的奇偶性，对返回结果进行补充判断
