# Prompt
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

## Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

## Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]

## Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
 

## Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.

# Solution

    class Solution:
        def twoSum(self, nums: List[int], target: int) -> List[int]:
            hashmap = {}
            for i in range(len(nums)):
                b = target - nums[i]
                if b in hashmap:
                    return [hashmap[b], i]
                hashmap[nums[i]] = i


# Tasks for One-pass Hash Table
1. Add all nums to a hashmap-like data structure (dict in Python) --> So we can search each element in O(1) time.
2. For each index i, see if any element nums[i] before it = target - nums[i]. If that's true, then return.

# Complexity Analysis
Time: O(n)
Space: O(n)