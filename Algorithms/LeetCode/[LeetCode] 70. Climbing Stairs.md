## Problem

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

## Example

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step
```

## Code

```
class Solution {
        public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int[] stairs = new int[n];
        stairs[0] = 1;
        stairs[1] = 2;
        for (int i = 2; i < n; i++) {
            stairs[i] = stairs[i - 1] + stairs[i - 2];
        }
        return stairs[n - 1];
    }
}
```

f(1) = 1, f(2) = 2 등 이런식으로 수식을 추론하면

f(n) = f(n-2) + f(n-1)이라는 수식이 추론된다.

---

## Link

[https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)