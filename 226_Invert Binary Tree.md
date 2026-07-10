# 226. Invert Binary Tree

**LeetCode:** https://leetcode.com/problems/invert-binary-tree/

**Difficulty:** Easy

---

# Pattern(s)

- Tree
- DFS
- Recursion
- BFS

---

# Pattern Recognition

Look for this approach when:

- You need to modify every node of a tree.
- The operation performed at every node is identical.
- The answer depends on traversing the entire tree.

---

# Intuition

To invert a binary tree,

every node simply swaps its left and right child.

For every node:

```
Before

      4
     / \
    2   7

After

      4
     / \
    7   2
```

We repeat this operation for every node in the tree.

---

# Brute Force

There is no meaningful brute-force approach.

The standard interview solutions are DFS and BFS.

---

# Better Approach

There is no meaningful intermediate approach.

Both DFS and BFS solve the problem optimally.

---

# Optimal Approach 1 (Recursive DFS)

## Idea

Visit every node.

Swap its left and right child.

Then recursively invert both subtrees.

## Algorithm

1. If the current node is `null`, return.
2. Swap the left and right child.
3. Recursively invert the left subtree.
4. Recursively invert the right subtree.
5. Return the root.

### Java Code

```java
class Solution {

    public TreeNode invertTree(TreeNode root) {

        if(root == null) {
            return null;
        }

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited exactly once.

**Space Complexity:** **O(h)**

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

# Optimal Approach 2 (BFS)

## Idea

Traverse the tree level by level.

For every node:

- Swap its children.
- Push both children into the queue.

Continue until every node has been processed.

## Algorithm

1. Push the root into the queue.
2. While the queue is not empty:
   - Remove one node.
   - Swap its children.
   - Push non-null children.
3. Return the root.

### Java Code

```java
class Solution {

    public TreeNode invertTree(TreeNode root) {

        if(root == null) {
            return null;
        }

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while(!queue.isEmpty()) {

            TreeNode node = queue.poll();

            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;

            if(node.left != null) {
                queue.offer(node.left);
            }

            if(node.right != null) {
                queue.offer(node.right);
            }

        }

        return root;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited exactly once.

**Space Complexity:** **O(n)**

- In the worst case, the queue may contain all nodes at the widest level.

C++
```cpp
class Solution {
    void rec(TreeNode* root)
    {
        if(root==nullptr)
        {
            return;
        }
        TreeNode* temp=root->right;
        root->right=root->left;
        root->left=temp;
        rec(root->left);
        rec(root->right);
    }
public:
    TreeNode* invertTree(TreeNode* root) {
        rec(root);
        return root;
    }
};
OR
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root==nullptr)
        {
            return root;
        }
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            TreeNode* rv=q.front();
            q.pop();
            TreeNode* temp=rv->right;
            rv->right=rv->left;
            rv->left=temp;
            if(rv->left!=nullptr)
            {
                q.push(rv->left);
            }
            if(rv->right!=nullptr)
            {
                q.push(rv->right);
            }
        }
        return root;
    }
};
```
