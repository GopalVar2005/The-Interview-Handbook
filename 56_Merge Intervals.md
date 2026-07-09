# 56. Merge Intervals

**LeetCode:** https://leetcode.com/problems/merge-intervals/

**Difficulty:** Medium

---

# Pattern(s)

- Intervals
- Sorting
- Greedy

---

# Pattern Recognition

Look for this approach when:

- The input contains intervals.
- You need to merge overlapping intervals.
- The intervals are not sorted.
- The problem asks to combine overlapping ranges into one.

---

# Intuition

Two intervals overlap if

```
currentStart <= previousEnd
```

For example,

```
[1,3]
[2,6]
```

Since

```
2 <= 3
```

they overlap and become

```
[1,6]
```

The brute force approach repeatedly checks every interval with every other interval until no further merges are possible.

The optimal approach first sorts the intervals by their starting point. After sorting, all overlapping intervals become adjacent, allowing them to be merged in a single traversal.

---

# Brute Force

## Idea

For every interval:

- Try merging it with every other interval.
- Whenever a merge happens, the interval expands.
- Since the expanded interval may now overlap with previously skipped intervals, repeat the scan until no more merges are possible.

Finally, add the completely merged interval to the answer.

## Algorithm

1. Create a visited array.
2. Traverse every interval.
3. Skip already merged intervals.
4. Try merging with every remaining interval.
5. If the interval expands, scan again.
6. Continue until no more merges are possible.
7. Add the final merged interval to the answer.

### Java Code

```java
class Solution {
    public int[][] merge(int[][] intervals) {

        boolean[] visited = new boolean[intervals.length];

        List<int[]> result = new ArrayList<>();

        for(int i = 0; i < intervals.length; i++) {

            if(visited[i]) {
                continue;
            }

            int start = intervals[i][0];
            int end = intervals[i][1];

            boolean merged;

            do {

                merged = false;

                for(int j = i + 1; j < intervals.length; j++) {

                    if(visited[j]) {
                        continue;
                    }

                    if(!(intervals[j][1] < start ||
                         intervals[j][0] > end)) {

                        start = Math.min(start, intervals[j][0]);
                        end = Math.max(end, intervals[j][1]);

                        visited[j] = true;
                        merged = true;
                    }

                }

            } while(merged);

            result.add(new int[]{start, end});

        }

        return result.toArray(new int[result.size()][]);
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n³)**

- Outer loop traverses all intervals.
- Inner loop compares with remaining intervals.
- Multiple scans may be required whenever the interval expands.

**Space Complexity:** **O(n)**

- Visited array and result list.

---

# Better Approach

There is no meaningful intermediate approach for this problem.

The optimization directly moves from repeated scanning to **Sorting + Greedy**.

---

# Optimal Approach

## Idea

Sort all intervals by their starting point.

After sorting, every interval that can be merged appears next to the previous merged interval.

For every interval:

- If it overlaps with the last merged interval, extend the ending point.
- Otherwise, start a new merged interval.

Since the intervals are sorted, we never need to look back.

## Algorithm

1. Sort the intervals by their starting point.
2. Add the first interval to the answer.
3. Traverse the remaining intervals.
4. If overlap exists:
   - Extend the previous interval.
5. Otherwise:
   - Add the current interval.
6. Return the answer.

### Java Code

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> ll=new ArrayList<>();
        Arrays.sort(intervals, (a, b)->{
            if(a[0]==b[0])
            {
                return a[1]-b[1];
            }
            return a[0]-b[0];
        });
        int first=intervals[0][0];
        int last=intervals[0][1];
        for(int i=1;i<intervals.length;i++)
        {
            if(last>=intervals[i][0])
            {
                last=Math.max(last, intervals[i][1]);
            }
            else
            {
                ll.add(new int[]{first, last});
                first=intervals[i][0];
                last=intervals[i][1];
            }
        }
        ll.add(new int[]{first, last});
        int[][] ans=new int[ll.size()][2];
        for(int i=0;i<ll.size();i++)
        {
            ans[i][0]=ll.get(i)[0];
            ans[i][1]=ll.get(i)[1];
        }
        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n log n)**

- Sorting takes **O(n log n)**.
- One traversal takes **O(n)**.

Overall,

```
O(n log n)
```

**Space Complexity:** **O(n)**

- Result list stores the merged intervals.

  C++
  ```cpp
  class Solution {
  public:
      vector<vector<int>> merge(vector<vector<int>>& intervals) {
          sort(intervals.begin(), intervals.end(), [](auto& a, auto& b){
              if(a[0]==b[0])
              {
                  return a[1]<b[1];
              }
              return a[0]<b[0];
          });
          vector<vector<int>> ans;
          int start=intervals[0][0], end=intervals[0][1];
          for(int i=1;i<intervals.size();i++)
          {
              if(intervals[i][0]<=end)
              {
                  end=max(end, intervals[i][1]);
              }
              else
              {
                  ans.push_back(vector<int>({start, end}));
                  start=intervals[i][0];
                  end=intervals[i][1];
              }
          }
          ans.push_back(vector<int>({start, end}));
          return ans;
      }
  };
  ```
