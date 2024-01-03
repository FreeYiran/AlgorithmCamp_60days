# 算法训练营 | Day 2 | 977.有序数组的平方, 209.长度最小的子数组, 59.螺旋矩阵II
[ 中文 | [English](../daily_record/Day%201.md) \]

---
##  <div align="center">:smile: 977.有序数组的平方</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/squares-of-a-sorted-array/">力扣题目</a>
  | <a href="https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1QB4y1D7ep/">视频解析</a> |
</p>

**:clock10: 时长：1 小时 30 分钟**

**:thinking: 最初想法**

看到题目，只想到绝对值，把数组分为两部分，以及可能会用到双指针的零散概念，但不知道如何实现。如果把数组元素都平方之后再进行快速排序，那么时间复杂度就是`0(n + nlog(n))`，不符合题目要求。

**:bulb: 资料解析**

看完讲解视频，能够捋清思路并且自行实现了。
- 非递减顺序排序的整数数组`nums`，平方后的大小是两边大、中间小。所以头尾元素进行比较，找出最大值放入新数组`result`最后一位，可以实现非递减顺序。
- 使用双指针的方法，定义`l`，`r`，作为左、右指针从前向后、从后往前遍历，定义`k`指向平方后的数组`result`的最后一个元素的下标。
- `if  nums[l] ** 2 > nums[r] ** 2`，那么，`result[k] = nums[l] ** 2`，左指针`l`自增
- 否则，`result[k] = nums[r] ** 2`，右指针`r`自增。
  
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        l =0  # 左指针
        r = len(nums) - 1  # 右指针
        k = len(nums) - 1  # 存放结果的指针，从最后开始更新，可以实现非递减顺序
        result = [float('inf')] * len(nums)  # 创建了一个只包含一个元素的列表，该元素是正无穷大（infinity）,用来存平方后的元素
        while l <= r:
            if  nums[l] ** 2 > nums[r] ** 2:  # 左右边界进行对比，找出最大值
                result[k] = nums[l] ** 2
                l += 1  # 左指针往右移动
            else:
                result[k] = nums[r] ** 2
                r -= 1  # 右指针往左移动
            k -= 1  # 存放结果的指针需要往前平移一位
        return result
```

**:point_down: 难点** 

  在0(n)的时间复杂度，如何使用双指针进行两端值比较，进行非递减排序。开始时，能想到双指针，但想不到有序数组平方后的规律是两边大、中间小，可以头尾进行比较，从而实现排序。

**:pencil: 问题总结**
1. 使用暴力破解非常简单，但是时间复杂度达不到要求。
2. 寻找有序数组的规律，从而找到突破。
   - 平方后的有序数组的值是两端大中间小。

##  <div align="center">:smile: 209.长度最小的子数组</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/minimum-size-subarray-sum/">力扣题目</a>
  | <a href="https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1tZ4y1q7XE">视频解析</a> |
</p>

**:clock10: 时长：2 小时**

**:thinking: 最初想法**

这道题能想到的也就是使用两层循环，一个控制起始位置，一个控制终止位置，把所有的符合条件的区间都枚举出来。然后判断最小的区间的长度。之前使用双指针较多，所以这道题目第一时间想到了双指针，但是这道题的窗口移动条件没能捋清楚。

**:bulb: 资料解析**

之前自己在思考时运用到了滑动窗口的思想，但一些条件的细节并没有思考清楚。滑动窗口其实也是双指针法的一种。

**方法一：** 暴力法
- 两个for循环，不断的寻找符合条件的子序列
```python
# 时间复杂度：O(n^2)
# 空间复杂度：O(1)
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        l = len(nums)
        min_len = float('inf')
        
        for i in range(l):
            cur_sum = 0
            for j in range(i, l):
                cur_sum += nums[j]
                if cur_sum >= s:
                    min_len = min(min_len, j - i + 1)
                    break
        
        return min_len if min_len != float('inf') else 0
```
**方法二：** 滑动窗口
- 窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组
- 窗口的起始位置如何移动：如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）
- 窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        l = len(nums)
        left = 0
        right = 0
        min_len = float('inf')
        cur_sum = 0 #当前的累加值
        
        while right < l:
            cur_sum += nums[right]
            
            while cur_sum >= s: # 当前累加值大于目标值
                min_len = min(min_len, right - left + 1)
                cur_sum -= nums[left]
                left += 1
            
            right += 1
        
        return min_len if min_len != float('inf') else 0
```

**:point_down: 难点** 

如何移动窗口，比如关键的一步是`cur_sum -= nums[left]`，自然而然地向右加一个新的数字参与计算，指针指向下一个索引时，不用重复进行累加。

**:pencil: 问题总结**
- 本题可以使用**暴力解法**和**滑动窗口**
- 相对于暴力法，在第二层循环都要执行`cur_sum += nums[j]`，滑动窗口只用在第一层while循环执行`cur_sum += nums[right]`,所以时间复杂度只有O(n)。

**:muscle:相关题目**
- [904.水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)
- [76.最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

---

##  <div align="center">:smile: 59.螺旋矩阵II</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/spiral-matrix-ii/">力扣题目</a>
  | <a href="https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1SL4y1N7mV/">视频解析</a> |
</p>

**:clock10: 时长：1 小时 30 分钟**

**:thinking: 最初想法**

这道题首要就是要找出规律，但是并没有发现什么规律，尤其是示例1中螺旋排列的8、9不知道怎么处理。

**:bulb: 资料解析**

- 模拟顺时针画正方形矩阵的过程:
  - 填充上行从左到右
  - 填充右列从上到下
  - 填充下行从右到左
  - 填充左列从下到上

坚持**循环不变量原则**，按照左闭右开的原则，设置边界条件来遍历。

```python
# 时间复杂度：O(n^2) 模拟遍历二维矩阵的时间
# 空间复杂度：O(1)
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]  # 创建一个n*n的矩阵
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1

        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums
```


**:point_down: 难点** 

- 如何设置螺旋矩阵的边界条件实现排列

**:pencil: 问题总结**
- 螺旋矩阵对我来说是一个新的概念，首先要理解和发现螺旋矩阵的排列规律，然后遵循**循环不变量**原则，分别设置边界条件
- 并不涉及什么算法，但是不知道螺旋矩阵排列如何模拟，只能先看代码去理解
- 为什么循环次数是n/2？

**:muscle:相关题目**
- [54.删除排序数组中的重复项](https://leetcode.cn/problems/spiral-matrix/)
- [剑指Offer 29.顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)
  
---

## :trophy: 收获
**:white_check_mark: 学习时长**
- 解决题目+写笔记：**5 小时**

**:white_check_mark: 心得**

今天的题目比昨天难以理解。经过两天的数组题目训练，了解了双指针法的运用，不同题目的双指针法如何使用很灵活，还需要反复练习来熟练掌握。