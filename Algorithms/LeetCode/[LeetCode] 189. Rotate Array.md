## Problem

Given an array, rotate the array to the right by *k* steps, where *k* is non-negative.

## Example

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

## Code

```java
class Solution {
    public void rotate(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return;
        }
        int[] a = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            a[(i + k) % nums.length] = nums[i];
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = a[i];
        }
    }
}
```

주어진 배열을 `k`만큼 오른쪽으로 회전시키는 문제입니다.

단 주의할 점은, `0 <= k <= 10^5`라는 범위고, 배열보다 클 수 있습니다.

그래서 `(i+k) % nums.length`만큼 이동하도록 했습니다.

------

## Link

https://leetcode.com/problems/rotate-array/
