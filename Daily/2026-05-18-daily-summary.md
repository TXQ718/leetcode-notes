# 2026-05-18 刷题总结

## 今日完成题目

| 题号 | 题目 | 难度 | 类型 | 文件 |
|---|---|---|---|---|
| 1 | 两数之和 | 简单 | 数组 / 哈希表 | [0001-two-sum.md](../hot100/0001-two-sum.md) |
| 2 | 两数相加 | 中等 | 链表 / 模拟 / 进位 | [0002-add-two-numbers.md](../hot100/0002-add-two-numbers.md) |
| 27 | 移除元素 | 简单 | 数组 / 双指针 | [0027-remove-element.md](../hot100/0027-remove-element.md) |

---

## 今日学习重点

今天主要学习了：

1. Python 数组、列表、字典和链表的基础语法。
2. 哈希表在数组查找问题中的应用。
3. 链表题中虚拟头结点、指针移动和进位处理的写法。

涉及的数据结构 / 算法思想：

- 数组
- 链表
- 哈希表
- 双指针
- 模拟
- 进位处理

---

## 一、今日题目复盘

### 题目一：LeetCode 1. 两数之和

题目类型：

```text
数组 / 哈希表
```

核心思路：

1. 遍历数组中的每个元素 `num`。
2. 对于当前元素，需要寻找的另一个数是 `target - num`。
3. 使用哈希表记录已经遍历过的数字和下标，如果 `target - num` 已经出现过，就返回两个下标。

关键代码：

```python
hashtable = {}

for i, num in enumerate(nums):
    another = target - num

    if another in hashtable:
        return [hashtable[another], i]

    hashtable[num] = i
```

易错点：

1. 不能先把当前数字放入哈希表，否则可能会重复使用同一个元素。
2. `in dict` 判断的是字典中的 key，不是 value。

掌握情况：

```text
基本掌握
```

---

### 题目二：LeetCode 2. 两数相加

题目类型：

```text
链表 / 模拟 / 进位
```

核心思路：

1. 两个链表是逆序存储的，可以从头结点开始逐位相加。
2. 每一位相加时，需要考虑 `l1` 当前值、`l2` 当前值和上一位产生的进位 `carry`。
3. 使用虚拟头结点 `dummy` 构造结果链表，最后返回 `dummy.next`。

关键代码：

```python
dummy = ListNode(0)
cur = dummy
carry = 0

while l1 or l2 or carry:
    x = l1.val if l1 else 0
    y = l2.val if l2 else 0

    total = x + y + carry
    carry = total // 10

    cur.next = ListNode(total % 10)
    cur = cur.next

    if l1:
        l1 = l1.next
    if l2:
        l2 = l2.next

return dummy.next
```

易错点：

1. 访问 `l1.val` 或 `l2.val` 之前，需要先判断链表是否为空。
2. 最后如果还有进位 `carry`，也要继续创建新结点。
3. 返回结果时应该返回 `dummy.next`，不是 `dummy`。

掌握情况：

```text
需要复习
```

---

### 题目三：LeetCode 27. 移除元素

题目类型：

```text
数组 / 双指针
```

核心思路：

1. 使用 `k` 表示新数组长度，也表示下一个可以写入的位置。
2. 遍历数组中的每个元素 `num`。
3. 如果 `num != val`，说明当前元素应该保留，把它放到 `nums[k]`，然后 `k += 1`。

关键代码：

```python
k = 0

for num in nums:
    if num != val:
        nums[k] = num
        k += 1

return k
```

易错点：

1. `remove()` 每次只删除第一个匹配元素，不是一次删除全部。
2. `while val in nums` 虽然简单，但效率不高，最坏情况下可能接近 `O(n²)`。
3. 不建议写 `for val in nums`，因为会覆盖函数参数 `val`。

掌握情况：

```text
基本掌握
```

---

## 二、今日 Python 语法收获

### 1. 获取数组长度

```python
len(nums)
```

解释：

`len(nums)` 用来获取 Python 列表的长度。

例如：

```python
nums = [2, 7, 11, 15]
print(len(nums))
```

输出：

```text
4
```

---

### 2. for 循环和 range

```python
for i in range(n):
    print(i)
```

解释：

`range(n)` 会生成从 `0` 到 `n - 1` 的整数，不包含 `n`。

例如：

```python
for i in range(5):
    print(i)
```

输出：

```text
0
1
2
3
4
```

---

### 3. enumerate 同时获取下标和值

```python
for i, num in enumerate(nums):
    print(i, num)
```

解释：

`enumerate(nums)` 可以同时得到数组元素的下标和值。

其中：

```text
i 表示下标
num 表示当前元素
```

---

### 4. 字典 dict

```python
hashtable = {}
hashtable[num] = i
```

解释：

`{}` 表示创建一个空字典。

`hashtable[num] = i` 表示把 `num` 作为 key，把 `i` 作为 value。

在「两数之和」中，字典用来记录：

```text
数字: 下标
```

---

### 5. 判断元素是否存在

```python
if another in hashtable:
```

解释：

当右边是字典时，`in` 判断的是 key 是否存在。

例如：

```python
hashtable = {2: 0}
print(2 in hashtable)
```

结果是：

```text
True
```

---

### 6. 链表结点访问

```python
cur.val
cur.next
```

解释：

Python 中访问对象成员使用点号。

在链表中：

```python
cur.val
```

表示当前结点的值。

```python
cur.next
```

表示当前结点的下一个结点。

---

### 7. 三元表达式

```python
x = l1.val if l1 else 0
```

解释：

如果 `l1` 不为空，就取 `l1.val`。

如果 `l1` 为空，就取 `0`。

这可以避免链表为空时访问 `.val` 报错。

---

## 三、和 C++ 的对比

今天遇到的 Python 写法：

```python
len(nums)
```

对应的 C++ 写法：

```cpp
nums.size()
```

---

今天遇到的 Python 写法：

```python
for i in range(n):
```

对应的 C++ 写法：

```cpp
for (int i = 0; i < n; i++) {
}
```

---

今天遇到的 Python 写法：

```python
hashtable = {}
hashtable[num] = i
```

对应的 C++ 写法：

```cpp
unordered_map<int, int> hashtable;
hashtable[num] = i;
```

---

今天遇到的 Python 写法：

```python
if another in hashtable:
```

对应的 C++ 写法：

```cpp
if (hashtable.find(another) != hashtable.end()) {
}
```

---

今天遇到的 Python 写法：

```python
cur.val
cur.next
```

对应的 C++ 写法：

```cpp
cur->val;
cur->next;
```

区别总结：

1. Python 不需要手动声明变量类型，C++ 通常需要声明类型。
2. Python 使用缩进表示代码块，C++ 使用 `{}` 表示代码块。
3. Python 中链表结点访问用点号，C++ 指针访问成员通常用 `->`。
4. Python 的 `dict` 类似 C++ 的 `unordered_map`。
5. Python 的 `range(n)` 类似 C++ 中的 `for (int i = 0; i < n; i++)`。

---

## 四、今日易错点

1. `range(n)` 生成的是 `0` 到 `n - 1`，不包含 `n`。
2. `for val in nums` 语法上可以，但在「移除元素」中不推荐，因为会覆盖参数 `val`。
3. `remove()` 每次只删除第一个匹配元素，不是删除所有匹配元素。
4. `in dict` 判断的是字典中的 key，不是 value。
5. 链表访问 `l1.val` 之前，要先判断 `l1` 是否为空。
6. 使用虚拟头结点时，最后应该返回 `dummy.next`。
7. 「两数相加」中不要忘记处理最后的进位 `carry`。

---

## 五、今日掌握的新模板

### 模板一：哈希表查找模板

适用场景：

```text
数组中查找某个数是否出现过 / 两数之和 / 快速匹配目标值
```

代码模板：

```python
hashtable = {}

for i, num in enumerate(nums):
    need = target - num

    if need in hashtable:
        return [hashtable[need], i]

    hashtable[num] = i
```

理解：

1. 哈希表用来记录已经遍历过的数字。
2. 每次遍历当前数字时，先判断它需要的另一个数字是否已经出现过。

---

### 模板二：数组原地覆盖模板

适用场景：

```text
移除元素 / 保留满足条件的元素 / 数组原地修改
```

代码模板：

```python
k = 0

for num in nums:
    if num != val:
        nums[k] = num
        k += 1

return k
```

理解：

1. `k` 表示新数组长度，也表示下一个写入位置。
2. 满足条件的元素向前覆盖，不满足条件的元素跳过。

---

### 模板三：链表逐位相加模板

适用场景：

```text
链表加法 / 模拟进位 / 两个链表长度可能不同
```

代码模板：

```python
dummy = ListNode(0)
cur = dummy
carry = 0

while l1 or l2 or carry:
    x = l1.val if l1 else 0
    y = l2.val if l2 else 0

    total = x + y + carry
    carry = total // 10

    cur.next = ListNode(total % 10)
    cur = cur.next

    if l1:
        l1 = l1.next
    if l2:
        l2 = l2.next

return dummy.next
```

理解：

1. `dummy` 用来简化链表头结点处理。
2. `carry` 用来保存进位。
3. `while l1 or l2 or carry` 可以统一处理链表长度不同和最后进位的情况。

---

## 六、还没完全理解的地方

1. 链表题中虚拟头结点 `dummy` 的使用还需要多练习。
2. `while l1 or l2 or carry` 这种统一循环条件需要反复手写。
3. Python 的三元表达式 `x = l1.val if l1 else 0` 需要继续熟悉。

---

## 七、明日计划

明天准备完成：

1. 继续按题号练习hot100
2. 继续总结刷题日记，并上传github管理

需要复习：

1. LeetCode 2. 两数相加。
2. Python 字典、列表、链表相关语法。

---

## 八、今日总结

今天主要完成了：

1. LeetCode 1. 两数之和。
2. LeetCode 2. 两数相加。
3. LeetCode 27. 移除元素。

今天最大的收获是：

```text
理解了 Python 中数组、字典和链表的基本写法，并能用哈希表、双指针和虚拟头结点解决基础算法题。
```

今天最需要加强的是：

```text
链表题还需要多练习，尤其是指针移动、虚拟头结点和进位处理。
```
