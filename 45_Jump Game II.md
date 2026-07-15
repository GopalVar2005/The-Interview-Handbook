# 45. Jump Game II

**LeetCode:** https://leetcode.com/problems/jump-game-ii/

**Difficulty:** Medium

---

# Pattern(s)

- Greedy
- Array

---

# Pattern Recognition

Look for this approach when:

- You need the **minimum number of jumps** to reach the end.
- Every position tells you the maximum jump length.
- The array is guaranteed to be reachable.
- You need to minimize the number of jumps.

---

# Intuition

Unlike Jump Game I,

here we need the **minimum** number of jumps.

The brute force tries every possible jump.

The better approach stores the minimum jumps for every index using Memoization.

The optimal greedy approach views the array level by level.

From all indices reachable using the current number of jumps,

find the farthest position that can be reached using one more jump.

Once we've explored the current range,

we take one jump and move to the next range.

---

# Brute Force

## Idea

From every index,

try every possible jump.

Return the minimum jumps required among all choices.

## Algorithm

1. Start from index `0`.
2. Try every jump from `1` to `nums[index]`.
3. Recursively compute the answer.
4. Return the minimum value.

### Java Code

```java
class Solution {

    private int rec(int index, int[] nums) {

        if(index >= nums.length - 1) {
            return 0;
        }

        int ans = Integer.MAX_VALUE;

        for(int jump = 1; jump <= nums[index]; jump++) {

            ans = Math.min(ans,
                    1 + rec(index + jump, nums));

        }

        return ans;

    }

    public int jump(int[] nums) {

        return rec(0, nums);

    }

}
```

### Complexity Analysis

**Time Complexity:** **Exponential (Worst Case)**

- Every index can recursively explore multiple jumps.

Approximately,

```
O(2ⁿ)
```

**Space Complexity:** **O(n)**

- Maximum recursion depth.

---

# Better Approach

## Idea

The recursive solution recomputes the same index many times.

Store the answer for every index.

Whenever an index is visited again,

return the stored answer.

## Algorithm

1. Create a DP array initialized with `-1`.
2. If an answer is already computed,
   return it.
3. Otherwise,
   recursively try every jump.
4. Store the minimum answer.

### Java Code

```java
class Solution {

    private int rec(int index,
                    int[] nums,
                    int[] dp) {

        if(index >= nums.length - 1) {
            return 0;
        }

        if(dp[index] != -1) {
            return dp[index];
        }

        int ans = Integer.MAX_VALUE;

        for(int jump = 1; jump <= nums[index]; jump++) {

            int next = rec(index + jump,
                           nums,
                           dp);

            ans = Math.min(ans,
                    1 + next);

        }

        return dp[index] = ans;

    }

    public int jump(int[] nums) {

        int[] dp = new int[nums.length];

        Arrays.fill(dp, -1);

        return rec(0, nums, dp);

    }

}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- There are `n` states.
- Each state may try up to `n` jumps.

Overall,

```
O(n²)
```

**Space Complexity:** **O(n)**

- DP array.
- Recursion stack.

---

# Optimal Approach

## Idea

Think of every jump as covering a range.

Initially,

```
Current Range

[0...0]
```

From every index inside this range,

find the farthest position that can be reached.

Once the current range ends,

take one jump,

and make the new range equal to the farthest reachable position.

Repeat until the last index is reached.

## Algorithm

1. Initialize:
   - `maxReach = 0`
   - `farthest = 0`
   - `jumps = 0`
2. Traverse the array.
3. Update

```
farthest =
max(farthest, i + nums[i])
```

4. If the current range ends:
   - Increase jumps.
   - Update the current range.
5. Return the number of jumps.

### Java Code

```java
class Solution {

    public int jump(int[] nums) {

        int maxReach = 0;
        int farthest = 0;
        int jumps = 0;

        for(int i = 0; i < nums.length; i++) {

            farthest =
                    Math.max(farthest,
                             i + nums[i]);

            if(i == maxReach &&
               i != nums.length - 1) {

                jumps++;

                maxReach = farthest;

            }

        }

        return jumps;

    }

}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every index is visited exactly once.

Overall,

```
O(n)
```

**Space Complexity:** **O(1)**

- Only a few variables are used.
