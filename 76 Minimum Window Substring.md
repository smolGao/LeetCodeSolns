# Problem
**Difficulty**: Hard  

Given two strings s and t of lengths m and n respectively, return the minimum window 
substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

Constraints:  
m == s.length  
n == t.length  
1 <= m, n <= 105  
s and t consist of uppercase and lowercase English letters.

# Logic
First we want to check if String t's length is less than s's length. If tLen is not <= sLen, we know that it's impossible for s to contains t. We return "" for this case.

Otherwise, we want to check all possible subwindows of t in s to find the minimum window.

One of way to do it is by forming two maps that corresponding to unique chars in s and t. The key is character, and the value is its frequency.

Then we count the total amount of characters we need in a window that consists t as `requried`.  

Later, we will use the following varaibles to traverse String s:  
* count = numbers in s map that matches the frequency in t map.
* left = the left boundary of the current windows
* start = the start of the minimum window
* minLen = the minimum window's length

Lastly, we iterate the String s with right pointer from 0 to the end of the string.

1. Update s map
2. Increment count if the number for that particular character is satisfied
3. While all conditions are satifised, count == required, shrink the windows by moving left boundary to the right and update the minLen and start when applicable.
4. After iteration of string s, return the substring from start index to start index + minLen


## Complexities
Runtime: O(M + N)   
Space: O(U)

M = s's length  
N = t's length  
U = number of unique character

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