# 1. Two Sum

**LeetCode:** https://leetcode.com/problems/two-sum/

**Difficulty:** Easy

---

# Pattern

- HashMap
- Complement Lookup

---

# Intuition

The brute force approach checks every possible pair of elements to find the target sum. However, this leads to many unnecessary comparisons.

Observe that for every element `nums[i]`, we only need one specific value to reach the target.

```
complement = target - nums[i]
```

If we can quickly determine whether this complement has already appeared, we can find the answer immediately.

A **HashMap** stores each visited number along with its index, allowing constant-time lookup and reducing the time complexity from **O(n²)** to **O(n)**.

---

# Brute Force

## Idea

Check every possible pair of elements. If their sum equals the target, return their indices.

## Algorithm

1. Traverse every element.
2. For each element, check every remaining element.
3. If their sum equals the target, return the indices.

### Java Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        for(int i = 0; i < nums.length; i++) {
            for(int j = i + 1; j < nums.length; j++) {

                if(nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }

            }
        }

        return new int[]{};
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- The outer loop runs `n` times.
- For each element, the inner loop checks the remaining elements.
- In the worst case, approximately `n × n` comparisons are performed.

**Space Complexity:** **O(1)**

- No extra data structure is used.
- Only a few variables are required.

---

# Better Approach

There is no meaningful intermediate approach for this problem.

The optimization directly jumps from the brute force solution to the optimal HashMap solution.

---

# Optimal Approach

## Idea

For every element, calculate its complement.

```
complement = target - nums[i]
```

If the complement already exists in the HashMap, we have found the required pair.

Otherwise, store the current element and its index in the HashMap.

Since each element is processed only once and HashMap lookup takes **O(1)** on average, the overall time complexity becomes **O(n)**.

## Algorithm

1. Create an empty HashMap.
2. Traverse the array.
3. Calculate the complement.
4. If the complement exists in the map, return its stored index and the current index.
5. Otherwise, store the current element and its index.
6. Continue until the answer is found.

### Java Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for(int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if(map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- The array is traversed only once.
- `containsKey()`, `get()`, and `put()` operations in a HashMap take **O(1)** on average.
- Therefore, the total time complexity is **O(n)**.

**Space Complexity:** **O(n)**

- In the worst case, every element is stored in the HashMap.
- Hence, the extra space required is proportional to the size of the array.

C++
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for(int i=0;i<nums.size();i++)
        {
            int comp=target-nums[i];
            if(map.find(comp)!=map.end())
            {
                return vector<int>({map[comp], i});
            }
            map[nums[i]]=i;
        }
        return vector<int>();
    }
};
```
