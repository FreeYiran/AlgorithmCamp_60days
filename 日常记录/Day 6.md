# 算法训练营 | Day 6 | 242. 有效的字母异位词, 349. 两个数组的交集, 202. 快乐数, 1. 两数之和
[ 中文 | [English](../daily_record/Day%206.md) \]

---

##  <div align="center">:smile: 242. 有效的字母异位词</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/valid-anagram/">力扣题目</a>
  | <a href="https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1YG411p7BA">视频解析</a> |
</p>

**:clock10: 时长：30 分钟**

**:thinking: 最初想法**

有效的字母异位词就是这两个字符串是相同的字母组成，`t`的字母是否都在`s`里出现。我第一想法是挨个把`t`里的字母拿去`s`中查找，看是否能找到。

**:bulb: 资料解析**

哈希有三种数据结构，数组、set、map，根据本题题意，26个小写字母在ASCII码里面都是连续的，哈希值较小且范围可控，定义数组比较合适。

1. 定一个数组叫做record，用来上记录字符串s里字符出现的次数，大小为26，初始化为0，因为字符a到字符z的ASCII也是26个连续的数值，需要把字符映射到数组也就是哈希表的索引下标上，因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。
2. 再遍历 字符串s的时候，只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。 这样就将字符串s中字符出现的次数，统计出来了。
3. 那看一下如何检查字符串t中是否出现了这些字符，同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。
4. 最后检查一下，record数组如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。
5. 时间复杂度为O(n)，空间上因为定义是的一个常量大小的辅助数组，所以空间复杂度为O(1)

```python
# 时间复杂度：O(n)
# 空间复杂度：O(1)

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0] * 26
        for i in s:
            #并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[ord(i) - ord("a")] += 1
        for i in t:
            record[ord(i) - ord("a")] -= 1
        for i in range(26):
            if record[i] != 0:
                #record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return False
        return True
```
**其他解法一：** 没有使用数组作为哈希表，只是介绍defaultdict这样一种解题思路
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import defaultdict
        
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for x in s:
            s_dict[x] += 1
        
        for x in t:
            t_dict[x] += 1
        return s_dict == t_dict
```

**其他解法二：** 没有使用数组作为哈希表，只是介绍Counter这种更方便的解题思路
```python
class Solution(object):
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter
        a_count = Counter(s)
        b_count = Counter(t)
        return a_count == b_count
```

**:point_down: 难点** 

如何利用数组实现哈希法解题

**:pencil: 问题总结**

- `abc` 和 `abc` 也是有效的字母异位词
- 难以想到如何把哈希和数组结合起来，因为哈希法已经忘了

##  <div align="center">:smile: 349. 两个数组的交集</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/intersection-of-two-arrays/">力扣题目</a>
  | <a href="https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1ba411S7wu">视频解析</a> |
</p>

**:clock10: 时长：30 分钟**

**:thinking: 最初想法**

只有暴力破解的想法

**:bulb: 资料解析**

上一道题目用的是数组做哈希表，但是本题没有限制数值的大小，就无法使用数组来做哈希表了。后面力扣改了题目描述和后台测试数据，增添了数值范围，就可以使用数组来做哈希表了，因为数组都是1000以内的。

**方法一：HashSet & HashDict**
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
    # 使用哈希表存储一个数组中的所有元素
        table = {}
        for num in nums1:
            table[num] = table.get(num, 0) + 1
        
        # 使用集合存储结果
        res = set()
        for num in nums2:
            if num in table:
                res.add(num)
                del table[num]
        
        return list(res)
```

**方法二：HashArray**

如果该数出现，字典的值 >= 1，如果未出现，字典的值 = 0， 寻找交集只需要将对应数字出现次数相乘，如果 > 0，说明两个数组都出现了同一个数字，将其添加到结果数组中。思想有点类似于one-hot编码。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        count1 = [0]*1001
        count2 = [0]*1001
        result = []
        for i in range(len(nums1)):
            count1[nums1[i]]+=1
        for j in range(len(nums2)):
            count2[nums2[j]]+=1
        for k in range(1001):
            if count1[k]*count2[k]>0:
                result.append(k)
        return result
```

**方法三：HashSet**

这段代码使用了集合的交集操作符 & 来找到两个数组的交集。

set(nums1)：将 nums1 转化为一个集合，这会去除重复的元素。set(nums1) & set(nums2)：使用交集操作符 & 得到两个集合的交集，也就是两个数组的交集。注意，集合的交集操作会自动去除重复的元素。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

**:point_down: 难点** 

灵活使用数组、元组、字典实现哈希法解题

**:pencil: 问题总结**

这道题可以使用不同的数据结构进行解题。方法一中 del table[num] 的原因是 在循环的时候，不会再次去匹配到这个数。如果我们没有使用 del table[num]，那么在遍历 nums2 的过程中，重复出现的元素会被多次添加到 res 中，导致重复计数。

##  <div align="center">:smile: 202. 快乐数</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/happy-number/">力扣题目</a>
  | <a href="https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html">文章解析</a> |
</p>

**:clock10: 时长：0 小时**

**:thinking: 最初想法**

不知道环形链表怎么处理，细节多，有点复杂，暂时跳过

**:bulb: 资料解析**

此题分两步走：
1. 判断链表是否有环
2. 如果有环，如何找到这个环的入口

**:point_down: 难点** 

**:pencil: 问题总结**

##  <div align="center">:smile: 1. 两数之和</div>

<p align="center">
  | <a href="https://leetcode.cn/problems/linked-list-cycle-ii/">力扣题目</a>
  | <a href="https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html">文章解析</a>
  | <a href="https://www.bilibili.com/video/BV1if4y1d7ob">视频解析</a> |
</p>

**:clock10: 时长：0 小时**

**:thinking: 最初想法**

不知道环形链表怎么处理，细节多，有点复杂，暂时跳过

**:bulb: 资料解析**

此题分两步走：
1. 判断链表是否有环
2. 如果有环，如何找到这个环的入口

**:point_down: 难点** 

**:pencil: 问题总结**


---

## :trophy: 收获
**:white_check_mark: 学习时长**
- 解决题目+写笔记：**2 小时 30 分钟**

**:white_check_mark: 心得**

链表相对于昨天的题