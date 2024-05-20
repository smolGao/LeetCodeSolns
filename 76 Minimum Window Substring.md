# Problem
Given two strings s and t of lengths m and n respectively, return the minimum window 
substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

Constraints:  
m == s.length  
n == t.length  
1 <= m, n <= 105  
s and t consist of uppercase and lowercase English letters.

# Logic

## Complexities
Runtime:     
Space: 

# Solutions
```
class Solution {
    public String minWindow(String s, String t) {
        int sLen = s.length(), tLen = t.length();
        if (tLen > sLen) return "";

        // Frequency map for characters in t
        HashMap<Character, Integer> tFreq = new HashMap<>();
        for (char c : t.toCharArray()) {
            tFreq.put(c, tFreq.getOrDefault(c, 0) +1);
        }

        // Frequency map for characters in the current window of s
        HashMap<Character, Integer> windowFreq = new HashMap<>();
        int required = tFreq.size(); // Number of unique characters in t
        int count = 0; // Number of unique characters in the current window that match the required frequency

        int left = 0, right = 0; // Sliding window pointers
        int minLen = Integer.MAX_VALUE; // Length of the minimum window
        int start = 0; // Start index of the minimum window

        for (right = 0; right < sLen; right++) {
            char cur = s.charAt(right);

            windowFreq.put(cur, windowFreq.getOrDefault(cur, 0) + 1);

            if (tFreq.containsKey(cur) && windowFreq.get(cur).intValue() == tFreq.get(cur).intValue()) {
                count++;
            }

            // Shrink the window whenever possible
            while (count == required) {
                cur = s.charAt(left);

                if (right-left+1 < minLen) {
                    minLen = right-left+1;
                    start = left;
                }

                windowFreq.put(cur, windowFreq.get(cur) - 1);
                if (tFreq.containsKey(cur) && windowFreq.get(cur).intValue() < tFreq.get(cur).intValue()) {
                    count--;
                }
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
    }
}
```