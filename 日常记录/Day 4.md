# 算法训练营 | Day 4 |  24. 两两交换链表中的节点, 19.删除链表的倒数第N个节点, 面试题 02.07. 链表相交, 142.环形链表II
[ 中文 | [English](../daily_record/Day%204.md) \]

---

##  <div align="center">:smile: 24. 两两交换链表中的节点</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/swap-nodes-in-pairs/">力扣题目</a>
  | <a href="https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1YT411g7br">视频解析</a> |
</p>

**:clock10: 时长：1 小时**

**:thinking: 最初想法**

没有想法

**:bulb: 资料解析**

使用虚拟头结点，要不然每次针对头结点（没有前一个指针指向头结点），还要单独处理。初始时，cur指向虚拟头结点，然后进行三步交换，需要设置temp保存节点，需要看文章解析里面的图。

**方法一：** 迭代法

```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy_head = ListNode(next=head) # 设置一个虚拟头结点
        current = dummy_head  # 将虚拟头结点指向head，这样方面后面做删除操作
        
        # 必须有cur的下一个和下下个才能交换，否则说明已经交换结束了
        while current.next and current.next.next:
            temp = current.next # 记录临时节点, 防止节点修改
            temp1 = current.next.next.next # 记录临时节点
            
            current.next = current.next.next # 步骤一
            current.next.next = temp # 步骤二
            temp.next = temp1 # 步骤三

            current = current.next.next # cur移动两位，准备下一轮交换
        return dummy_head.next
```

**方法二：** 递归法
```python
# 时间复杂度：O(n)
# 空间复杂度：O(n)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # 待翻转的两个node分别是pre和cur
        pre = head
        cur = head.next
        next = head.next.next
        
        cur.next = pre  # 交换
        pre.next = self.swapPairs(next) # 将以next为head的后续链表两两交换
         
        return cur
```

**:point_down: 难点** 

操作多个指针很容易乱，而且要操作的先后顺序也比较复杂

**:pencil: 问题总结**
- 思路不难，难的是如何实现指针的操作和操作的先后顺序，以及定义临时变量记录原来的节点
- 提交后说方法一超出时间限制

##  <div align="center">:smile: 19. 删除链表的倒数第N个节点</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/remove-nth-node-from-end-of-list/">力扣题目</a>
  | <a href="https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1vW4y1U7Gf/">视频解析</a> |
</p>

**:clock10: 时长：30 分钟**

**:thinking: 最初想法**

没想到好的办法

**:bulb: 资料解析**

看了思路后，大为震惊，这也行？

这道题是双指针的经典应用，如果要删除倒数第n个节点，让fast移动n步，然后让fast和slow同时移动，直到fast指向链表末尾。删掉slow所指向的节点就可以了。分为如下几步：
- 使用虚拟头结点
- 定义fast指针和slow指针，初始值为虚拟头结点
- fast首先走n + 1步 ，为什么是n+1呢，因为只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作）
- fast和slow同时移动，直到fast指向末尾
- 删除slow指向的下一个节点

```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 创建一个虚拟节点，并将其下一个指针设置为链表的头部
        dummy_head = ListNode(0, head)
        
        # 创建两个指针，慢指针和快指针，并将它们初始化为虚拟节点
        slow = fast = dummy_head
        
        # 快指针比慢指针快 n+1 步
        for i in range(n+1):
            fast = fast.next
        
        # 移动两个指针，直到快速指针到达链表的末尾
        while fast:
            slow = slow.next
            fast = fast.next
        
        # 通过更新第 (n-1) 个节点的 next 指针删除第 n 个节点
        slow.next = slow.next.next
        
        return dummy_head.next
```

**:point_down: 难点** 

如何一趟扫描实现，双指针如何指定是解题的关键


##  <div align="center">:smile: 面试题 02.07. 链表相交</div>
<div align="center">此题同
<a href="https://leetcode.cn/problems/intersection-of-two-linked-lists/">160.链表相交</a>
</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/">力扣题目</a>
  | <a href="https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html">文章解析</a> |
</p>

**:clock10: 时长：1 小时**

**:thinking: 最初想法**

没有想法

**:bulb: 资料解析**

简单来说，就是求两个链表交点节点的指针。交点不是数值相等，而是指针相等。
- 利用了与上一题类似的指针移动方式，curA指向链表A的头结点，curB指向链表B的头结点。
- 求出两个链表的长度，并求出两个链表长度的差值，然后让curA移动到，和curB 末尾对齐的位置
- 比较curA和curB是否相同，如果不相同，同时向后移动curA和curB，如果遇到curA == curB，则找到交点。
- 否则循环退出返回空指针。

**方法一：** 求两个链表的长度差，同时出发
```python
# 时间复杂度：O(n + m)
# 空间复杂度：O(1)

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lenA, lenB = 0, 0
        cur = headA
        while cur:         # 求链表A的长度
            cur = cur.next 
            lenA += 1
        cur = headB 
        while cur:         # 求链表B的长度
            cur = cur.next 
            lenB += 1
        curA, curB = headA, headB
        if lenA > lenB:     # 让curB为最长链表的头，lenB为其长度
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA 
        for _ in range(lenB - lenA):  # 让curA和curB在同一起点上（末尾位置对齐）
            curB = curB.next 
        while curA:         #  遍历curA 和 curB，遇到相同则直接返回
            if curA == curB:
                return curA
            else:
                curA = curA.next 
                curB = curB.next
        return None 
```

**方法二：** 求两个链表的长度差，同时出发(代码封装版本)
```python
# 时间复杂度：O(n + m)
# 空间复杂度：O(1)

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lenA = self.getLength(headA)
        lenB = self.getLength(headB)
        
        # 通过移动较长的链表，使两链表长度相等
        if lenA > lenB:
            headA = self.moveForward(headA, lenA - lenB)
        else:
            headB = self.moveForward(headB, lenB - lenA)
        
        # 将两个头向前移动，直到它们相交
        while headA and headB:
            if headA == headB:
                return headA
            headA = headA.next
            headB = headB.next
        
        return None
    
    def getLength(self, head: ListNode) -> int:
        length = 0
        while head:
            length += 1
            head = head.next
        return length
    
    def moveForward(self, head: ListNode, steps: int) -> ListNode:
        while steps > 0:
            head = head.next
            steps -= 1
        return head
```
**方法三：** 等比例法
```python
# 时间复杂度：O(n + m)
# 空间复杂度：O(1)

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        # 处理边缘情况
        if not headA or not headB:
            return None
        
        # 在每个链表的头部初始化两个指针
        pointerA = headA
        pointerB = headB
        
        # 遍历两个链表直到指针相交
        while pointerA != pointerB:
            # 将指针向前移动一个节点
            pointerA = pointerA.next if pointerA else headB
            pointerB = pointerB.next if pointerB else headA
        
        # 如果相交，指针将位于交点节点，如果没有交点，值为None
        return pointerA
```


##  <div align="center">:smile: 142. 环形链表II</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/linked-list-cycle-ii/">力扣题目</a>
  | <a href="https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1if4y1d7ob">视频解析</a> |
</p>

**:clock10: 时长：1 小时**

**:thinking: 最初想法**

不知道环形链表怎么处理，细节多，有点复杂

**:bulb: 资料解析**

此题分两步走：
1. 判断链表是否有环

使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，**如果 fast 和 slow指针在途中相遇 ，说明这个链表有环**。

fast走两个节点，slow走一个节点,如果链表有环，fast指针先进入环内，开始绕圈，后slow指针进入环内，当slow没有走完一圈的时候，fast指针走得快，一定超越slow指针，一定会在环内相遇。

2. 如果有环，如何找到这个环的入口

假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。那么相遇时： slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：`(x + y) * 2 = x + y + n (y + z)`

两边消掉一个（x+y）: `x + y = n (y + z)`

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：`x = n (y + z) - y`

整理公式之后为如下公式：`x = (n - 1) (y + z) + z`

这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。
- 如果 n = 1 ，公式化简为 x = z ，意思是**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。
- 如果 n > 1 ，意思是fast指针在环形转n圈之后才遇到 slow指针，和n为1的时候 效果是一样的，一样可以通过这个方法找到 环形的入口节点。

**方法一：** 快慢指针法

```python
# 时间复杂度：O(n),快慢指针相遇前，指针走的次数小于链表长度，快慢指针相遇后，两个index指针走的次数也小于链表长度，总体为走的次数小于 2n
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        slow = head
        fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
            # If there is a cycle, the slow and fast pointers will eventually meet
            if slow == fast:
                # Move one of the pointers back to the start of the list
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        # If there is no cycle, return None
        return None
```
**方法二：** 集合法
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        visited = set()
        
        while head:
            if head in visited:
                return head
            visited.add(head)
            head = head.next
        
        return None
```


**:point_down: 难点** 

如何判断链表是否有环，如何通过设置快慢指针找到这个环的入口

**:pencil: 问题总结**

- 通过文章解析和视频讲解后，思路特别清晰，能够自己根据思路写出代码。
- 第二种方法，集合法更简单，完全想不到

---

## :trophy: 收获
**:white_check_mark: 学习时长**
- 解决题目+写笔记：**3 小时 30 分钟**

**:white_check_mark: 心得**

链表相对于昨天的题，上难度了。齐上演各类不同的双指针法，练习太少了，不能举一反三。尤其是环形链表II，这道题的解题思路运用到了数学思想，细节繁多，虽然代码比较简单，但是思路细节需要消化。