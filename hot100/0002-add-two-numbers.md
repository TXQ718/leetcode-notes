# LeetCode 2. 两数相加

## 题目信息

- 题号：2
- 题目：两数相加
- 难度：中等
- 类型：链表、模拟、进位
- 链接：https://leetcode.cn/problems/add-two-numbers/

---

## 题目描述

给定两个非空链表 `l1` 和 `l2`，它们分别表示两个非负整数。

数字按照逆序存储，每个结点只存储一位数字。

要求将两个数相加，并以相同的链表形式返回结果。

---

## 示例

```text
输入：
l1 = 2 -> 4 -> 3
l2 = 5 -> 6 -> 4

输出：
7 -> 0 -> 8

解释：
2 -> 4 -> 3 表示数字 342
5 -> 6 -> 4 表示数字 465
342 + 465 = 807
所以返回 7 -> 0 -> 8
```

---

## 解题思路

这道题的核心思路是：

1. 因为链表是逆序存储的，所以可以从头结点开始逐位相加。
2. 每一位相加时，需要考虑 `l1` 当前值、`l2` 当前值和上一位产生的进位 `carry`。
3. 如果某个链表已经为空，就把它当前位的值当作 `0`；如果最后还有进位，也要继续创建新结点。

可以从下面几个角度思考：

- 这道题考察链表和模拟加法。
- 需要使用虚拟头结点 `dummy` 来方便构造结果链表。
- 需要使用 `carry` 保存进位。
- 需要考虑两个链表长度不同，以及最后仍然有进位的情况。

---

## 初始想法

我一开始想到的方法是：

```text
先处理 l1 和 l2 都不为空的情况。
再单独处理 l1 剩余部分。
再单独处理 l2 剩余部分。
最后处理 carry。
```

这种方法可以通过，但是代码重复较多。

因为三段循环中都在做类似的事情：

```text
取当前位
加上 carry
创建新结点
移动指针
```

所以可以优化成一个统一循环：

```python
while l1 or l2 or carry:
```

---

## Python 代码

```python
class Solution:
    def addTwoNumbers(
        self,
        l1: Optional[ListNode],
        l2: Optional[ListNode]
    ) -> Optional[ListNode]:

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

---

## 代码解释

### 1. 关键代码一

```python
dummy = ListNode(0)
cur = dummy
```

解释这段代码的作用。

`dummy` 是虚拟头结点。

它本身不存储有效答案，只是为了方便连接新结点。

`cur` 表示当前结果链表的尾结点。

最后返回：

```python
return dummy.next
```

因为真正的答案链表是从 `dummy.next` 开始的。

---

### 2. 关键代码二

```python
while l1 or l2 or carry:
```

解释这段代码的作用。

这句表示只要满足下面任意一个条件，就继续循环：

```text
l1 还没有遍历完
l2 还没有遍历完
carry 还有进位
```

它可以统一处理：

```text
两个链表一样长
l1 更长
l2 更长
最后还有进位
```

---

### 3. 关键代码三

```python
x = l1.val if l1 else 0
y = l2.val if l2 else 0
```

解释这段代码的作用。

这是 Python 的三元表达式。

```python
x = l1.val if l1 else 0
```

意思是：

```text
如果 l1 不为空，就取 l1.val
否则，x = 0
```

类似 C++ 中的：

```cpp
int x = l1 ? l1->val : 0;
```

这样可以避免在 `l1` 为空时访问 `l1.val` 导致报错。

---

## Python 语法总结

这道题中用到的 Python 语法有：

### 1. 链表结点

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

解释：

`self.val` 表示当前结点的值。

`self.next` 表示当前结点指向的下一个结点。

---

### 2. 链表判空

```python
while l1 and l2:
```

解释：

表示只有当 `l1` 和 `l2` 都不为空时才继续循环。

完整写法是：

```python
while l1 is not None and l2 is not None:
```

本题优化后使用：

```python
while l1 or l2 or carry:
```

表示只要还有链表没有遍历完，或者还有进位，就继续。

---

### 3. 整除和取模

```python
carry = total // 10
cur.next = ListNode(total % 10)
```

解释：

`//` 表示整除，用来计算进位。

`%` 表示取余，用来计算当前位应该存储的数字。

例如：

```python
total = 15
carry = total // 10   # 1
digit = total % 10    # 5
```

---

## 易错点

1. 最后不要忘记处理进位 `carry`。
2. 访问 `l1.val` 或 `l2.val` 前，要先判断链表是否为空。
3. 返回结果时应该返回 `dummy.next`，不是 `dummy`。

---

## 边界情况

需要注意的特殊情况：

1. 输入为空：LeetCode 原题中两个链表都是非空链表。
2. 只有一个元素：例如 `l1 = 5, l2 = 5`，结果应该是 `0 -> 1`。
3. 有重复元素：链表中可以有多个相同数字，不影响计算。
4. 结果不存在：本题一定有结果，至少返回一个结点。
5. 其他特殊情况：两个链表长度不同，或者最后产生新的进位。

---

## 复杂度分析

时间复杂度：`O(max(m, n))`

原因：

设两个链表长度分别为 `m` 和 `n`，最多遍历较长链表的长度。

空间复杂度：`O(max(m, n))`

原因：

需要创建一个新的结果链表，长度最多为 `max(m, n) + 1`。

---

## 今日总结

这道题让我掌握了：

1. Python 链表的基本写法。
2. 虚拟头结点 `dummy` 的用法。
3. 进位 `carry` 的处理方式。

还需要复习的地方：

1. `while l1 or l2 or carry` 的循环条件。
2. `x = l1.val if l1 else 0` 这种三元表达式。
