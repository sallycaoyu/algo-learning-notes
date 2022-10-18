# Description
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

# Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

# Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
 

# Constraints:

1 <= nums.length <= 3 * 104
-100 <= nums[i] <= 100
nums is sorted in non-decreasing order.


# Solution
## 1. Time O(n), Space O(1)
    class Solution:
        def removeDuplicates(self, nums: List[int]) -> int:
            idx = 1
            while idx != len(nums):
                if nums[idx] == nums[idx-1]:
                    nums.pop(idx)
                else:
                    idx += 1
            
            return len(nums)

    For each element, see if it is the same as its previous, if so then pop it from the list,
    so all future elements can move one step to the left;
    else we can have idx going forward by 1 step.

## 2. Time O(n), Space O(n)
    class Solution:
        def removeDuplicates(self, nums: List[int]) -> int:
            num_set = set()
            idx = 0
            while idx != len(nums):
                if nums[idx] not in num_set: # O(1) for set lookup
                    num_set.add(nums[idx])
                    idx += 1
                else:
                    nums.pop(idx)
            return len(nums)

    Add each appeared num to a set, for each element in nums, look it up in set (O(1) time). 
    If appeared before, pop it from nums; if not appeared, add it to set, and increment index 
    by 1.

