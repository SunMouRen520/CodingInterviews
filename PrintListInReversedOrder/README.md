# 题目 从尾到头打印链表
# 题目描述

输入一个链表的头结点，从尾到头反过来打印出每个节点的值。链表节点定义如下：
```
struct ListNode
{
    int   m_nKey;
    ListNode* m_pNext;
};
```

# 解题思路
此题与LeetCode-206题反转链表类似。

思路1：此题要求打印链表每个节点的值，由于不用考虑链表的生成（其实有点复杂），所以我们直接在函数中依次将链表中的节点值取出来，保存至一个容器（对于C++）或者列表（对于Python）中；然后再逆序打印这个容器或者列表即可。

思路2：思路1其实偷懒了，因为严格意义上来说，既然是对于链表来操作，那么其实返回结果其实也应该是链表，而不应该是简单的容器或者列表。按这样理解，我们其实应该直接对链表进行逆序打印，而不是转换成中间变量来操作。

分析：

  1). 若链表为空或只有一个元素，则直接返回；

  2). 设置两个前后相邻的指针p,q. 将p所指向的节点作为q指向节点的后继；

  3). 重复2),直到q为空

  4). 调整链表头和链表尾


# 代码

- [C++](./PrintListInReversedOrder.cpp)


- [Python](./PrintListInReversedOrder.py)

**部分重要思想总结：**

- 向链表从后向前添加元素：将新加节点的next指向当前链表（即在首部加节点），生成新的链表，再赋值给当前链表即可
- 向链表从前向后添加元素：找到当前链表的最后节点，将当前链表最后节点的next指向新的节点，即实现链表的扩展，此时不需要重新赋值（因为是在原来的链表上添加的）

## C++
### 方法一：保存至容器，然后逆序打印
```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> res;
        ListNode* p = head;
        if (p==NULL) return res;
        while(p!=NULL)
        {
            res.push_back(p->val);
            p = p->next;
        }
        // 逆序方法1：利用c++自带的库函数reverse
        reverse(res.begin(),res.end());
        
        // 逆序方法2：利用for循环
        // vector<int> temp;
        // for (int i=res.size()-1;i>=0;i--)
        //{
        //    temp.push_back(res[i]);
        //}
        // return temp;
        
        // 逆序方法3：vector迭代器
        //for (vector<int>::reverse_iterator it = res.rbegin(); it!= res.rend(); it++)
        {
        //    temp.push_back(*it);
        //}
        // return temp;
        
        // 逆序方法4：栈
        //std::stack<ListNode*> nodes;
        //ListNode *pNode = head;
        //while(pNode!=NULL)
        //{
        //    nodes.push(pNode);
        //    pNode = pNode->next;
        //}
        //while(!nodes.empty())
        //{
        //    pNode = nodes.top();
        //    temp.push_back(pNode->val);
        //    nodes.pop();
        //}
        // return temp;
        return res;
    }
};
```

### 方法二：原地修改链表(**重要**)
```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> res;
        if (head==NULL)
            return res;
        ListNode* prev = head;
        ListNode* cur = head->next;
        ListNode* temp = head->next->next;
        while(cur)
        {
            temp = cur->next;//temp作为中间节点，记录当前结点的下一个节点的位置
            cur->next = prev;//当前结点指向前一个节点
            prev = cur;//指针后移
            cur = temp;// 指针后移,处理下一个节点
        }
        head->next = NULL; //while结束后，将翻转后的最后一个节点（即翻转前的第一个结点head）的链域置为NULL
        while(prev)
        {
            res.push_back(prev->val);
            prev = prev->next;
        }
        return res;
    }
};
```

## Python
### 方法一：保存为列表，然后逆序打印
```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        res = []
        p = listNode
        while p:
            res.append(p.val)
            p = p.next
        ## 列表反转的两种方法
        res = res[::-1]
        # res.reverse()
        return res
```
###  方法二：原地旋转链表(**重要**)
```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, head):
        # write code here
        res = []
        if head == None:
            return res
        prev = head
        cur = head.next
        temp = head.next.next
        while cur:
            temp = cur.next
            cur.next = prev
            prev = cur
            cur = temp
        head.next = None
        while prev:
            res.append(prev.val)
            prev = prev.next
        return res
```
# 参考
   - [LeetCode-206-反转链表](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Reverse-Linked-List/README.md)
