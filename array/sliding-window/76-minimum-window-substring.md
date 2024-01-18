# 76. Minimum Window Substring
Given two strings s and t of lengths m and n respectively, return the minimum window
substring
of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

Example 2:

Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.

Example 3:

Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

Constraints:
* m == s.length
* n == t.length
* 1 <= m, n <= 105
* s and t consist of uppercase and lowercase English letters.

 
Algorithm: Sliding Window. Keep two pointers: start and end.
1. Map out the frequency of each character in the target string
2. Increment end until you have a valid window.
3. Increment start to find the smallest valid window.

The tricky part is tracking whether your current window is valid. One way to accomplish this is to use that map of the target string from step 1 and have a `count` variable. The `count` variable represents how many characters in the target string are not in your current window. Once `count` is `0`, your current window is valid.

Solution
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        # Map out t
        m = collections.defaultdict(int)
        for ch in t:
            m[ch] = m[ch] + 1
    
        start, end, minStart, minLen = 0, 0, 0, float('inf')
        count = len(t)

        while end < len(s):
            # Find and account for a valid window
            endCh = s[end]
            if endCh in m:
                if m[endCh] > 0:
                    count -= 1
                m[endCh] -= 1
            end += 1

            # Find min window while we have a valid window
            while count == 0:
                # Track the min window
                if end - start < minLen:
                    minLen = end - start
                    minStart = start

                # Make the window smaller
                startCh = s[start]
                if startCh in m:
                    if m[startCh] >= 0:
                        count += 1
                    m[startCh] += 1
                start += 1

        minString = ""
        if minLen < float('inf'):
            minString = s[minStart:minStart+minLen]
        return minString
```