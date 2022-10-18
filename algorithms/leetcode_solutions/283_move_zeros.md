# Description
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

# Example 1:
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]

# Example 2:
Input: nums = [0]
Output: [0]
 

# Constraints:
1 <= nums.length <= 104
-231 <= nums[i] <= 231 - 1

# Solution 1: Two-Pointer, Time O(n), Space O(1) 
    class Solution:
        def moveZeroes(self, nums: List[int]) -> None:
            """
            Do not return anything, modify nums in-place instead.
            """
            l = 0
            r = 0
            
            for r in range(len(nums)):
                if nums[r] != 0:
                    nums[l], nums[r] = nums[r], nums[l]
                    l += 1
    
    Since we want non-zeros to be at left, zeros to be at right,
    as long as we move to the right and see a non-zero value, we swap it
    to the left.
    
# Solution 2: Time O(n), Space O(1)
    class Solution:
        def moveZeroes(self, nums: List[int]) -> None:
            """
            Do not return anything, modify nums in-place instead.
            """
            last_nonzero = 0
            for i in range(len(nums)):
                if nums[i] != 0:
                    nums[last_nonzero] = nums[i]
                    last_nonzero += 1
            
            for j in range(last_nonzero, len(nums)):
                nums[j] = 0
    
    Instead of swap in solution 1, we move non-zero elements forward,
    and fill zeros