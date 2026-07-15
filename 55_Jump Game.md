# 55. Jump Game

**LeetCode:** https://leetcode.com/problems/jump-game/

**Difficulty:** Medium

---

# Pattern(s)

- Greedy
- Array

---

# Pattern Recognition

Look for this approach when:

- You need to determine whether the last index can be reached.
- Every position tells you the maximum distance you can move.
- You only need to answer **Yes/No**, not the minimum number of jumps.

---

# Intuition

From every index,

we can jump up to `nums[i]` positions ahead.

The brute force approach tries every possible jump.

The optimal greedy approach keeps track of the **farthest index** that can be reached.

If we ever reach an index that is beyond our farthest reachable position,

it is impossible to move further.

Otherwise,

keep updating the farthest reachable index.

---

# Brute Force

## Idea

From every index,

try all possible jumps.

If any jump reaches the last index,

return `true`.

Otherwise,

return `false`.

## Algorithm

1. Start from index `0`.
2. Try every jump from `1` to `nums[i]`.
3. Recursively check whether the destination can reach the end.
4. If any path succeeds, return `true`.
5. Otherwise, return `false`.

### Java Code

```java
class Solution {

    private boolean rec(int index, int[] nums) {

        if(index >= nums.length - 1) {
            return true;
        }

        for(int jump = 1; jump <= nums[index]; jump++) {

            if(rec(index + jump, nums)) {
                return true;
            }

        }

        return false;

    }

    public boolean canJump(int[] nums) {

        return rec(0, nums);

    }

}
```

### Complexity Analysis

**Time Complexity:** **O(2ⁿ)**
(here one choice->nums[i] recursive calls, means exponential that's why 2^n)

- At every index, multiple recursive calls are made.
- Many states are recomputed repeatedly.

**Space Complexity:** **O(n)**

- Maximum recursion depth is `n`.

---

# Better Approach

## Idea

The recursion solves the same index many times.

Store the answer for every index.

If an index has already been computed,

return the stored result.

## Algorithm

1. Create a DP array.
2. If the answer for an index is already known,
   return it.
3. Otherwise,
   recursively try every possible jump.
4. Store the result.

### Java Code

```java
class Solution {

    private boolean rec(int index,
                        int[] nums,
                        int[] dp) {

        if(index >= nums.length - 1) {
            return true;
        }

        if(dp[index] != -1) {
            return dp[index] == 1;
        }

        for(int jump = 1; jump <= nums[index]; jump++) {

            if(rec(index + jump, nums, dp)) {

                dp[index] = 1;
                return true;

            }

        }

        dp[index] = 0;

        return false;

    }

    public boolean canJump(int[] nums) {

        int[] dp = new int[nums.length];

        Arrays.fill(dp, -1);

        return rec(0, nums, dp);

    }

}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- There are `n` states.
- From each state, we may try up to `n` jumps.

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

Keep track of the **farthest index** that can currently be reached.

Initially,

```
maxReach = 0
```

For every index:

- If the current index is beyond `maxReach`,
  it means we cannot even reach this index.

- Otherwise,
  update

```
maxReach = max(maxReach, i + nums[i])
```

If we finish traversing the array,

the last index is reachable.

## Algorithm

1. Initialize `maxReach = 0`.
2. Traverse every index.
3. If `i > maxReach`,
   return `false`.
4. Update

```
maxReach = max(maxReach, i + nums[i])
```

5. If traversal finishes,
   return `true`.

### Java Code

```java
class Solution {

    public boolean canJump(int[] nums) {

        int maxReach = 0;

        for(int i = 0; i < nums.length; i++) {

            if(i > maxReach) {
                return false;
            }

            maxReach = Math.max(maxReach,
                                i + nums[i]);

        }

        return true;

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

- Only one variable is maintained.
