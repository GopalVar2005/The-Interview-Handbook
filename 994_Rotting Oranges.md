# 994. Rotting Oranges

**LeetCode:** https://leetcode.com/problems/rotting-oranges/

**Difficulty:** Medium

---

# Pattern(s)

- Graph
- BFS
- Multi-Source BFS
- Matrix Traversal

---

# Pattern Recognition

Look for this approach when:

- The input is a 2D grid.
- Something spreads from multiple starting points.
- You need the minimum time, minimum distance, or minimum steps.
- Changes happen level by level.

---

# Intuition

Every rotten orange spreads its rot to adjacent fresh oranges every minute.

Instead of running BFS from every rotten orange separately, place **all rotten oranges into the queue initially**.

Now, every BFS level represents **one minute**.

- Minute 0 → Initially rotten oranges.
- Minute 1 → Newly rotten oranges.
- Minute 2 → Oranges rotted by minute 1 oranges.
- ...

This is called **Multi-Source BFS** because BFS starts simultaneously from multiple sources.

---

# Brute Force

## Idea

Simulate the process minute by minute.

In every minute:

- Traverse the entire grid.
- Any fresh orange adjacent to a rotten orange becomes rotten in the next minute.
- Repeat until no more oranges can rot.

## Algorithm

1. Traverse the entire grid.
2. Mark oranges that will rot in the current minute.
3. Traverse again and update them.
4. Repeat until no changes occur.
5. Check whether fresh oranges still remain.

### Complexity Analysis

**Time Complexity:** **O((m × n)²)**

- In the worst case, every minute scans the entire grid.

**Space Complexity:** **O(1)**

- Ignoring the grid modifications.

---

# Better Approach

There is no meaningful intermediate approach.

The optimization directly moves to **Multi-Source BFS**.

---

# Optimal Approach (Multi-Source BFS)

## Idea

Initially, every rotten orange is already spreading the infection.

So instead of starting BFS from one orange,

start BFS from **all rotten oranges together**.

Each BFS level corresponds to **one minute**.

Whenever a fresh orange becomes rotten,

- Add it to the queue.
- Decrease the fresh orange count.

At the end,

- If fresh oranges remain → return `-1`.
- Otherwise, return the total minutes.

## Algorithm

1. Traverse the grid.
2. Add every rotten orange to the queue.
3. Count fresh oranges.
4. Perform BFS level by level.
5. Rot adjacent fresh oranges.
6. Reduce the fresh count.
7. Increment the minute after every level.
8. If fresh oranges remain, return `-1`.
9. Otherwise, return the minutes.

### Java Code

```java
class Solution {

    public int orangesRotting(int[][] grid) {

        int rows = grid.length;
        int cols = grid[0].length;

        Queue<int[]> queue = new LinkedList<>();

        int fresh = 0;

        for(int i = 0; i < rows; i++) {

            for(int j = 0; j < cols; j++) {

                if(grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                }

                else if(grid[i][j] == 1) {
                    fresh++;
                }

            }

        }

        if(fresh == 0) {
            return 0;
        }

        int[][] dir = {
                {-1,0},
                {1,0},
                {0,-1},
                {0,1}
        };

        int minutes = 0;

        while(!queue.isEmpty()) {

            int size = queue.size();

            boolean rotten = false;

            while(size-- > 0) {

                int[] curr = queue.poll();

                for(int[] d : dir) {

                    int nr = curr[0] + d[0];
                    int nc = curr[1] + d[1];

                    if(nr >= 0 &&
                       nc >= 0 &&
                       nr < rows &&
                       nc < cols &&
                       grid[nr][nc] == 1) {

                        grid[nr][nc] = 2;

                        fresh--;

                        queue.offer(new int[]{nr, nc});

                        rotten = true;

                    }

                }

            }

            if(rotten) {
                minutes++;
            }

        }

        return fresh == 0 ? minutes : -1;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(m × n)**

- Every orange enters the queue at most once.

**Space Complexity:** **O(m × n)**

- In the worst case, the queue may contain all oranges.

  C++
  ```cpp
  class Solution {
  public:
      int orangesRotting(vector<vector<int>>& grid) {
          int m=grid.size(), n=grid[0].size();
          queue<vector<int>> q;
          int fresh=0;
          for(int i=0;i<m;i++)
          {
              for(int j=0;j<n;j++)
              {
                  if(grid[i][j]==2)
                  {
                      q.push(vector<int>({i, j}));
                  }
                  else if(grid[i][j]==1)
                  {
                      fresh++;
                  }
              }
          }
          vector<vector<int>> dir({{1, 0}, {-1, 0}, {0, 1}, {0, -1}});
          int minutes=0;
          while(!q.empty() && fresh>0)
          {
              int size=q.size();
              for(int i=0;i<size;i++)
              {
                  vector<int> rv=q.front();
                  q.pop();
                  int j=0;
                  while(j<4)
                  {
                      int newr=rv[0]+dir[j][0];
                      int newc=rv[1]+dir[j][1];
                      if(newr>=0 && newc>=0 && newr<m && newc<n && grid[newr][newc]==1)
                      {
                          grid[newr][newc]=2;
                          fresh--;
                          q.push(vector<int>({newr, newc}));
                      }
                      j++;
                  }
              }
              minutes++;
          }
          if(fresh>0)
          {
              return -1;
          }
          return minutes;
      }
  };
  ```
