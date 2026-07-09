# 200. Number of Islands

**LeetCode:** https://leetcode.com/problems/number-of-islands/

**Difficulty:** Medium

---

# Pattern(s)

- Graph
- DFS
- BFS
- Matrix Traversal
- Flood Fill

---

# Pattern Recognition

Look for this approach when:

- The input is a 2D grid.
- Cells are connected vertically and horizontally.
- You need to count the number of connected components.
- Once a cell is visited, it should not be processed again.

---

# Intuition

Every island is a group of connected `'1'` cells.

If we start from any unvisited land cell (`'1'`), we can visit the entire island using **DFS** or **BFS**.

Once an island has been completely visited, it will never be counted again.

Therefore,

- Traverse the grid.
- Whenever an unvisited land cell is found,
    - Increment the island count.
    - Visit the entire island.

---

# Brute Force

There is no practical brute-force approach for this problem.

The natural solution is to traverse the grid and perform either **DFS** or **BFS** whenever an unvisited land cell is found.

This is already the optimal approach.

---

# Better Approach

There is no meaningful intermediate approach.

The optimization directly uses **Graph Traversal (DFS/BFS)**.

---

# Optimal Approach (DFS)

## Idea

Traverse every cell.

Whenever an unvisited land cell (`'1'`) is found,

- One new island is discovered.
- Perform DFS.
- Mark every connected land cell as visited.

Continue until the entire grid has been traversed.

## Algorithm

1. Traverse every cell.
2. If the current cell is water or already visited, continue.
3. Increment the island count.
4. Perform DFS in all four directions.
5. Mark every visited land cell.
6. Return the total count.

### Java Code

```java
class Solution {

    private void dfs(char[][] grid, int row, int col) {

        if(row < 0 || col < 0 ||
           row >= grid.length || col >= grid[0].length ||
           grid[row][col] == '0') {

            return;
        }

        grid[row][col] = '0';

        dfs(grid, row - 1, col);
        dfs(grid, row + 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row, col + 1);

    }

    public int numIslands(char[][] grid) {

        int islands = 0;

        for(int i = 0; i < grid.length; i++) {

            for(int j = 0; j < grid[0].length; j++) {

                if(grid[i][j] == '1') {

                    islands++;

                    dfs(grid, i, j);

                }

            }

        }

        return islands;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(m × n)**

- Every cell is visited at most once.

**Space Complexity:** **O(m × n)**

- In the worst case, the recursion stack may contain every cell.

---

# Optimal Approach (BFS)

## Idea

Instead of recursion, use a queue.

Whenever a new island is found,

- Push it into the queue.
- Visit all connected land cells level by level.
- Mark them as visited.

## Algorithm

1. Traverse the grid.
2. Whenever land is found,
    - Increment the answer.
    - Push it into the queue.
3. Process all connected land cells.
4. Repeat until the queue becomes empty.
5. Return the answer.

### Java Code

```java
class Solution {

    public int numIslands(char[][] grid) {

        int islands = 0;

        int[][] dir = {
                {-1,0},
                {1,0},
                {0,-1},
                {0,1}
        };

        for(int i = 0; i < grid.length; i++) {

            for(int j = 0; j < grid[0].length; j++) {

                if(grid[i][j] == '1') {

                    islands++;

                    Queue<int[]> queue = new LinkedList<>();

                    queue.offer(new int[]{i, j});

                    grid[i][j] = '0';

                    while(!queue.isEmpty()) {

                        int[] curr = queue.poll();

                        for(int[] d : dir) {

                            int nr = curr[0] + d[0];
                            int nc = curr[1] + d[1];

                            if(nr >= 0 &&
                               nc >= 0 &&
                               nr < grid.length &&
                               nc < grid[0].length &&
                               grid[nr][nc] == '1') {

                                grid[nr][nc] = '0';

                                queue.offer(new int[]{nr, nc});

                            }

                        }

                    }

                }

            }

        }

        return islands;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(m × n)**

- Every cell enters the queue at most once.

**Space Complexity:** **O(m × n)**

- Queue may contain all cells in the worst case.

  C++
  ```cpp
    class Solution {
    void rec(int i, int j, vector<vector<char>>& grid)
    {
        if(i<0 || j<0 || i>=grid.size() || j>=grid[0].size() || grid[i][j]=='0')
        {
            return;
        }
        grid[i][j]='0';
        rec(i+1, j, grid);
        rec(i-1, j, grid);
        rec(i, j+1, grid);
        rec(i, j-1, grid);
    }
  public:
      int numIslands(vector<vector<char>>& grid) {
          int m=grid.size(), n=grid[0].size(), count=0;
          for(int i=0;i<m;i++)
          {
              for(int j=0;j<n;j++)
              {
                  if(grid[i][j]=='1')
                  {
                      rec(i, j, grid);
                      count++;
                  }
              }
          }
          return count;
      }
  };
  ```
