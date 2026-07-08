# 3. Longest Substring Without Repeating Characters

**LeetCode:** https://leetcode.com/problems/longest-substring-without-repeating-characters/

**Difficulty:** Medium

---

# Pattern(s)

- Sliding Window
- Two Pointers
- HashSet / HashMap

---

# Pattern Recognition

Look for this approach when:

- You need the longest/shortest contiguous substring.
- The window can expand and shrink.
- Duplicate characters are not allowed.
- The problem asks for a contiguous substring.

---

# Intuition

We need to find the longest substring that contains only unique characters.

The brute force approach generates every possible substring and checks whether it contains duplicate characters.

A better approach uses a sliding window along with a HashSet. Whenever a duplicate character is found, shrink the window until all characters become unique again.

The optimal approach replaces the repeated removals with a HashMap that directly stores the last occurrence of every character. Instead of moving the left pointer one step at a time, we jump it directly to the correct position.

---

# Brute Force

## Idea

Generate every possible substring.

For each substring, check whether all characters are unique.

If yes, update the maximum length.

## Algorithm

1. Select every index as the starting point.
2. Select every possible ending point.
3. Check whether the substring contains duplicate characters.
4. Update the maximum length.
5. Return the answer.

### Java Code

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {

        int ans = 0;

        for(int i = 0; i < s.length(); i++) {

            HashSet<Character> set = new HashSet<>();

            for(int j = i; j < s.length(); j++) {

                if(set.contains(s.charAt(j))) {
                    break;
                }

                set.add(s.charAt(j));
                ans = Math.max(ans, j - i + 1);
            }
        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- Outer loop selects every starting index.
- Inner loop extends the substring until a duplicate is found.

In the worst case,

```
O(n²)
```

**Space Complexity:** **O(n)**

- HashSet stores characters of the current substring.

---

# Better Approach

## Idea

Use a sliding window and a HashSet.

Expand the window by moving the right pointer.

If a duplicate character appears, keep removing characters from the left until the duplicate is removed.

The window always contains unique characters.

## Algorithm

1. Create an empty HashSet.
2. Initialize two pointers (`left`, `right`).
3. Expand the window by moving `right`.
4. If a duplicate appears, remove characters from the left until it disappears.
5. Update the maximum window length.
6. Return the answer.

### Java Code

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {

        HashSet<Character> set = new HashSet<>();

        int left = 0;
        int ans = 0;

        for(int right = 0; right < s.length(); right++) {

            while(set.contains(s.charAt(right))) {
                set.remove(s.charAt(left));
                left++;
            }

            set.add(s.charAt(right));

            ans = Math.max(ans, right - left + 1);
        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every character enters the window once.
- Every character leaves the window once.

Although there is a nested `while` loop, each character is removed only once.

Therefore,

```
O(2n)

Ignoring constants,

O(n)
```

**Space Complexity:** **O(n)**

- HashSet stores characters currently inside the window.

---

# Optimal Approach

## Idea

In the previous approach, when a duplicate is found, we remove characters one by one.

Instead, store the **last index** of every character using a HashMap.

Whenever a duplicate is found inside the current window, move the left pointer directly after its previous occurrence.

This avoids unnecessary removals.

## Algorithm

1. Create a HashMap.
2. Initialize `left = 0`.
3. Traverse the string using the right pointer.
4. If the current character already exists inside the current window, update `left`.
5. Store the latest index of the character.
6. Update the maximum window length.
7. Return the answer.

### Java Code

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {

        HashMap<Character, Integer> map = new HashMap<>();

        int left = 0;
        int ans = 0;

        for(int right = 0; right < s.length(); right++) {

            char ch = s.charAt(right);

            if(map.containsKey(ch)) {
                left = Math.max(left, map.get(ch) + 1);
            }

            map.put(ch, right);

            ans = Math.max(ans, right - left + 1);
        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- The string is traversed only once.
- HashMap operations (`containsKey()`, `get()`, `put()`) take **O(1)** on average.

Overall,

```
O(n)
```

**Space Complexity:** **O(n)**

- HashMap stores the last occurrence of each character.

  C++
  ```cpp
  class Solution {
  public:
      int lengthOfLongestSubstring(string s) {
          unordered_map<char, int> map;
          int i=0, j=0, ans=0;
          while(j<s.size())
          {
              if(map.find(s[j])!=map.end())
              {
                  i=max(i, map[s[j]]+1);
                  // max because what if previous occurence is behind the current window
              }
              ans=max(ans, j-i+1);
              map[s[j]]=j;
              j++;
          }
          return ans;
      }
  };
  ```
