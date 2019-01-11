这是 LeetCode 刷题系列的第 4 道题。

题号：7

题名：Reverse Integer

# 问题描述

Given a 32-bit signed integer, reverse digits of an integer.

# Example

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Example 4:**

```
Input: 153423646
Output: 0
```

**Note:**

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

> 这里稍微解释下，这道题是给你一个整数 x，x 的取值范围是 $[-2^{31}, 2^{31}-1]$，让你求出倒置后的 x。需要注意的是，倒置后的 x 可能会溢出，若倒置后的 x 溢出了，程序应该返回 0。

# 解题思路

这个问题应该分成两个部分，第一个部分就是将 x 倒置，第二部分是，在倒置的过程中要随时判断倒置后的数有没有溢出。为什么会溢出？在第四个样例中，输入的数是 153423646，在区间 $[−2^{31},  2^{31} − 1]$ 范围内，没有溢出，但是倒置后，这个数变成了 646324351，大于 $2^{31}-1$，溢出了。

首先看一下，倒置的问题怎么解决。我们用 rev 来表示倒置后的 x，倒置的过程就是不断地将 x 的最后一位数拿出来，放到 rev 的末尾的过程。

用伪代码描述就是：

```
rev = 0
while x > 0:
	pop = x % 10
	x = x / 10;
	temp = rev * 10 + pop
	rev = temp
```

接下来要解决的问题是，在倒置的过程中如何判断有没有发生溢出。不妨假设 x > 0，在上述伪代码中，

- 如果 temp 溢出了，那么必然有 $rev\geq \frac{INTMAX}{10}$
- 如果 $rev > \frac{INTMAX}{10}$，那么 temp 必然溢出
- 如果 $rev = \frac{INTMAX}{10}$，那么只有当 pop > 7 的时候，temp 才会溢出。

当 x < 0 的时候，逻辑也是类似的。

按照以上描述，可以写出 Java 代码如下：

```Java
public static int reverse(int x) {
    int rev = 0;
    while (x != 0) {
        int pop = x % 10;
        x /= 10;
        if (rev > Integer.MAX_VALUE/10 || 
            (rev == Integer.MAX_VALUE / 10 && pop > 7)) 
            return 0;
        if (rev < Integer.MIN_VALUE/10 || 
            (rev == Integer.MIN_VALUE / 10 && pop < -8)) 
            return 0;
        rev = rev * 10 + pop;
    }
    return rev;
}
```

# 复杂度分析

- 时间复杂度为 $O(log_{10}x)$，因为 x 的位数是 $log_{10}x$。
- 空间复杂度为 O(1)。