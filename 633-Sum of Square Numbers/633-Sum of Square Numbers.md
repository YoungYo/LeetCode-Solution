这是 LeetCode 刷题系列的第 3 题。

题号：633

题名：Sum of Square Numbers

# 问题描述

Given a non-negative integer「c」, your task is to decide whether there're two integers「a」and「b」such that $a^2+b^2=c$.

# Example

**Example 1:**

```
Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
```

**Example 2:**

```
Input: 3
Output: False
```

# 解题思路

这道题的算法用的是双指针。首先求出 $ \lfloor \sqrt c \rfloor​$，令其等于 right，然后令 left = 0，令 $sum=left^2+right^2​$，剩下的步骤用伪代码描述如下：

```
while left <= right:
	若 sum = c，返回 true
	若 sum < c，left = left + 1
	若 sum > c，right = right + 1
	sum = left * left + right * right
返回 false
```

Java 代码如下：

```java
public boolean judgeSquareSum(int c) {
    int left = 0;
    int right = (int) Math.sqrt(c);
    while(left <= right){
        int sum = left * left + right * right;
        if(sum < c)
            left++;
        else if(sum > c)
            right--;
        else {
            return true;
        }
    }
    return false;
}
```

算法复杂度为：$O(\sqrt{c})$