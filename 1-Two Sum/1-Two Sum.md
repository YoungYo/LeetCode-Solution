刷题第一天，本来是想着先拿个简单的练练手，但是没想到自己的水平如此之菜，连这个简单的题都只能想到暴力求解法，只好看了一下答案。所以下面讲的解题思路是用我自己的话把网站上给出的解题思路复述了一遍。

问题描述我没有翻译，倒不是我偷懒，而是想着让大家顺便练练英语阅读能力，毕竟对于程序员而言，英语好了也是一个优势。

# 问题描述

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

# Example

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

# 解题思路

最简单暴力的方式是，从左向右遍历数组中的元素，每遍历到一个元素 $x$，就在整个数组中查找是否存在 $target - x$。但是这种方式显然不是我们想要的。我们需要一种更高效的方式去检查数组中是否存在 $target - x$，也就是 $x$ 的补数，如果补数存在，我们应该立即查找到它的索引。那么，需要维护数组中的每一个元素与它的索引的映射关系，最好的方式就是使用 hash table。因为 hash table 是几乎可以做到在常数时间内查找，所以 hash table 能够将查找时间从 $O(n)$ 降到 $O(1)$，之所以说是「几乎」，因为可能会出现冲突，这时查找时间可能会退化到 $O(n)$，但是，只要哈希函数选用的合理，hsah table 的平均查找时间是可以达到 $O(1)$ 的。

利用 hash table，我们可以很容易地想到，通过两个迭代，第个一个迭代，把每一个元素的值以及它的索引组成一个键值对，放到 hash table 里面，然后，第二个迭代，检查每一个元素的补数（即 $target-nums[i]$）是否存在于 hash table 之中。下面是这种思路的实现代码：

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

但是其实还能继续优化，当我们向 hash table 中插入键值对的时候，可以检查当前元素的补数是否已经存在于 hash table 中，如果存在，就说明我们已经找到了解，并且可以立即返回这个解。光说可能有点抽象，看代码就明白了：

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```



这里附上完整 Java 代码：

```java
import java.util.HashMap;
import java.util.Map;

public class TwoSum {
	public static int[] twoSum(int[] nums, int target) {
		Map<Integer, Integer> map = new HashMap<>();
		for(int i = 0; i < nums.length; i++) {
			int complement = target - nums[i];
			if(map.containsKey(complement)) {
				return new int[] {map.get(complement), i};
			}
			map.put(nums[i], i);
		}
		throw new IllegalArgumentException("No Two Sum Solution");
	}
	
	public static void main(String[] args) {
		int[] nums = {2, 7, 11, 15};
		int target = 17;
		int[] solu = twoSum(nums, target);
		System.out.println(solu[0]);
		System.out.println(solu[1]);
	}
	
}
```



# 刷题感受

1. 尽管之前已经学过 hash table，但是在做这个题的时候并没有想起来用它，说明自己的编程知识还没有形成一定的体系。
2. 在这个题中，hash table 的高效可见一斑，通过哈希函数，实现从 key 到 value 的映射，查找时间可低至 $O(1)$，是一种以空间换时间的策略。
3. 遇到问题先不要上来就用暴力求解，多思考，有没有进一步优化的可能。这种优化的思维需要长久的练习来提高。