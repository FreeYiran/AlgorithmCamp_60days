# Algorithm Training Camp | Day 1 | 704. Binary Search, 27. Remove Element.
[ [中文](../日常记录/Day%201.md) | English \]

---

##  <div align="center">:smile: 704. Binary Search</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/binary-search/">Leetcode Link</a>
  | <a href="https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html">Article Analysis</a>
  | <a href="https://www.bilibili.com/video/BV1fA4y1o715/?vd_source=ff3d934eb3ba9f6fad52d275a7ef2465">Video Analysis</a> |
</p>

**:clock10: Time：2 hours**

**:thinking: Initial Thoughts**

I have completely forgotten the binary search method. The last time I studied data structures and algorithms was three years ago during the time I was preparing for the postgraduate entrance examination.

**:bulb: Material Interpretation**
After reading the `Approach` section, I realized that I had overlooked a detail: binary search requires the array to be sorted. Additionally, the array must have no duplicate elements; otherwise, the returned index might not be unique. The binary search algorithm, as the name suggests, involves identifying the index of the middle position, denoted as `middle`, and checking whether the value at that position is equal to the `target`. The algorithm sets the left and right pointers as `left` and `right`.
- If `nums[middle] > target`, move the right pointer `right` to the middle.
- If `nums[middle] < target`, move the left pointer `left` to the middle.
- If `nums[middle] == target`, return the index of the middle position.

Depending on the definition of intervals, there are two ways to represent intervals: closed on the left and right, denoted as [left, right], or closed on the left and open on the right, denoted as [left, right). The loop conditions for different intervals also vary, so it's important to distinguish between them.

**Method One:** Defining the ·`target` within a closed interval [left, right]

> - Use while (left <= right) with <= because left == right is meaningful.
> - If nums[middle] > target, assign right as middle - 1 because the current nums[middle] cannot be the target, so the end index of the left interval to be searched is middle - 1.

```python
# The time complexity is O(log n), where n is the length of the array
# The space complexity is O(1)

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # Define the target within a closed interval [left, right]

        while left <= right:
            middle = left + (right - left) // 2  # To prevent overflow, equivalent to (left + right) / 2
            
            if nums[middle] > target:
                right = middle - 1  # Target is in the left interval, so update [left, middle - 1]
            elif nums[middle] < target:
                left = middle + 1   # Target is in the right interval, so update [middle + 1, right]
            else:
                return middle  # Target value found in the array, return the index
        return -1  # Target value not found in the array
```

**Method Two:** Defining the `target` within a closed interval on the left and open on the right, denoted as [left, right).
> - Using `while (left < right)` with `<` because `left == right` has no meaning in the interval [left, right)
> - If `nums[middle] > target`, update `right` to `middle` because the current `nums[middle]` is not equal to the `target`. To continue searching in the left interval, and since the search interval is closed on the left and open on the right, updating `right` to `middle` ensures that the next search interval will not include the comparison of `nums[middle]`.
```python
# The time complexity is O(log n), where n is the length of the array
# The space complexity is O(1)

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # Define the target within a closed interval [left, right)

        while left < right:  # As left == right is invalid within [left, right), we use <
            middle = left + (right - left) // 2  # To prevent overflow, equivalent to (left + right) // 2

            if nums[middle] > target:
                right = middle  # Target is in the left interval, update to [left, middle)
            elif nums[middle] < target:
                left = middle + 1  # Target is in the right interval, update to [middle + 1, right)
            else:
                return middle  # Target value found in the array, return the index
        return -1  # Target value not found in the array

```

**:point_down: Difficulties**

Different definitions of boundary conditions in binary search lead to two distinct implementations of the algorithm.

**:pencil: Summary**
1. Preconditions for Binary Search:
   - Sorted array
   - Array with no duplicate elements
2. Different interval definitions lead to distinct treatments of the two binary search approaches. (Refer to the content quoted above.)
3. The reason it's not `middle = (left + right) // 2` but rather `middle = left + ((right - left) // 2)` is to prevent integer overflow. 
   Integer overflow occurs when using a fixed number of binary bits to represent an integer value, and the value exceeds the representable range. Integers are typically represented using a finite number of binary bits, and different bit lengths have limited ranges. When performing addition, subtraction, multiplication, or other mathematical operations, if the result goes beyond the representable range, integer overflow occurs.

   For example, consider using an 8-bit binary representation for a signed integer ranging from -128 to 127. If you try to add one to 127, the result is 128. However, in an 8-bit representation, the maximum value can only be 127, leading to overflow. In binary, 128 is represented as 10000000, but in an 8-bit representation, the highest bit '1' indicates a negative number. Therefore, the computer might interpret this result as -128.
4. Detail: `middle = (left + right) // 2` performs integer division.

**:muscle:Related Problems**
- [35.Search Insert Position](https://leetcode.cn/problems/search-insert-position/)
- [34.Find First and Last Position of Element in Sorted Array](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [69.Sqrtx](https://leetcode.cn/problems/sqrtx/)
- [367.Valid Perfect Square](https://leetcode.cn/problems/valid-perfect-square/)

##  <div align="center">:smile: 27. Remove Element</div>
<p align="center">
  | <a href="https://leetcode.cn/problems/remove-element/">Leetcode Link</a>
  | <a href="https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE">Article Analysis</a>
  | <a href="https://www.bilibili.com/video/BV12A4y1Z7LP/">Video Analysis</a> |
</p>

**:clock10: Time：3 hours**

**:thinking: Initial Thoughts**

I didn't quite understand the statement in the problem that says "You do not need to consider the elements beyond the new length in the array," and I have no idea about the solution.

**:bulb: Material Interpretation**
The elements of an array are contiguous in memory addresses; you cannot individually delete a specific element from the array. Instead, you can only overwrite elements.

**Method One:** Brute force solution
- Iterate through the array elements.
- When the value is equal to the 'val' element, sequentially move the following element to overwrite the previous one, reducing the array length by 1.
```python
# The time complexity is O(n^2)
# The space complexity is O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, l = 0, len(nums)
        while i < l:
            if nums[i] == val:  # Find elements equal to the target value
                for j in range(i + 1, l):  # Remove the element and shift the subsequent elements forward
                    nums[j - 1] = nums[j]
                l -= 1  # Reduce the array length
                i -= 1  # Adjust index after shifting
            i += 1
        return l
```
**Method 2: Two-Pointer Approach (Fast and Slow Pointers)**
- Fast Pointer: Iterate through the array to find elements for the new array (those that are not equal to 'val'). If not equal, perform copying and overwriting, and increment the slow pointer by 1. Fast pointer increments by 1 in each iteration.
- Slow Pointer: If the array element is equal to 'val', skip that element without copying. In the end, the slow pointer will represent the length of the new array.
```python
# The time complexity is O(n^2)
# The space complexity is O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        fast = 0  # Fast pointer
        slow = 0  # Slow pointer
        size = len(nums)
        while fast < size:  # Use < instead of <= to avoid out-of-bounds when fast equals size
            # The slow pointer collects values that are not equal to 'val'. If the fast pointer value is not equal to 'val', replace it with the slow pointer.
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow

```
A more concise approach:
- Iterate through the array nums, and each time, take the element as num and set the index as ans.
- If the element is not equal to val, copy and overwrite by nums[ans] = num, then increment ans by 1.
- If the element is equal to val, skip it without copying. In the end, ans represents the length of the new array.
- This method is more suitable when removing multiple elements.
```python
# The time complexity is O(n^2)
# The space complexity is O(1)
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        len = 0
        for num in nums:
            if num != val:
                nums[ans] = num
                ans += 1
        return ans
```
**Method 3: Two-Pointer Approach (Head and Tail Pointers)**
- Head Pointer: Iterate through the array. If the array element is not equal to 'val', increment the head pointer by 1, while the tail pointer remains unchanged.
- Tail Pointer: If the array element is equal to 'val', overwrite `nums[i]` with `nums[ans - 1]`, and decrement the tail pointer by 1 while keeping the head pointer unchanged.
- This method is more suitable when removing a smaller number of elements and may change the order of array elements.
```python
# The time complexity is O(n^2)
# The space complexity is O(1)
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

**:point_down: Difficulties**

Consider a more efficient solution other than brute force, which involves the use of two pointers, and how their movements and conditional checks are utilized to improve efficiency.

**:pencil: Summary**
- Without changing the order of elements, you can utilize both the **brute force** and **two-pointer approach (fast and slow pointers)**.
- By altering the order of elements, you can utilize the **two-pointer approach (head and tail pointers)**.
- It's recommended to draw diagrams to better understand the pros and cons of different methods for this problem.

**:muscle:Related Problems**
- [26.Remove Duplicates from Sorted Array](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
- [283.Move Zeroes](https://leetcode.cn/problems/move-zeroes/)
- [844.Backspace String Compare](https://leetcode.cn/problems/backspace-string-compare/)
- [977.Squares of a Sorted Array](https://leetcode.cn/problems/squares-of-a-sorted-array/)

---

## :trophy: Gains
**:white_check_mark: Learning Duration**
- Creating a GitHub repository and writing a note template:**2 hours**
- Solving problems and writing notes:**5 hours**

**:white_check_mark: Insights**

For someone who has never practiced problem-solving and has completely forgotten about data structures and algorithms, today's workload can be described as quite intensive. Hopefully, I can become familiar with Markdown syntax and formatting, so that I can write notes more efficiently in the future.