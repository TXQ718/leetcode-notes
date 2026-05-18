# LeetCode 1. 两数之和

## 题目信息

- 题号：1
- 题目：两数之和
- 难度：简单
- 类型：数组、哈希表
- 链接：https://leetcode.cn/problems/two-sum/

---

## 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，要求在数组中找到两个数，使它们的和等于 `target`，并返回这两个数的下标。

题目要求不能重复使用同一个元素。

---

## 示例

```text
输入：nums = [2, 7, 11, 15], target = 9
输出：[0, 1]
解释：nums[0] + nums[1] = 2 + 7 = 9
```

---

## 解题思路

这道题的核心思路是：

1. 使用哈希表记录已经遍历过的数字和它的下标。
2. 遍历数组时，假设当前数字是 `num`，那么需要寻找的另一个数字就是 `target - num`。
3. 如果 `target - num` 已经在哈希表中，说明找到了答案；否则就把当前数字存入哈希表。

可以从下面几个角度思考：

- 这道题考察数组和哈希表。
- 使用哈希表可以快速判断某个数字之前是否出现过。
- 不需要双重循环，否则时间复杂度会变成 `O(n²)`。
- 需要注意不能重复使用同一个元素，所以应该先判断，再存当前数字。

---

## 初始想法

我一开始想到的方法是：

```text
暴力枚举数组中的两个数，判断它们的和是否等于 target。
```

这种方法可以通过，但是效率不高。

暴力写法需要两层循环，时间复杂度是 `O(n²)`。

优化方法是使用哈希表，将“查找另一个数”的过程从线性查找优化为哈希查找。

---

## Python 代码

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashtable = {}

        for i, num in enumerate(nums):
            another = target - num

            if another in hashtable:
                return [hashtable[another], i]

            hashtable[num] = i

        return []
```

---

## 代码解释

### 1. 关键代码一

```python
hashtable = {}
```

解释这段代码的作用。

这行代码创建了一个空字典。

Python 中的字典 `dict` 类似 C++ 中的 `unordered_map`。

在本题中，字典中存储的是：

```text
数字: 下标
```

例如：

```python
{2: 0}
```

表示数字 `2` 的下标是 `0`。

---

### 2. 关键代码二

```python
for i, num in enumerate(nums):
```

解释这段代码的作用。

`enumerate(nums)` 可以同时获得数组下标和数组元素。

其中：

```text
i 表示当前元素下标
num 表示当前元素值
```

例如：

```python
nums = [2, 7, 11, 15]
```

遍历过程是：

```text
i = 0, num = 2
i = 1, num = 7
i = 2, num = 11
i = 3, num = 15
```

类似 C++ 中的：

```cpp
for (int i = 0; i < nums.size(); i++) {
    int num = nums[i];
}
```

---

### 3. 关键代码三

```python
if another in hashtable:
    return [hashtable[another], i]
```

解释这段代码的作用。

`another` 表示当前数字需要搭配的另一个数字：

```python
another = target - num
```

如果 `another` 已经在哈希表中，说明之前已经出现过这个数字。

此时：

```python
hashtable[another]
```

表示另一个数字的下标。

`i` 表示当前数字的下标。

所以返回：

```python
[hashtable[another], i]
```

---

## Python 语法总结

这道题中用到的 Python 语法有：

### 1. 字典 dict

```python
hashtable = {}
hashtable[num] = i
```

解释：

`{}` 表示创建一个空字典。

`hashtable[num] = i` 表示把 `num` 作为 key，把 `i` 作为 value。

类似 C++ 中的：

```cpp
unordered_map<int, int> hashtable;
hashtable[num] = i;
```

---

### 2. enumerate

```python
for i, num in enumerate(nums):
```

解释：

`enumerate(nums)` 可以同时获得下标和值。

这比单独写 `for i in range(len(nums))` 更简洁。

---

### 3. in

```python
if another in hashtable:
```

解释：

判断 `another` 是否是字典中的 key。

注意，`in dict` 默认判断的是 key，不是 value。

---

## 易错点

1. 不能先把当前数字存入哈希表，否则可能会重复使用同一个元素。
2. `in dict` 判断的是字典的 key，不是 value。
3. 返回的是两个数的下标，不是两个数本身。

---

## 边界情况

需要注意的特殊情况：

1. 输入为空：LeetCode 原题中一般不会给空数组作为有效答案，但代码最后保留 `return []` 作为兜底。
2. 只有一个元素：无法组成两个数之和，返回空列表。
3. 有重复元素：可以处理，例如 `nums = [3, 3], target = 6`。
4. 结果不存在：代码会执行到最后并返回 `[]`。
5. 其他特殊情况：数组中可能有负数，哈希表方法仍然适用。

---

## 复杂度分析

时间复杂度：`O(n)`

原因：

只需要遍历数组一次，字典查询平均时间复杂度是 `O(1)`。

空间复杂度：`O(n)`

原因：

最坏情况下，可能需要把数组中的很多元素都存入字典。

---

## 今日总结

这道题让我掌握了：

1. Python 字典 `dict` 的基本用法。
2. `enumerate(nums)` 同时获取下标和值。
3. 用哈希表优化查找问题的思路。

还需要复习的地方：

1. `in dict` 判断的是 key。
2. 为什么要先判断，再存当前数字。
