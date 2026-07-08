# 242. Valid Anagram

**LeetCode:** https://leetcode.com/problems/valid-anagram/

**Difficulty:** Easy

---

# Pattern(s)

- Hashing
- Frequency Counting
- Array

---

# Pattern Recognition

Look for this approach when:

- You need to determine whether two strings contain the same characters.
- Character frequencies matter.
- The order of characters does not matter.
- The strings contain lowercase English letters.

---

# Intuition

Two strings are anagrams if they contain the **same characters with the same frequency**.

The brute force approach sorts both strings and compares them.

A better approach counts the frequency of every character using a HashMap.

Since the problem contains only lowercase English letters, we can further optimize by replacing the HashMap with a fixed-size frequency array of size `26`.

---

# Brute Force

## Idea

Sort both strings.

If the sorted strings are identical, then both strings contain exactly the same characters.

## Algorithm

1. Check if both strings have the same length.
2. Convert both strings into character arrays.
3. Sort both arrays.
4. Compare the sorted arrays.
5. Return the result.

### Java Code

```java
import java.util.Arrays;

class Solution {
    public boolean isAnagram(String s, String t) {

        if(s.length() != t.length()) {
            return false;
        }

        char[] sArray = s.toCharArray();
        char[] tArray = t.toCharArray();

        Arrays.sort(sArray);
        Arrays.sort(tArray);

        return Arrays.equals(sArray, tArray);
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n log n)**

- Sorting each string takes **O(n log n)**.
- Comparing both arrays takes **O(n)**.

Overall,

```
O(n log n)
```

**Space Complexity:** **O(n)**

- Character arrays are created for both strings.

---

# Better Approach

## Idea

Instead of sorting, count the frequency of every character using a HashMap.

Increase the frequency while traversing the first string.

Decrease the frequency while traversing the second string.

If every frequency becomes zero, the strings are anagrams.

## Algorithm

1. Check if both strings have equal length.
2. Create a HashMap.
3. Count frequencies of characters in the first string.
4. Decrease frequencies using the second string.
5. If any frequency is not zero, return `false`.
6. Otherwise, return `true`.

### Java Code

```java
import java.util.HashMap;

class Solution {
    public boolean isAnagram(String s, String t) {

        if(s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Integer> map = new HashMap<>();

        for(char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        for(char ch : t.toCharArray()) {

            if(!map.containsKey(ch)) {
                return false;
            }

            map.put(ch, map.get(ch) - 1);

            if(map.get(ch) == 0) {
                map.remove(ch);
            }

        }

        return map.isEmpty();
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- One traversal for the first string.
- One traversal for the second string.
- HashMap operations take **O(1)** on average.

Overall,

```
O(n)
```

**Space Complexity:** **O(k)**

- `k` is the number of distinct characters.
- In the worst case, `k = n`.

---

# Optimal Approach

## Idea

The problem states that the strings contain only lowercase English letters.

Instead of using a HashMap, use a frequency array of size `26`.

Each index represents one character.

- Increment the count for characters in the first string.
- Decrement the count for characters in the second string.
- If every frequency is zero, the strings are anagrams.

Using an array is faster than a HashMap because array access takes constant time without hashing overhead.

## Algorithm

1. Check if both strings have equal length.
2. Create an integer array of size `26`.
3. Traverse both strings together.
4. Increment the frequency for characters in the first string.
5. Decrement the frequency for characters in the second string.
6. Traverse the frequency array.
7. If any value is not zero, return `false`.
8. Otherwise, return `true`.

### Java Code

```java
class Solution {
    public boolean isAnagram(String s, String t) {

        if(s.length() != t.length()) {
            return false;
        }

        int[] freq = new int[26];

        for(int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
            freq[t.charAt(i) - 'a']--;
        }

        for(int count : freq) {
            if(count != 0) {
                return false;
            }
        }

        return true;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- One traversal of both strings.
- One traversal of the frequency array (`26` elements).

Overall,

```
O(n + 26)

Since 26 is a constant,

O(n)
```

**Space Complexity:** **O(1)**

- The frequency array always contains exactly `26` elements.
- Since its size does not depend on the input size, the extra space is constant.

  C++
  ```cpp
  class Solution {
  public:
      bool isAnagram(string s, string t) {
          vector<int> freq(26);
          for(char ch:s)
          {
              freq[ch-'a']++;
          }
          for(char ch:t)
          {
              freq[ch-'a']--;
          }
          for(int num:freq)
          {
              if(num!=0)
              {
                  return false;
              }
          }
          return true;
      }
  };
  OR
  class Solution {
    public:
        bool isAnagram(string s, string t) {
            sort(s.begin(), s.end());
            sort(t.begin(), t.end());
            return s==t;
        }
    };
```
