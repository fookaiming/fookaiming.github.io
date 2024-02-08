# Binary Search

## General Approach
1. handle coner case
2. define search space
   - [left, right] or ***[left, right)***
3. deduce stop condition from seaching space
4. mid value: *(left + right) / 2* may cause overflow, better use ***((right - left) >> 1) + left***
6. according to search space, update left/right

## Questions

### Classic binary search
```text
Given an array of integers nums which is sorted in ascending order, and an integer target,
write a function to search target in nums. If target exists, then return its index.
Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1

Constraints:

  -  1 <= nums.length <= 104
  -  -10^4 < nums[i], target < 10^4
  -  All the integers in nums are unique.
  -  nums is sorted in ascending order.

link: https://leetcode.com/problems/binary-search/
```
```java
class _704 {
    public int search(int[] nums, int target) {
        // 1. Handle corner case
        if (nums.length == 1) return nums[0] == target ? 0 : -1;
        if (target > nums[nums.length - 1]) return nums.length;

        // 2. define search space, here  [left, right)
        int left = 0;

        // 2. according to [left, right),
        // the search space do not include right pointer
        int right = nums.length;

        // 3. deduce stop condition from seaching space,
        //    according to [left, right), left == right is illegal
        while (left < right) {
            // 4. be careful about overflow
            int mid = left + ((right - left) >> 1);

            if (nums[mid] == target) {
                return mid;
            } // 5. according to [left, right), update left/right
            else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return -1;
    }
}
```

### Insertion Point
- if target is found in array, the insertion point should be the index of the first target
- left point will never go beyond right when search space is [left, right)
- put the boundary of left pointer to target -> left pointer will converage
```text
 Given a sorted array of distinct integers and a target value,
 return the index if the target is found.
 If not, return the index where it would be if it were inserted in order.

 You must write an algorithm with O(log n) runtime complexity.

 Input: nums = [1,3,5,6], target = 5
 Output: 2

 Input: nums = [1,3,5,6], target = 2
 Output: 1

 Input: nums = [1,3,5,6], target = 7
 Output: 4

 Constraints:
 - 1 <= nums.length <= 104
 - -10^4 <= nums[i] <= 10^4
 - nums contains distinct values sorted in ascending order.
 - -10^4 <= target <= 10^4

link: https://leetcode.com/problems/search-insert-position/
```
```java
public class _35 {
    public int searchInsert(int[] nums, int target) {        
        if (target < nums[0]) return 0;
        if (target > nums[nums.length - 1]) return nums.length;

        int left = 0;
        int right = nums.length;

        while(left < right) {
            int mid =  ((right - left) >> 1) + left;
            // left point will never go beyond right since search space is [left, right)
            // put right pointer to boundary -> left pointer will converage
            if (target <= nums[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }

        //  if (nums[mid] < target) {
        //     left = mid + 1;
        // } else {
        //     right = mid;
        // }
        }

        return left;
    }
}
```

### First and Last Position
- variant of Q35
- first apporach is the find the insertion point, then nevigate the pointer to find last element
  - this apporach is slow when the distance between first and last element is far
- second apporach is to search insertion point of target value then search ***insertion point of (target + 1)***

> please handle the coner case and boundary check carefully, and think about how to reduce the search space by boundary check
```text
 Given an array of integers nums sorted in non-decreasing order,
 find the starting and ending position of a given target value.
 If target is not found in the array, return [-1, -1].

 You must write an algorithm with O(log n) runtime complexity.

 Input: nums = [5,7,7,8,8,10], target = 8
 Output: [3,4]

 Input: nums = [5,7,7,8,8,10], target = 6
 Output: [-1,-1]

 Input: nums = [], target = 0
 Output: [-1,-1]

 Constraints:
 - 0 <= nums.length <= 10^5
 - -10^9 <= nums[i] <= 10^9
 - nums is a non-decreasing array.
 - -10^9 <= target <= 10^9

link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array
 ```
```java
public class _34 {
    public int[] searchRange(int[] nums, int target) {
        if (nums.length == 0) return new int[]{-1, -1};

        if (nums.length == 1) return nums[0] == target ? new int[]{0, 0} : new int[]{-1, -1};

        // reduce the search space by boundary check
        if (nums[nums.length - 1] < target) return new int[]{-1, -1};

        int left = binarySearch(nums, 0, target);
        if (nums[left] != target) return new int[]{-1, -1};
        if (left == nums.length - 1) return new int[]{left, left};

        int right = binarySearch(nums, left, target + 1);
        if (right >= nums.length) return new int[]{left, right - 1};
        if (nums[right] > target) return new int[]{left, right - 1};
        return new int[]{left, right};


    }

    private int binarySearch(int[] nums, int start, int target) {
        int left = start;
        int right = nums.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (target <= nums[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }
}
```