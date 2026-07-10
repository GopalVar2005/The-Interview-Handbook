# 102. Binary Tree Level Order Traversal

**LeetCode:** https://leetcode.com/problems/binary-tree-level-order-traversal/

**Difficulty:** Medium

---

# Pattern(s)

- Tree
- BFS
- Queue
- Level Order Traversal

---

# Pattern Recognition

Look for this approach when:

- The problem asks to traverse a tree level by level.
- Nodes at the same depth should be grouped together.
- You need to process one tree level before moving to the next.

---

# Intuition

Instead of going deep into one branch (DFS),

visit all nodes at the current level first.

A queue naturally supports this.

The key observation is:

> **At any moment, the queue contains exactly one level (or the remaining nodes of that level).**

So,

- Find the current queue size.
- Process exactly that many nodes.
- Their children automatically form the next level.

---

# Brute Force

There is no meaningful brute-force approach.

The standard interview solution is BFS using a queue.

---

# Better Approach

There is no meaningful intermediate approach.

The natural solution is Level Order Traversal using BFS.

---

# Optimal Approach (BFS)

## Idea

Use a queue.

For every iteration,

- The queue size tells us how many nodes belong to the current level.
- Process exactly those nodes.
- Store their values.
- Push their children into the queue.

After processing all nodes of one level,

store the current level in the answer.

## Algorithm

1. If the tree is empty, return an empty list.
2. Push the root into the queue.
3. While the queue is not empty:
   - Store the current queue size.
   - Process exactly that many nodes.
   - Push their children.
   - Store the current level.
4. Return the answer.

### Java Code

```java
class Solution {

    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> ans = new ArrayList<>();

        if(root == null) {
            return ans;
        }

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while(!queue.isEmpty()) {

            int size = queue.size();

            List<Integer> level = new ArrayList<>();

            while(size-- > 0) {

                TreeNode node = queue.poll();

                level.add(node.val);

                if(node.left != null) {
                    queue.offer(node.left);
                }

                if(node.right != null) {
                    queue.offer(node.right);
                }

            }

            ans.add(level);

        }

        return ans;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited exactly once.

**Space Complexity:** **O(n)**

- In the worst case, the queue may contain all nodes of the widest level.

---

# Optimal Approach (DFS)

## Idea

DFS can also solve this problem.

The only difference is that we keep track of the current level.

Whenever we visit a new level for the first time,

create a new list.

Then insert the current node into its corresponding level.

## Algorithm

1. Traverse using DFS.
2. Keep track of the current level.
3. If this level doesn't exist, create it.
4. Add the node value.
5. Traverse left and right children.

### Java Code

```java
class Solution {

    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> ans = new ArrayList<>();

        dfs(root, 0, ans);

        return ans;
    }

    private void dfs(TreeNode root,
                     int level,
                     List<List<Integer>> ans) {

        if(root == null) {
            return;
        }

        if(level == ans.size()) {
            ans.add(new ArrayList<>());
        }

        ans.get(level).add(root.val);

        dfs(root.left, level + 1, ans);

        dfs(root.right, level + 1, ans);

    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited once.

**Space Complexity:** **O(h)**

- Recursion stack.
- Worst case (skewed tree): **O(n)**
- Balanced tree: **O(log n)**

*(The output list is not counted as auxiliary space.)*

C++
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(root==nullptr)
        {
            return ans;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            int size=q.size();
            vector<int> ll;
            for(int i=0;i<size;i++)
            {
                TreeNode* rv=q.front();
                q.pop();
                ll.push_back(rv->val);
                if(rv->left!=nullptr)
                {
                    q.push(rv->left);
                }
                if(rv->right!=nullptr)
                {
                    q.push(rv->right);
                }
            }
            ans.push_back(ll);
        }
        return ans;
    }
};
OR
class Solution {
    void rec(int level, TreeNode* root, vector<vector<int>>& ans)
    {
        if(root==nullptr)
        {
            return;
        }
        if(level==ans.size())
        {
            ans.push_back(vector<int>());
        }
        ans[level].push_back(root->val);
        rec(level+1, root->left, ans);
        rec(level+1, root->right, ans);
    }
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        rec(0, root, ans);
        return ans;
    }
};
```
