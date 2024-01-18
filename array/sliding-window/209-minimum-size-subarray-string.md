# 209. Minimum Size Subarray Sum
Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

Example 1:

Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

Example 2:

Input: target = 4, nums = [1,4,4]
Output: 1

Example 3:

Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

Constraints:

    1 <= target <= 109
    1 <= nums.length <= 105
    1 <= nums[i] <= 104

 
Follow up: If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log(n)).
 
Algorithm: Sliding Window. Keep two pointers: start and end.
1. Increment end until you have a valid window.
2. Increment start to find the smallest valid window.

To see if your current window is valid, keep a count variable to track the sum of your current window. 

Solution
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        start, end, minStart, minLength = 0, 0, 0, float('inf')
        count = 0
        
        while end < len(nums):
            count += nums[end]

            while count >= target and start <= end:
                curLength = end - start + 1
                minLength = min(minLength, curLength)
                count -= nums[start]
                start += 1
            
            end += 1

        # No valid subarray found
        if minLength == float('inf'): 
            minLength = 0

        return minLength
```

Runtime: O(n) to iterate over each element in the array. 