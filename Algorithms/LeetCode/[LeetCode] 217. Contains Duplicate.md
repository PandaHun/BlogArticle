## Problem

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

## Example

```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## Code

```java
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        if (nums == null || nums.length == 0) {
            return false;
        }
        HashSet<Integer> hs = new HashSet<>();
        for (int number : nums) {
            if (!hs.add(number)) {
                return true;
            }
        }
        return false;
    }
}
```

처음에는 간단하게 `nums` 배열을 `LinkedList`에 넣어 확인하는 식으로 구현했으나, TimeOut이 떴습니다.

그래서 `HashSe`을 사용해 TimeOut을 해결했습니다.

------

## Link

https://leetcode.com/problems/contains-duplicate/
