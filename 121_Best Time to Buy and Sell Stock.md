# 121. Best Time to Buy and Sell Stock

**LeetCode:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

**Difficulty:** Easy

---

# Pattern(s)

- Array
- Greedy
- Prefix Minimum

---

# Intuition

We need to maximize the profit by buying the stock before selling it.

For every selling day, the profit depends on the **lowest buying price before that day**.

The brute force approach checks every possible buying and selling pair, which is inefficient.

A better observation is that for every buying day, we only need the **maximum selling price available in the future**.

An even better observation is that while traversing the array from left to right, we can simply keep track of the **minimum buying price seen so far**. For each day, we calculate the profit if we sell on that day and update the maximum profit.

This reduces the time complexity to **O(n)** while using only **O(1)** extra space.

---

# Brute Force

## Idea

Try every possible buying day.

For each buying day, check every possible selling day after it and calculate the profit.

Return the maximum profit.

## Algorithm

1. Select every day as the buying day.
2. Check every future day as the selling day.
3. Calculate the profit.
4. Update the maximum profit.
5. Return the answer.

### Java Code

```java
class Solution {
    public int maxProfit(int[] prices) {

        int maxProfit = 0;

        for(int i = 0; i < prices.length; i++) {

            for(int j = i + 1; j < prices.length; j++) {

                maxProfit = Math.max(maxProfit, prices[j] - prices[i]);

            }

        }

        return maxProfit;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- The outer loop selects every buying day.
- The inner loop checks every possible selling day after it.
- Therefore, approximately `n × n` comparisons are performed.

**Space Complexity:** **O(1)**

- No extra data structure is used.

---

# Better Approach

## Idea

Instead of checking every future selling day repeatedly, precompute the **maximum selling price available after every index**.

Create an array `right[]` where:

```
right[i] = maximum stock price after index i
```

Now the maximum profit for buying on day `i` becomes:

```
right[i] - prices[i]
```

This avoids repeatedly searching for the maximum future price.

## Algorithm

1. Create an array `right[]`.
2. Traverse from right to left.
3. Store the maximum future stock price at every index.
4. Traverse the array again.
5. Calculate the profit for every buying day.
6. Return the maximum profit.

### Java Code

```java
class Solution {
    public int maxProfit(int[] prices) {

        int n = prices.length;

        int[] right = new int[n];

        right[n - 1] = -1;

        for(int i = n - 2; i >= 0; i--) {
            right[i] = Math.max(right[i + 1], prices[i + 1]);
        }

        int ans = 0;

        for(int i = 0; i < n; i++) {
            ans = Math.max(ans, right[i] - prices[i]);
        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- One traversal to build the `right[]` array.
- One traversal to calculate the answer.

Overall,

```
O(n + n) = O(n)
```

**Space Complexity:** **O(n)**

- Extra array `right[]` of size `n` is used.

---

# Optimal Approach

## Idea

While traversing the array, always remember the **minimum stock price seen so far**.

For every day's price:

- Calculate the profit if we sell on that day.
- Update the maximum profit.
- Update the minimum buying price for future days.

This eliminates the need for the extra `right[]` array.

## Algorithm

1. Initialize `minPrice` with the first day's price.
2. Initialize `maxProfit` as `0`.
3. Traverse the array from left to right.
4. Calculate the profit using the minimum price seen so far.
5. Update the maximum profit.
6. Update the minimum buying price.
7. Return the answer.

### Java Code

```java
class Solution {
    public int maxProfit(int[] prices) {

        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.length; i++) {

            int profit = prices[i] - minPrice;

            maxProfit = Math.max(maxProfit, profit);

            minPrice = Math.min(minPrice, prices[i]);
        }

        return maxProfit;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- The array is traversed only once.
- Every iteration performs constant-time operations.
- Therefore, the overall time complexity is **O(n)**.

**Space Complexity:** **O(1)**

- Only two variables (`minPrice` and `maxProfit`) are used.
- No extra space proportional to the input size is required.

  ```cpp
  class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice=prices[0];
        int maxProfit=0;
        for(int num:prices)
        {
            maxProfit=max(maxProfit, num-minPrice);
            minPrice=min(minPrice, num);
        }
        return maxProfit;
    }
};
```
