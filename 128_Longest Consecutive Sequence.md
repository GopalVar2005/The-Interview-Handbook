# 128. Longest Consecutive Sequence

**LeetCode:** https://leetcode.com/problems/longest-consecutive-sequence/

**Difficulty:** Medium

---

# Pattern(s)

- HashSet
- Array
- Sequence Detection

---

# Pattern Recognition

Look for this approach when:

- You need to find consecutive elements.
- Order of the input array does not matter.
- Fast lookup is required.
- You need to avoid sorting to achieve linear time.

---

# Intuition

We need to find the length of the longest sequence of consecutive numbers.

The brute force approach starts from every number and keeps searching for the next consecutive number.

A better approach sorts the array and counts consecutive elements.

The optimal approach stores every element in a HashSet.

Instead of starting from every number,

we only start from numbers that are the **beginning of a sequence**.

A number is the beginning of a sequence if

```
num - 1
```

does not exist in the HashSet.

This avoids counting the same sequence multiple times.

---

# Brute Force

## Idea

For every element,

keep checking whether the next consecutive number exists in the array.

Update the maximum sequence length.

## Algorithm

1. Traverse every element.
2. Start with the current number.
3. Repeatedly search for the next consecutive number.
4. Count the sequence length.
5. Update the answer.

### Java Code

```java
class Solution {

    private boolean exists(int[] nums, int target) {

        for(int num : nums) {

            if(num == target) {
                return true;
            }

        }

        return false;
    }

    public int longestConsecutive(int[] nums) {

        int ans = 0;

        for(int num : nums) {

            int curr = num;
            int len = 1;

            while(exists(nums, curr + 1)) {

                curr++;
                len++;

            }

            ans = Math.max(ans, len);

        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n²)**

- For every element, we may scan the entire array searching for the next number.

**Space Complexity:** **O(1)**

- No extra data structure is used.

---

# Better Approach

## Idea

Sort the array first.

Traverse the sorted array.

Whenever consecutive elements differ by `1`, increase the current streak.

Ignore duplicates.

Whenever the sequence breaks,

start counting again.

## Algorithm

1. Sort the array.
2. Traverse the array.
3. Ignore duplicates.
4. If consecutive, increase the streak.
5. Otherwise, reset the streak.
6. Update the maximum streak.

### Java Code

```java
class Solution {

    public int longestConsecutive(int[] nums) {

        if(nums.length == 0) {
            return 0;
        }

        Arrays.sort(nums);

        int longest = 1;
        int current = 1;

        for(int i = 1; i < nums.length; i++) {

            if(nums[i] == nums[i - 1]) {

                continue;

            }
            else if(nums[i] == nums[i - 1] + 1) {

                current++;

            }
            else {

                longest = Math.max(longest, current);
                current = 1;

            }

        }

        longest = Math.max(longest, current);

        return longest;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n log n)**

- Sorting takes `O(n log n)`.
- Traversal takes `O(n)`.

Overall,

```
O(n log n)
```

**Space Complexity:** **O(1)**

*(Ignoring the space used by the sorting algorithm.)*

---

# Optimal Approach

## Idea

Store all elements in a HashSet.

Now,

only start counting from numbers that are the beginning of a sequence.

A number is the beginning if

```
num - 1
```

is **not** present in the HashSet.

Then keep checking

```
num + 1

num + 2

num + 3
```

until the sequence ends.

Since every number is visited only once,

the overall complexity becomes linear.

## Algorithm

1. Insert all elements into a HashSet.
2. Traverse every number.
3. If `num - 1` exists,
   - Skip this number.
4. Otherwise,
   - Start counting the sequence.
5. Keep checking the next consecutive number.
6. Update the maximum length.

### Java Code

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> set=new HashSet<>();
        for(int num:nums)
        {
            set.add(num);
        }
        int max=0;
        for(int num:nums)
        {
            if(!set.contains(num-1))
            {
                int count=0;
                while(set.contains(num))
                {
                    set.remove(num);
                    count++;
                    num++;
                }
                max=Math.max(max, count);
            }
        }
        return max;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Inserting into HashSet: **O(n)**
- Every number is processed at most once while expanding sequences.

Overall,

```
O(n)
```

**Space Complexity:** **O(n)**

- HashSet stores all elements.

C++
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st(nums.begin(), nums.end());
        int ans=0;
        for(int num:nums)
        {
            if(st.find(num-1)==st.end())
            {
                int cur=0;
                while(st.find(num)!=st.end())
                {
                    st.erase(num);
                    cur++;
                    num++;
                }
                ans=max(ans, cur);
            }
        }
        return ans;
    }
};
```

  C++
  ```cpp

  ```
