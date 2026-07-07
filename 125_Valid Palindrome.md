# 125. Valid Palindrome

**LeetCode:** https://leetcode.com/problems/valid-palindrome/

**Difficulty:** Easy

---

# Pattern(s)

- Two Pointers
- String

---

# Pattern Recognition

Look for this approach when:

- You need to compare characters from both ends.
- The problem asks whether a string is a palindrome.
- Certain characters need to be ignored (spaces, punctuation, etc.).
- Extra space should be minimized.

---

# Intuition

A palindrome reads the same from left to right and right to left.

Since we need to compare characters from both ends, using two pointers is a natural choice.

The main challenge is that the problem ignores all non-alphanumeric characters and is case-insensitive.

We first skip all invalid characters, convert valid characters to lowercase, compare them, and continue moving towards the center.

If any pair of characters differs, the string is not a palindrome.

---

# Brute Force

## Idea

Create a new string containing only lowercase alphanumeric characters.

Then check whether the cleaned string is a palindrome.

## Algorithm

1. Traverse the original string.
2. Keep only lowercase letters and digits.
3. Store them in a new string.
4. Check whether the new string is a palindrome.
5. Return the result.

### Java Code

```java
class Solution {
    public boolean isPalindrome(String s) {

        StringBuilder str = new StringBuilder();

        for(char ch : s.toCharArray()) {

            if(Character.isLetterOrDigit(ch)) {
                str.append(Character.toLowerCase(ch));
            }

        }

        String cleaned = str.toString();

        int left = 0;
        int right = cleaned.length() - 1;

        while(left < right) {

            if(cleaned.charAt(left) != cleaned.charAt(right)) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- One traversal to build the cleaned string.
- One traversal to check the palindrome.

Overall,

```
O(n + n) = O(n)
```

**Space Complexity:** **O(n)**

- Extra string is created to store valid characters.

---

# Better Approach

There is no commonly used intermediate approach for this problem.

The optimization directly moves from the brute force solution to the optimal two-pointer solution.

---

# Optimal Approach

## Idea

Instead of creating another string, use two pointers directly on the original string.

- Move the left pointer until an alphanumeric character is found.
- Move the right pointer until an alphanumeric character is found.
- Compare both characters after converting them to lowercase.
- If they differ, return `false`.
- Otherwise, continue until the pointers meet.

This avoids creating an extra string.

## Algorithm

1. Initialize two pointers.
2. Skip non-alphanumeric characters.
3. Compare lowercase characters.
4. If they differ, return `false`.
5. Move both pointers.
6. Return `true` if all characters match.

### Java Code

```java
class Solution {
    public boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while(left < right) {

            while(left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }

            while(left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            if(Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Each character is visited at most once by either the left or right pointer.
- Therefore, the entire string is traversed only once.

Overall,

```
O(n)
```

**Space Complexity:** **O(1)**

- Only two pointers are used.
- No extra string or data structure is created.

C++
```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int l=0;
        int r=s.size()-1;
        while(l<r)
        {
            while(l<r && !isalnum(s[l]))
            {
                l++;
            }
            while(l<r && !isalnum(s[r]))
            {
                r--;
            }
            if(tolower(s[l])!=tolower(s[r]))
            {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
};
```
