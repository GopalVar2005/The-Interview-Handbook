# 875. Koko Eating Bananas

**LeetCode:** https://leetcode.com/problems/koko-eating-bananas/

**Difficulty:** Medium

---

# Pattern(s)

- Binary Search on Answer
- Binary Search

---

# Pattern Recognition

Look for this approach when:

- The problem asks for the **minimum** or **maximum** possible answer.
- The answer lies in a range of values.
- You can verify whether a particular answer works.
- If one answer works, every larger (or smaller) answer also works.

---

# Intuition

Koko can choose any eating speed.

- Smaller speed → More hours required.
- Larger speed → Fewer hours required.

Notice the monotonic property:

```
Speed ↑

↓

Hours Needed ↓
```

If a speed is enough to finish all bananas within `h` hours,

then every larger speed will also work.

Instead of checking every possible speed, we can Binary Search on the answer.

The search space is:

```
1 ........ Maximum Pile
```

- Minimum possible speed = `1`
- Maximum useful speed = `Maximum Pile`

---

# Brute Force

## Idea

Try every possible eating speed from `1` to the maximum pile.

For every speed,

calculate the total number of hours required.

The first speed that finishes within `h` hours is the answer.

## Algorithm

1. Find the maximum pile.
2. Try every speed from `1` to `maxPile`.
3. Calculate the total hours required.
4. If the hours are within `h`, return the current speed.

### Java Code

```java
class Solution {

    public int minEatingSpeed(int[] piles, int h) {

        int maxPile = 0;

        for(int pile : piles) {
            maxPile = Math.max(maxPile, pile);
        }

        for(int speed = 1; speed <= maxPile; speed++) {

            long hours = 0;

            for(int pile : piles) {

                hours += (long)Math.ceil((double)pile / speed);

            }

            if(hours <= h) {
                return speed;
            }

        }

        return -1;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n × maxPile)**

- We try every possible speed.
- For every speed, we traverse all piles once.

**Space Complexity:** **O(1)**

- No extra space is used.

---

# Better Approach

There is no meaningful intermediate approach.

The optimization directly uses Binary Search on Answer.

---

# Optimal Approach

## Idea

Instead of checking every speed,

Binary Search the answer.

The search space is:

```
1 ........ Maximum Pile
```

For every middle speed,

calculate the total hours required.

- If the hours are within `h`,
  - store the answer,
  - try a smaller speed.
- Otherwise,
  - increase the speed.

The first valid speed will be the minimum possible answer.

## Algorithm

1. Find the maximum pile.
2. Set:
   - `low = 1`
   - `high = maxPile`
3. While `low <= high`:
   - Calculate `mid`.
   - Compute total hours required.
   - If hours ≤ `h`:
     - Save the answer.
     - Search the left half.
   - Otherwise:
     - Search the right half.
4. Return the answer.

### Java Code

```java
class Solution {

    public int minEatingSpeed(int[] piles, int h) {

        int maxPile = 0;

        for(int pile : piles) {
            maxPile = Math.max(maxPile, pile);
        }

        int low = 1;
        int high = maxPile;

        int ans = maxPile;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            long hours = 0;

            for(int pile : piles) {

                hours += (long)Math.ceil((double)pile / mid);

            }

            if(hours <= h) {

                ans = mid;
                high = mid - 1;

            }
            else {

                low = mid + 1;

            }

        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n × log(maxPile))**

- Binary Search performs `log(maxPile)` iterations.
- In every iteration, we traverse all piles once.

Overall,

```
O(n × log(maxPile))
```

**Space Complexity:** **O(1)**

- No extra space is used.

C++
```cpp
class Solution {
    long long time(vector<int>& piles, int mid)
    {
        long long time=0;
        for(int pile:piles)
        {
            time+=ceil((double)pile/mid);
        }
        return time;
    }
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int mx=0;
        for(int pile:piles)
        {
            mx=max(pile, mx);
        }
        int lo=1;
        int hi=mx;
        int ans=0;
        while(lo<=hi)
        {
            int mid=lo+(hi-lo)/2;
            if(time(piles, mid)<=h)
            {
                ans=mid;
                hi=mid-1;
            }
            else
            {
                lo=mid+1;
            }
        }
        return ans;
    }
};
```
