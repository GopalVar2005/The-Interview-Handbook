# 70. Climbing Stairs

**LeetCode:** https://leetcode.com/problems/climbing-stairs/

**Difficulty:** Easy

---

# Pattern(s)

- Dynamic Programming
- Fibonacci
- Space Optimization

---

# Pattern Recognition

Look for this approach when:

- The answer depends on previous states.
- You need to count the number of ways.
- There are only a few choices at each step.
- The same subproblems are solved repeatedly.

---

# Intuition

To reach the `n`th stair, there are only two possibilities:

- Come from the `(n-1)`th stair by taking one step.
- Come from the `(n-2)`th stair by taking two steps.

Therefore,

```
ways(n) = ways(n-1) + ways(n-2)
```

This is exactly the Fibonacci recurrence.

The brute force solution recursively computes the same subproblems multiple times.

Using Dynamic Programming, we store previously computed results and avoid redundant calculations.

Finally, since only the last two states are required, we can optimize the space to **O(1)**.

---

# Brute Force (Recursion)

## Idea

At every stair, we have two choices:

- Climb one step.
- Climb two steps.

Recursively calculate both possibilities.

## Algorithm

1. If `n <= 2`, return `n`.
2. Otherwise,
   - Calculate ways from `n-1`.
   - Calculate ways from `n-2`.
3. Return their sum.

### Java Code

```java
class Solution {

    public int climbStairs(int n) {

        if(n <= 2) {
            return n;
        }

        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(2ⁿ)**

- Every function call generates two more recursive calls.
- Many subproblems are solved repeatedly.

**Space Complexity:** **O(n)**

- Due to the recursion stack.

---

# Better Approach (Dynamic Programming)

## Idea

Store the answer for every stair.

Instead of solving the same problem repeatedly, use previously computed answers.

## Algorithm

1. Create a DP array.
2. Initialize the base cases.
3. Compute the answer for every stair.
4. Return `dp[n]`.

### Java Code

```java
class Solution {

    public int climbStairs(int n) {

        int[] dp = new int[n + 1];

        return solve(n, dp);
    }

    private int solve(int n, int[] dp) {

        if(n <= 2) {
            return n;
        }

        if(dp[n] != 0) {
            return dp[n];
        }

        dp[n] = solve(n - 1, dp) + solve(n - 2, dp);

        return dp[n];
    }
}

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every stair is computed exactly once.

**Space Complexity:** **O(n)**

- DP array stores the answer for every stair.

---

# Optimal Approach

## Idea

Observe that to calculate the current answer, we only need the previous two answers.

Instead of storing the entire DP array, keep only two variables.

This reduces the space complexity from **O(n)** to **O(1)**.

## Algorithm

1. Handle base cases.
2. Store the first two answers.
3. Iterate from `3` to `n`.
4. Calculate the current answer.
5. Shift the previous answers.
6. Return the final answer.

### Java Code

```java
class Solution {
    public int climbStairs(int n) {

        if(n <= 2) {
            return n;
        }

        int prev2 = 1;
        int prev1 = 2;

        for(int i = 3; i <= n; i++) {

            int current = prev1 + prev2;

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every stair is computed exactly once.

**Space Complexity:** **O(1)**

- Only three integer variables are used.

  C++
  ```cpp
  class Solution {
    int rec(int n, vector<int>& dp)
    {
        if(n==0)
        {
            return 1;
        }
        if(n<0)
        {
            return 0;
        }
        if(dp[n]!=-1)
        {
            return dp[n];
        }
        return dp[n]=rec(n-1, dp)+rec(n-2, dp);
    }
  public:
      int climbStairs(int n) {
          vector<int> dp(n+1, -1);
          return rec(n, dp);
      }
  };
    OR
    class Solution {
    public:
        int climbStairs(int n) {
            if(n<=2)return n;
            int prev1=1;
            int prev2=2;
            for(int i=3;i<=n;i++)
            {
                int temp=prev2;
                prev2+=prev1;
                prev1=temp;
            }
            return prev2;
        }
    };
  ```
