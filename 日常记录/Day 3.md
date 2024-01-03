# 算法训练营 | Day 3 |  203.移除链表元素, 707.设计链表, 206.反转链表 
[ 中文 | [English](../daily_record/Day%203.md) \]

---
##  <div align="center">:smile: 203.移除链表元素</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/remove-linked-list-elements/">力扣题目</a>
  | <a href="https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV18B4y1s7R9">视频解析</a> |
</p>

**:clock10: 时长：1 小时**

**:thinking: 最初想法**

这道题很简单，思路也比较清晰，单链表删除节点，直接将当前节点的next指针指向要删除的节点的下一个节点就可以了。但是是否删除头节点的情况要分开处理，因为单链表只能指向下一个节点，删除头节点的话如何处理。同时还要弄清楚循环的条件。

**:bulb: 资料解析**

**方法一：** 直接使用原来的链表来进行删除操作
- 移除头节点：头结点没有前一个节点，所以将头结点向后移动一位，这样就从链表中移除了一个头结点。
- 移除其他节点：通过前一个节点指向下一个节点，来移除当前节点

```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 删除头节点
        while head and head.val == val:
            head = head.next
        # 删除非头节点
        cur = head
        while cur and cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur =  cur.next
        return head
```
**方法二：** 设置一个虚拟头结点再进行移除节点操作
- 设置虚拟头节点为新的头节点，如果要移除旧的头节点，直接将新的头节点指向旧的头节点的下一个节点就可以的，这样就将移除头节点和其他节点的方式统一了。
- return 头结点的时候，return dummyNode->next，这才是新的头结点

```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
        
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```


**:point_down: 难点** 

需要单独写一段逻辑来处理移除头节点的情况，可以思考一个统一的逻辑（设置虚拟头节点）来移除。

**:pencil: 问题总结**

- 用python就不用手动清理内存了，如果使用C++，需要手动在内存中删除这个节点。

##  <div align="center">:smile: 707.设计链表</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/design-linked-list/">力扣题目</a>
  | <a href="https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1FU4y1X7WD">视频解析</a> |
</p>

**:clock10: 总时长：1 小时 30 分钟**

**:thinking: 最初想法**

需要自己设计MyLinkedList类的功能：
- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

**:bulb: 资料解析**

这道题目设计链表的五个接口，已经覆盖了链表的常见操作，是练习链表操作非常好的一道题目。跟上一道题一样，链表操作的两种方式：1. 直接使用原来的链表来进行操作。2. 设置一个虚拟头结点再进行操作。先使用虚拟头节点方法实现单链表，再直接使用原来的链表实现双链表。

**单链表：** 

```python
# 时间复杂度：涉及 index 的相关操作为 O(index), 其余为 O(1)
# 空间复杂度：O(1)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        current = self.dummy_head.next
        for i in range(index):
            current = current.next
            
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = ListNode(val, current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = current.next.next
        self.size -= 1

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

**双链表：** 

```python
# 时间复杂度：涉及 index 的相关操作为 O(index), 其余为 O(1)
# 空间复杂度：O(1)

class ListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        if index < self.size // 2:
            current = self.head
            for i in range(index):
                current = current.next
        else:
            current = self.tail
            for i in range(self.size - index - 1):
                current = current.prev
                
        return current.val

    def addAtHead(self, val: int) -> None:
        new_node = ListNode(val, None, self.head)
        if self.head:
            self.head.prev = new_node
        else:
            self.tail = new_node
        self.head = new_node
        self.size += 1

    def addAtTail(self, val: int) -> None:
        new_node = ListNode(val, self.tail, None)
        if self.tail:
            self.tail.next = new_node
        else:
            self.head = new_node
        self.tail = new_node
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        if index == 0:
            self.addAtHead(val)
        elif index == self.size:
            self.addAtTail(val)
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index - 1):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index):
                    current = current.prev
            new_node = ListNode(val, current, current.next)
            current.next.prev = new_node
            current.next = new_node
            self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        if index == 0:
            self.head = self.head.next
            if self.head:
                self.head.prev = None
            else:
                self.tail = None
        elif index == self.size - 1:
            self.tail = self.tail.prev
            if self.tail:
                self.tail.next = None
            else:
                self.head = None
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index - 1):
                    current = current.prev
            current.prev.next = current.next
            current.next.prev = current.prev
        self.size -= 1

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

**:point_down: 难点** 

每个功能的实现不难，主要是注意细节。双链表涉及太多的条件判断，少许有点复杂。

**:pencil: 问题总结**
- 思路比较简单，挨个实现五个接口就行。


##  <div align="center">:smile: 206.反转链表</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/reverse-linked-list/">力扣题目</a>
  | <a href="https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1nB4y1i7eL">视频解析</a> |
</p>

**:clock10: 总时长：1 小时 30 分钟**

**:thinking: 最初想法**

没想出来

**:bulb: 资料解析**

改变链表的next指针的指向，直接将链表反转 ，而不用重新定义一个新的链表。

**方法一：** 双指针法
- 首先定义一个`cur`指针，指向头结点，再定义一个`pre`指针，初始化为`None`
- 用`tmp`指针保存一下保存当前节点`cur->next`,因为接下来要改变`cur->next`的指向了,将`cur->next`指向`pre`，此时已经反转了第一个节点了
- 循环移动`pre`和`cur`指针，当`cur`指向`None`时，循环结束，链表反转完毕
- return pre指针，pre指针就指向了新的头结点
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while cur:
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
```

**方法二：** 递归法（从前往后）

```python
# 时间复杂度：O(n), 要递归处理链表的每个节点
# 空间复杂度：O(n), 递归调用了 n 层栈空间

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        return self.reverse(head, None)
    def reverse(self, cur: ListNode, pre: ListNode) -> ListNode:
        if cur == None:
            return pre
        temp = cur.next
        cur.next = pre
        return self.reverse(temp, cur)
```
**方法三：** 使用虚拟头结点解决链表反转(头插法)
```python
# 时间复杂度：O(n), 要递归处理链表的每个节点
# 空间复杂度：O(1), 不需要栈

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        # 创建虚头结点
        dummyHead = ListNode(-1, None)
        # 遍历所有节点
        cur = head
        while cur:
            temp = cur.next
            # 头插法
            cur.next = dummyHead.next
            dummyHead.next = cur
            cur = temp
        return dummyHead.next
```

**:point_down: 难点** 

不知道该怎么反转

**:pencil: 问题总结**
- 用之前一直常用的**双指针法（快慢指针）**，同样的思路也可以使用**递归法**实现

---

## :trophy: 收获
**:white_check_mark: 学习时长**
- 解决题目+写笔记：**4 小时**

**:white_check_mark: 心得**

今天链表的题目思路比较简单，理解起来很容易，但是第二题需要耐心、仔细地实现不同的接口。