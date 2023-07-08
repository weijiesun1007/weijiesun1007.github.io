# [每日一题] 1019.链表中的下一个更大节点


Leetcode 每日一题

1019.链表中的下一个更大节点
<!--more-->

# 每日一题 2023.04.10

**题目描述：**

给定一个长度为 n 的链表 `head`，对于列表中的每个节点，查找下一个更大节点的值。也就是说，对于每个节点，找到它旁边的第一个节点的值，这个节点的值 `严格大于` 它的值。

返回一个整数数组 `answer` ，其中 `answer[i]` 是第 `i` 个节点( 从1开始 )的下一个更大的节点的值。如果第 `i` 个节点没有下一个更大的节点，设置 `answer[i] = 0` 。

**题目链接：**[1019.链表中的下一个更大节点](https://leetcode.cn/problems/next-greater-node-in-linked-list/)

![](images/每日一题/0410/链表下一个最大节点.png)

**题目难度：** 中等

## 循环双指针

**题目解读：**

1. 没什么想法，感觉两次循环是最容易想到的方法，虽然效率肯定不怎么样，但能做出来就行吧！
  
2. 那么就用两个tmp节点来进行循环，tmp1节点从head链表的第一个节点开始，为head链表里的每个节点寻找下一个更大节点；
  
3. tmp2节点从tmp1的下一个节点开始，一直到链表尾部，直到找到第一个值比tmp1节点值更大的节点，将该节点的val属性值存入res容器的对应位置；
  
4. 维护一个布尔值find，记录当前tmp1节点是否能够找到下一个更大节点，如果tmp2循环至链表尾部仍没有找到，则对应的find值为false，直接在res容器的对应位置存个0即可，代表该节点没有下一个更大节点。
  
### 代码实现

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        vector<int> res;
        ListNode* tmp1=head;
        while(tmp1!=NULL)
        {
            bool find=false;
            ListNode *tmp2=tmp1->next;
            while(tmp2!=NULL)
            {
                if(tmp2->val>tmp1->val)
                {
                    res.push_back(tmp2->val);
                    find=true;
                    break;
                }
                tmp2=tmp2->next;
            }
            if(!find)
                res.push_back(0);
            tmp1=tmp1->next;
        }
        return res;
    }
};
```

## 单调栈方法

对一个数，想要寻找下一个比他更大的元素，都可以使用**单调栈**的方法

具体思路如下：

初始化两个列表stack和res：
+ stack表示栈底到栈顶单调递减的栈，存储形式为某节点的值和序号(value, index)
+ res用于返回最终结果，第i个元素即为第i个节点下一个比他更大的元素的值

之后，从头至尾遍历链表各个节点，如当前节点的值大于单调栈栈顶的值，说明当前节点为栈顶元素下一个比他更大的元素

因为单调栈中元素自底向上依次递减，所以轮流比较弹出上一元素后新的栈顶元素与当前节点值大小，直至栈空或当前值小于栈顶元素

有点类似于滑动窗口收缩左节点的意思，当遍历完所有节点后，还在栈中的元素在res中对应值均为默认的0，即未找到下一个比他更大的元素

本题在此基础上还有递归的方法，但目前还没学明白，之后会对回溯递归进一步学习了解

**参考链接：** [单调栈官方题解](https://leetcode.cn/problems/next-greater-node-in-linked-list/solution/lian-biao-zhong-de-xia-yi-ge-geng-da-jie-u9yo/)

### 代码实现
```python
class Solution:
    def nextLargerNodes(self, head: Optional[ListNode]) -> List[int]:
        index = -1
        cur = head
        stack = []
        res = []
        while cur:
            index += 1
            res.append(0)
            while stack and stack[-1][0] < cur.val:
                res[stack[-1][1]] = cur.val
                stack.pop()
            stack.append((cur.val, index))
            cur = cur.next
        return res
```

