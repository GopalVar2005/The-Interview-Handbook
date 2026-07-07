# 53. Maximum Subarray

**LeetCode:** https://leetcode.com/problems/maximum-subarray/

**Difficulty:** Medium

---

# Pattern(s)

- Array
- Kadane's Algorithm

---

# Pattern Recognition

Look for this approach when:

- You need the maximum/minimum sum of a contiguous subarray.
- The problem involves contiguous elements.
- The previous subarray can either help or hurt the current answer.
- You need an O(n) solution instead of checking all subarrays.

---

# Intuition

We need to find the contiguous subarray having the maximum sum.

The brute force approach generates every possible subarray and calculates its sum.

A better approach avoids recalculating the sum every time by using a running sum.

The optimal approach (Kadane's Algorithm) observes that if the running sum becomes negative, it will only decrease the sum of any future subarray. Therefore, we discard it and start a new subarray from the current element.

This allows us to solve the problem in a single traversal.

---

# Brute Force

## Idea

Generate every possible subarray.

For every subarray, calculate its sum separately and update the maximum sum.

## Algorithm

1. Choose every index as the starting point.
2. Choose every possible ending point.
3. Calculate the sum of that subarray.
4. Update the maximum sum.
5. Return the maximum sum.

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int ans = Integer.MIN_VALUE;

        for(int i = 0; i < nums.length; i++) {

            for(int j = i; j < nums.length; j++) {

                int sum = 0;

                for(int k = i; k <= j; k++) {
                    sum += nums[k];
                }

                ans = Math.max(ans, sum);

            }

        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n³)**

- First loop selects the starting index.
- Second loop selects the ending index.
- Third loop calculates the subarray sum.

Overall,

```
O(n × n × n) = O(n³)
```

**Space Complexity:** **O(1)**

- No extra data structure is used.

---

# Better Approach

## Idea

Instead of calculating the sum of every subarray from scratch, maintain a running sum.

For every starting index, keep extending the subarray and keep adding the next element.

This removes the third loop.

## Algorithm

1. Select every index as the starting point.
2. Initialize `sum = 0`.
3. Extend the subarray one element at a time.
4. Keep updating the running sum.
5. Update the maximum answer.

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int ans = Integer.MIN_VALUE;

        for(int i = 0; i < nums.length; i++) {

            int sum = 0;

            for(int j = i; j < nums.length; j++) {

                sum += nums[j];

                ans = Math.max(ans, sum);

            }

        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- Outer loop selects the starting index.
- Inner loop extends the subarray.
- Running sum is updated in O(1).

Overall,

```
O(n × n) = O(n²)
```

**Space Complexity:** **O(1)**

- No extra data structure is used.

---

# Optimal Approach (Kadane's Algorithm)

## Idea

While traversing the array, maintain the sum of the current subarray.

If the running sum becomes negative, it can never increase the sum of any future subarray.

So instead of carrying a negative sum forward, discard it and start a new subarray from the next element.

At every step, update the maximum sum obtained so far.

## Algorithm

1. Initialize `sum = 0`.
2. Initialize `maxSum = Integer.MIN_VALUE`.
3. Traverse the array.
4. Add the current element to the running sum.
5. Update the maximum sum.
6. If the running sum becomes negative, reset it to `0`.
7. Return the maximum sum.

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int sum = 0;
        int maxSum = Integer.MIN_VALUE;

        for(int num : nums) {

            sum += num;

            maxSum = Math.max(maxSum, sum);

            if(sum < 0) {
                sum = 0;
            }

        }

        return maxSum;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- The array is traversed only once.
- Every iteration performs constant-time operations.

Overall,

```
O(n)
```

**Space Complexity:** **O(1)**

- Only two variables (`sum` and `maxSum`) are used.
- No extra space proportional to the input size is required.

C++
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum=0;
        int maxSum=INT_MIN;
        for(int num:nums)
        {
            sum+=num;
            maxSum=max(maxSum, sum);
            if(sum<0)
            {
                sum=0;
            }
        }
        return maxSum;
    }
};
```
