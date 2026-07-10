# 139. Word Break

**LeetCode:** https://leetcode.com/problems/word-break/

**Difficulty:** Medium

---

# Pattern(s)

- Dynamic Programming
- Recursion
- Memoization
- Tabulation
- String

---

# Pattern Recognition

Look for this approach when:

- The string needs to be broken into smaller valid parts.
- The same substring is checked repeatedly.
- The answer is either **possible/not possible**.
- Every decision depends on future indices⭐️.

---

# Intuition

Starting from index `0`, we try every possible word that begins at the current index.

If a chosen word exists in the dictionary, recursively check whether the remaining string can also be broken.

The brute-force approach tries every possible partition.

Memoization stores whether a particular starting index can successfully reach the end.

Tabulation computes the answer from the end of the string towards the beginning.

---

# Brute Force (Recursion)

## Idea

Starting from the current index,

Try every possible ending index.

If the substring belongs to the dictionary, recursively solve the remaining string.

If any path reaches the end of the string, return `true`.

## Algorithm

1. Start from index `0`.
2. Try every substring starting from the current index.
3. If the substring exists in the dictionary:
   - Recursively solve the remaining string.
4. If any recursive call returns `true`, return `true`.
5. Otherwise return `false`.

### Java Code

```java
class Solution {

    private boolean solve(int index, String s, HashSet<String> set) {

        if(index == s.length()) {
            return true;
        }

        for(int i = index; i < s.length(); i++) {

            String word = s.substring(index, i + 1);  // endExclusive

            if(set.contains(word)) {

                if(solve(i + 1, s, set)) {
                    return true;
                }

            }

        }

        return false;
    }

    public boolean wordBreak(String s, List<String> wordDict) {

        HashSet<String> set = new HashSet<>(wordDict);

        return solve(0, s, set);
    }
}
```

### Complexity Analysis

Should I cut here or not?

**Time Complexity:** **O(2ⁿ)**

- Every position may branch into multiple recursive calls.

**Space Complexity:** **O(n)**

- Maximum recursion depth is `n`.

---

# Better Approach (Recursion + Memoization)

## Idea

Many recursive calls start from the same index.

Instead of solving repeatedly,

store whether the string starting from each index can be segmented.

## Algorithm

1. Create a DP array initialized with `-1`.
2. If the answer for an index is already known, return it.
3. Try every possible substring.
4. Store whether segmentation is possible.
5. Return the stored answer.

### Java Code

```java
class Solution {

    private boolean solve(int index,
                          String s,
                          HashSet<String> set,
                          int[] dp) {

        if(index == s.length()) {
            return true;
        }

        if(dp[index] != -1) {
            return dp[index] == 1;
        }

        for(int i = index; i < s.length(); i++) {

            String word = s.substring(index, i + 1);

            if(set.contains(word)) {

                if(solve(i + 1, s, set, dp)) {

                    dp[index] = 1;

                    return true;

                }

            }

        }

        dp[index] = 0;

        return false;
    }

    public boolean wordBreak(String s, List<String> wordDict) {

        HashSet<String> set = new HashSet<>(wordDict);

        int[] dp = new int[s.length()];

        Arrays.fill(dp, -1);

        return solve(0, s, set, dp);
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**
n states with n work
- There are `n` starting indices.
- For each index, we try all possible ending indices.

*(Ignoring the cost of `substring()` as is commonly done in interviews.)*

**Space Complexity:** **O(n)**

- DP array.
- Recursion stack.

---

# Optimal Approach (Tabulation)

## Idea

Let

```
dp[i]
```

represent whether the substring

```
s[i...n-1]
```

can be segmented.

The recursion starts from the beginning.

The tabulation works backwards.

If a valid word starts at `i` and the remaining suffix can also be segmented, then

```
dp[i] = true
```

The base case is

```
dp[n] = true
```

because an empty string is already segmented.

## Algorithm

1. Create a DP array of size `n+1`.
2. Set `dp[n] = true`.
3. Traverse indices from right to left.
4. Try every possible ending index.
5. If the current word exists in the dictionary and `dp[end+1]` is true:
   - Mark `dp[i] = true`.
6. Return `dp[0]`.

### Java Code

```java
class Solution {

    public boolean wordBreak(String s, List<String> wordDict) {

        HashSet<String> set = new HashSet<>(wordDict);

        int n = s.length();

        boolean[] dp = new boolean[n + 1];

        dp[n] = true;

        for(int i = n - 1; i >= 0; i--) {

            for(int j = i; j < n; j++) {

                String word = s.substring(i, j + 1);

                if(set.contains(word) && dp[j + 1]) {

                    dp[i] = true;

                    break;

                }

            }

        }

        return dp[0];
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- Two nested loops.

*(Ignoring substring creation cost in interview analysis.)*

**Space Complexity:** **O(n)**

- DP array.

  C++
  ```cpp
  class Solution {
    bool rec(int idx, string& s, unordered_set<string>& st, vector<int>& dp)
    {
        if(idx==s.size())
        {
            return true;
        }
        if(dp[idx]!=-1)
        {
            return dp[idx];
        }
        for(int i=idx;i<s.size();i++)
        {
            string temp=s.substr(idx, i-idx+1);
            if(st.find(temp)!=st.end())
            {
                if(rec(i+1, s, st, dp))
                {
                    dp[idx]=1;
                    return true;
                }
            }
        }
        dp[idx]=0;
        return false;
    }
  public:
      bool wordBreak(string s, vector<string>& wordDict) {
          unordered_set<string> st(wordDict.begin(), wordDict.end());
          vector<int> dp(s.size(), -1);
          return rec(0, s, st, dp);   
      }
  };
  OR
  class Solution {
  public:
      bool wordBreak(string s, vector<string>& wordDict) {
          int n=s.size();
          unordered_set<string> st(wordDict.begin(), wordDict.end());
          vector<bool> dp(n+1);
          dp[n]=true;
          for(int i=n-1;i>=0;i--)
          {
              for(int j=i;j<n;j++)
              {
                  string temp=s.substr(i, j-i+1);
                  if(st.find(temp)!=st.end() && dp[j+1])
                  {
                      dp[i]=true;
                      break;
                  }
              }
          }
          return dp[0];
      }
  };
  ```
