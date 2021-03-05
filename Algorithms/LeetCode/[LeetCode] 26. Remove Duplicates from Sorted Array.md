## Problem

Given a sorted array *nums*, remove the duplicates **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** such that each element appears only *once* and returns the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

## Example

```
Input: nums = [1,1,2]
Output: 2, nums = [1,2]
Explanation: Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the returned length.
```

## Code

```java
public int removeDuplicates(int[] nums) {
        int index = 1;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] != nums[i+1]) {
                nums[index++] = nums[i+1];
            }
        }
        return index;
}
```

주어진 배열만을 이용해서 중복된 원소를 제거해야 합니다.

변수 `index`의 값을 1로 두고, 반복하면서 중복이 아닌 변수를 `index`위치에 값을 두어 중복을 제거합니다.

이후 리턴한 값 까지의 배열이 정답으로 인정됩니다.

------

## Link

https://leetcode.com/problems/remove-duplicates-from-sorted-array/