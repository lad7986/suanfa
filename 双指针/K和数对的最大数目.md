**1679.K和数对的最大数目**

给你一个整数数组 `nums` 和一个整数 `k` 。

每一步操作中，你需要从数组中选出和为 `k` 的两个整数，并将它们移出数组。

返回你可以对数组执行的最大操作数。

**示例 1：**

```
输入：nums = [1,2,3,4], k = 5
输出：2
解释：开始时 nums = [1,2,3,4]：
- 移出 1 和 4 ，之后 nums = [2,3]
- 移出 2 和 3 ，之后 nums = []
不再有和为 5 的数对，因此最多执行 2 次操作。
```

**示例 2：**

```
输入：nums = [3,1,3,4,3], k = 6
输出：1
解释：开始时 nums = [3,1,3,4,3]：
- 移出前两个 3 ，之后nums = [1,4,3]
不再有和为 6 的数对，因此最多执行 1 次操作。
```

用哈希表更好做，哈希表存每个数字出现的次数。对于一个哈希表里面的值num，只需要检查哈希表里有没有值为k-num的就行了，

```c#
using System;
using System.Collections.Generic;

public class Solution {
    public int MaxOperations(int[] nums, int k) {
        // 哈希表，用来记录每个数出现的次数
        var freq = new Dictionary<int, int>();
        int operations = 0;

        // 统计每个数字的出现次数
        foreach (var num in nums) {
            if (freq.ContainsKey(num)) {
                freq[num]++;
            } else {
                freq[num] = 1;
            }
        }

        // 遍历哈希表中的每个数
        foreach (var key in freq.Keys) {
            if (freq.ContainsKey(key)) {
                int complement = k - key;  // 找到与当前数配对的另一个数

                if (key == complement) {
                    // 如果 num == k - num，配对需要成对出现
                    operations += freq[key] / 2;
                    freq[key] = 0;  // 所有的配对已经处理完
                } else if (freq.ContainsKey(complement)) {
                    // 如果 k - num 也在哈希表中
                    int pairs = Math.Min(freq[key], freq[complement]);
                    operations += pairs;
                    freq[key] -= pairs;  // 更新频率
                    freq[complement] -= pairs;  // 更新频率
                }
            }
        }

        return operations;
    }
}

```

