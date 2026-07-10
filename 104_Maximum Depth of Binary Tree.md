# 104. Maximum Depth of Binary Tree

**LeetCode:** https://leetcode.com/problems/maximum-depth-of-binary-tree/

**Difficulty:** Easy

---

# Pattern(s)

- Tree
- DFS
- Recursion
- BFS (Level Order Traversal)

---

# Pattern Recognition

Look for this approach when:

- You need the height/depth of a tree.
- The answer depends on both left and right subtrees.
- The problem asks for the longest path from the root to a leaf.

---

# Intuition

The depth of a tree is:

> **1 + maximum depth of its left and right subtree**

Every node asks the same question:

> "What is the maximum depth of my left subtree?"

and

> "What is the maximum depth of my right subtree?"

Whichever is larger becomes the answer.

---

# Brute Force

There is no meaningful brute-force approach for this problem.

The natural solution is recursive DFS, which is already optimal.

---

# Better Approach

There is no meaningful intermediate approach.

The two standard interview solutions are:

- Recursive DFS
- Iterative BFS

Both visit every node exactly once.

---

# Optimal Approach 1 (Recursive DFS)

## Idea

For every node:

- Find the depth of the left subtree.
- Find the depth of the right subtree.
- Return

```
1 + max(leftDepth, rightDepth)
```

The recursion automatically computes the height from bottom to top.

## Algorithm

1. If the node is `null`, return `0`.
2. Recursively compute the left subtree depth.
3. Recursively compute the right subtree depth.
4. Return `1 + max(left, right)`.

### Java Code

```java
class Solution {

    public int maxDepth(TreeNode root) {

        if(root == null) {
            return 0;
        }

        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return 1 + Math.max(left, right);

    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited exactly once.

**Space Complexity:** **O(h)**

- `h` is the height of the tree.
- Due to the recursion stack.

Worst case (skewed tree):

```
O(n)
```

Balanced tree:

```
O(log n)
```

---

# Optimal Approach 2 (BFS / Level Order Traversal)

## Idea

Every BFS level represents one depth level.

Traverse the tree level by level.

After finishing one level,

increase the depth.

## Algorithm

1. Push the root into a queue.
2. While the queue is not empty:
   - Process one complete level.
   - Increase depth.
3. Return the depth.

### Java Code

```java
class Solution {

    public int maxDepth(TreeNode root) {

        if(root == null) {
            return 0;
        }

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        int depth = 0;

        while(!queue.isEmpty()) {

            int size = queue.size();

            while(size-- > 0) {

                TreeNode node = queue.poll();

                if(node.left != null) {
                    queue.offer(node.left);
                }

                if(node.right != null) {
                    queue.offer(node.right);
                }

            }

            depth++;

        }

        return depth;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node enters and leaves the queue exactly once.

**Space Complexity:** **O(n)**

- In the worst case, the queue may contain all nodes of the widest level.

C++
```cpp
class Solution {
    int ht(TreeNode* root)
    {
        if(root==nullptr)
        {
            return 0;
        }
        int left=ht(root->left);
        int right=ht(root->right);
        return max(left, right)+1;
    }
public:
    int maxDepth(TreeNode* root) {
        return ht(root);
    }
};
OR
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr)
        {
            return 0;
        }
        queue<TreeNode*> q;
        q.push(root);
        int ht=0;
        while(!q.empty())
        {
            int size=q.size();
            for(int i=0;i<size;i++)
            {
                TreeNode* rv=q.front();
                q.pop();
                if(rv->left!=nullptr)
                {
                    q.push(rv->left);
                }
                if(rv->right!=nullptr)
                {
                    q.push(rv->right);
                }
            }
            ht++;
        }
        return ht;
    }
};
```
