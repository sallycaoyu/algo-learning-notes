# Prompt

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

# Example 1:

Input: s = "()"
Output: true
# Example 2:

Input: s = "()[]{}"
Output: true
# Example 3:

Input: s = "(]"
Output: false

# Constraints:

1 <= s.length <= 104
s consists of parentheses only '()[]{}'.

# Solution
## 1. Time O(n), Space O(n)
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        paren_map = {')':'(', ']':'[', '}':'{'}
        
        for i in s:
            if i in paren_map:
                if stack:
                    top = stack.pop()
                    if top != paren_map[i]:
                        return False
                else: # If there is no element in stack, we cannot pop any element from it, so we directly return False.
                    return False 
            else:
                stack.append(i)
        return not stack   

- Use a stack that only stores left parenthesis, and a dict storing the mapping of left & right parenthesis.
- Iterate through s, when seeing a right parenthesis, pop the last element from stack, see if they map.      