# 算法训练营 | Day 1 | 704. 二分查找, 27. 移除元素.
[ 中文 | [English](../daily_record/Day%201.md) \]

---

##  <div align="center">:smile: 704. 二分查找</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/binary-search/">力扣题目</a>
  | <a href="https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1fA4y1o715/?vd_source=ff3d934eb3ba9f6fad52d275a7ef2465">视频解析</a> |
</p>

**:clock10: 时长：2 小时**

**:thinking: 最初想法**

已经完全忘了二分查找方法,上一次学习数据结构与算法还是三年前考研的时候。

**:bulb: 资料解析**

看完`思路`，原来自己忽视了一个细节，二分查找必须得是有序数组。同时，数组中无重复元素，否则返回的下标可能不唯一。二分法，顾名思义就是找出中间位置的下标`middle`，判断该位置值是否为`target`。设定左右指针为`left`，`right`。
- `nums[middle] > target`，右侧指针`right`移到中间
- `nums[middle] < target`，左侧指针`left`移到中间。
- `nums[middle] == target`，返回中间位置下标
  
根据区间定义不同，有两种区间的写法，左闭右闭即[left, right]，或者左闭右开即[left, right)。不同区间的循环条件写法也不同，需要注意区分。

**方法一：** 定义`target`在左闭右闭区间里，即[left, right]
> - while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
> - if (nums[middle] > target) right 要赋值为 `middle - 1`，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 `middle - 1`

```python
# 时间复杂度：O(log n)，其中 n 是数组的长度
# 空间复杂度：O(1)

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # 定义target在左闭右闭的区间里，[left, right]

        while left <= right:
            middle = left + (right - left) // 2  # 防止溢出 等同于(left + right)/2
            
            if nums[middle] > target:
                right = middle - 1  # target在左区间，所以[left, middle - 1]
            elif nums[middle] < target:
                left = middle + 1  # target在右区间，所以[middle + 1, right]
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值
```

**方法二：** 定义`target`在左闭右开区间里，即[left, right)
> - while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
> - if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]
```python
# 时间复杂度：O(log n)，其中 n 是数组的长度
# 空间复杂度：O(1)

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # 定义target在左闭右开的区间里，即：[left, right)

        while left < right:  # 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            middle = left + (right - left) // 2  防止溢出 等同于(left + right)//2

            if nums[middle] > target:
                right = middle  # target 在左区间，在[left, middle)中
            elif nums[middle] < target:
                left = middle + 1  # target 在右区间，在[middle + 1, right)中
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值
```

**:point_down: 难点** 

  二分查找的边界条件定义不同，就有两种不同的二分写法 

**:pencil: 问题总结**
1. 二分查找的前提
   - 有序数组
   - 数组中无重复元素
2. 区间定义不同，两种二分法的处理有什么不一样？（见上文引用内容）
3. 为什么不是`middle = (left + right) // 2`，而是`middle = left + ((right - left) // 2)`？  
    答：防止整数溢出。整数溢出是指使用固定位数的二进制表示来存储整数值时，当数值超出表示范围而无法被准确表示时发生的情况。整数通常用有限数量的二进制位表示，不同位数的表示范围是有限的。当进行加法、减法、乘法或其他数学运算时，如果结果超过了表示范围，就会导致整数溢出。<br>
    例如，考虑一个使用8位二进制表示的有符号整数，范围从 -128 到 127。如果你试图对 127 进行加一操作，结果是 128，然而在8位表示中，最大值只能是 127，因此会发生溢出。在二进制中，128 表示为 10000000，但在8位表示中，最高位的1表示的是负数。所以，计算机可能会把这个结果解释为 -128。
4. 细节：`middle = (left + right) // 2`执行的是整数除法。

**:muscle:相关题目**
- [35.搜索插入位置](https://leetcode.cn/problems/search-insert-position/)
- [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [69.x 的平方根](https://leetcode.cn/problems/sqrtx/)
- [367.有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

##  <div align="center">:smile: 27. 移除元素</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/remove-element/">力扣题目</a>
  | <a href="https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV12A4y1Z7LP/">视频解析</a> |
</p>

**:clock10: 时长：3 小时**

**:thinking: 最初想法**

没太读懂题目中所说的“你不需要考虑数组中超出新长度后面的元素”，完全不知道解法。

**:bulb: 资料解析**

数组的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖。

**方法一：暴力法** 
- 遍历数组元素
- 当数值是val元素时，依次移动后一个元素覆盖前一个元素，数组长度减1。
```python
# 时间复杂度：O(n^2)
# 空间复杂度：O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, l = 0, len(nums)
        while i < l:
            if nums[i] == val: # 找到等于目标值的节点
                for j in range(i+1, l): # 移除该元素，并将后面元素向前平移
                    nums[j - 1] = nums[j]
                l -= 1
                i -= 1
            i += 1
        return l
```
**方法二：双指针法（快慢指针）**
- 快指针：遍历数组，寻找新数组的元素（不等于val的数）。如果不相等，进行拷贝覆盖，慢指针自增1，每次循环快指针自增1
- 慢指针：如果数组元素等于val，跳过该元素不进行拷贝覆盖，最后慢指针就是新数组的长度
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        # 快慢指针
        fast = 0  # 快指针
        slow = 0  # 慢指针
        size = len(nums)
        while fast < size:  # 不加等于是因为，a = size 时，nums[a] 会越界
            # slow 用来收集不等于 val 的值，如果 fast 对应值不等于 val，则把它与 slow 替换
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```
更简洁的一种写法：
- 遍历数组`nums`，每次取出的数字变量为`num`,设置下标`ans`
- 如果遍历元素不等于val，进行拷贝覆盖`nums[ans] = num`，`ans`自增1
- 如果遍历元素等于val，跳过该元素不进行拷贝覆盖，最后`ans`就是新数组的长度
- 此方法在移除元素较多时更适合使用
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        len = 0
        for num in nums:
            if num != val:
                nums[ans] = num
                ans += 1
        return ans
```

**方法三：双指针法(头尾指针)**
- 头指针：遍历数组，如果数组元素不等于val，头指针自增1，尾指针不变
- 尾指针：如果数组元素等于val，将`nums[ans - 1]`覆盖到`nums[i]`，尾指针自减1，头指针不变。
- 此方法适合在移除元素较少时更适合使用，会改变数组元素的顺序。
```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0  # 头指针
        ans = len(nums)  # 尾指针
        while i < ans:
            if nums[i] == val:
                nums[i] = nums[ans - 1]
                ans -= 1
            else:
                i += 1
        return ans
```
**:point_down: 难点** 

思考除暴力破解外，更有效率的解法，双指针的移动及其条件判断后的操作

**:pencil: 问题总结**
- 不改变元素顺序，可以使用**暴力解法**和**双指针法（快慢指针）**
- 改变元素顺序，可以使用**双指针法（头尾指针）**
- 这道题可以自己多画图去感受不同方法的优劣。

**:muscle:相关题目**
- [26.删除排序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
- [283.移动零](https://leetcode.cn/problems/move-zeroes/)
- [844.比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)
- [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

---

## :trophy: 收获
**:white_check_mark: 学习时长**
- 创建Github仓库+写笔记模板：**2 小时**
- 解决题目+写笔记：**5 小时**

**:white_check_mark: 心得**

对于从未刷过题，且完全忘了数据结构与算法的人来说，今天的工作量可谓是顶满了。希望能够熟悉md的语法和格式，之后写笔记能够快点。