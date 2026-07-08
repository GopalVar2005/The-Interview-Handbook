# 198. House Robber

**LeetCode:** https://leetcode.com/problems/house-robber/

**Difficulty:** Medium

---

# Pattern(s)

- Dynamic Programming
- Recursion
- Memoization
- Space Optimization

---

# Pattern Recognition

Look for this approach when:

- You need to maximize or minimize a value.
- At each step, you have two choices.
- Choosing one option affects future choices.
- The same subproblems are solved repeatedly.

---

# Intuition

A robber cannot rob two adjacent houses.

For every house, there are only two choices:

- Rob the current house and skip the next one.
- Skip the current house and move to the next house.

The answer is the maximum of these two choices.

The recursive solution repeatedly solves the same subproblems.

Memoization stores previously computed answers to avoid recomputation.

Finally, we observe that only the previous two DP states are required, allowing us to optimize the space to **O(1)**.

---

# Brute Force (Recursion)

## Idea

For every house, make two recursive choices:

- Rob it.
- Skip it.

Return the maximum amount obtained.

## Algorithm

1. Start from index `0`.
2. If the current index is outside the array, return `0`.
3. Rob the current house and move to `index + 2`.
4. Skip the current house and move to `index + 1`.
5. Return the maximum of both choices.

### Java Code

```java
class Solution {

    public int rob(int[] nums) {
        return solve(0, nums);
    }

    private int solve(int index, int[] nums) {

        if(index >= nums.length) {
            return 0;
        }

        int rob = nums[index] + solve(index + 2, nums);

        int skip = solve(index + 1, nums);

        return Math.max(rob, skip);
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(2ⁿ)**

- Every house generates two recursive calls.
- Many subproblems are solved repeatedly.

**Space Complexity:** **O(n)**

- Due to the recursion stack.

---

# Better Approach (Recursion + Memoization)

## Idea

The recursive solution repeatedly calculates the answer for the same index.

Store the answer for every index in a DP array.

Whenever a subproblem is encountered again, return the stored answer instead of recomputing it.

## Algorithm

1. Create a DP array initialized with `-1`.
2. If the answer for the current index is already stored, return it.
3. Otherwise,
   - Rob the current house.
   - Skip the current house.
4. Store the maximum answer.
5. Return the stored answer.

### Java Code

```java
import java.util.Arrays;

class Solution {

    public int rob(int[] nums) {

        int[] dp = new int[nums.length];

        Arrays.fill(dp, -1);

        return solve(0, nums, dp);
    }

    private int solve(int index, int[] nums, int[] dp) {

        if(index >= nums.length) {
            return 0;
        }

        if(dp[index] != -1) {
            return dp[index];
        }

        int rob = nums[index] + solve(index + 2, nums, dp);

        int skip = solve(index + 1, nums, dp);

        dp[index] = Math.max(rob, skip);

        return dp[index];
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every index is solved only once.

**Space Complexity:** **O(n)**

- **O(n)** for the DP array.
- **O(n)** recursion stack.

Overall auxiliary space is **O(n)**.

---

# Optimal Approach

## Idea

Instead of storing the answer for every house, observe the recurrence:

```
dp[i] = max(dp[i-1], nums[i] + dp[i-2])
```

To calculate the current answer, we only need:

- Previous answer (`dp[i-1]`)
- Answer two houses back (`dp[i-2]`)

So we replace the entire DP array with two variables.

## Algorithm

1. Handle the single house case.
2. Initialize:
   - `prev2 = nums[0]`
   - `prev1 = max(nums[0], nums[1])`
3. Traverse the remaining houses.
4. Compute the current answer.
5. Shift the previous answers.
6. Return the final answer.

### Java Code

```java
class Solution {

    public int rob(int[] nums) {

        int n = nums.length;

        if(n == 1) {
            return nums[0];
        }

        int prev2 = nums[0];
        int prev1 = Math.max(nums[0], nums[1]);

        for(int i = 2; i < n; i++) {

            int current = Math.max(prev1, nums[i] + prev2);

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every house is processed exactly once.

**Space Complexity:** **O(1)**

- Only three integer variables are used.

  C++
  ```cpp
    class Solution {
    int rec(int idx, vector<int>& nums, vector<int>& dp)
    {
        if(idx>=nums.size())
        {
            return 0;
        }
        if(dp[idx]!=-1)
        {
            return dp[idx];
        }
        int ans=nums[idx]+rec(idx+2, nums, dp);
        ans=max(ans, rec(idx+1, nums, dp));
        return dp[idx]=ans;
    }
  public:
      int rob(vector<int>& nums) {
          vector<int> dp(nums.size(), -1);
          return rec(0, nums, dp);
      }
  };
  OR
  class Solution {
  public:
      int rob(vector<int>& nums) {
          int n=nums.size();
          int prev1=nums[0];
          if(n==1)return prev1;
          int prev2=max(nums[0], nums[1]);
          if(n==2)return prev2;
          for(int i=2;i<nums.size();i++)
          {
              int cur=max(prev, nums[i]+prev1);
              prev1=prev2;
              prev2=cur;
          }
          return prev2;
      }
  };
  ```
