# 141. Linked List Cycle

**LeetCode:** https://leetcode.com/problems/linked-list-cycle/

**Difficulty:** Easy

---

# Pattern(s)

- Linked List
- Fast & Slow Pointers (Floyd's Cycle Detection)
- Two Pointers

---

# Pattern Recognition

Look for this approach when:

- The problem involves a linked list.
- You need to detect whether a cycle exists.
- The list cannot be modified.
- You are asked to solve it using constant extra space.

---

# Intuition

If we keep visiting nodes, there are two possibilities:

- We eventually reach `null` → No cycle.
- We visit the same node again → Cycle exists.

The brute force stores every visited node.

The optimal approach uses two pointers:

- Slow pointer moves **1 step**.
- Fast pointer moves **2 steps**.

If a cycle exists, the fast pointer will eventually catch the slow pointer.

If no cycle exists, the fast pointer reaches `null`.

---

# Brute Force

## Idea

Traverse the linked list.

Store every visited node in a HashSet.

If we ever visit the same node again,

a cycle exists.

## Algorithm

1. Create a HashSet.
2. Traverse the linked list.
3. If the current node already exists in the HashSet:
   - Return `true`.
4. Otherwise, insert it into the HashSet.
5. If traversal reaches `null`, return `false`.

### Java Code

```java
class Solution {

    public boolean hasCycle(ListNode head) {

        HashSet<ListNode> set = new HashSet<>();

        while(head != null) {

            if(set.contains(head)) {
                return true;
            }

            set.add(head);

            head = head.next;

        }

        return false;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- Every node is visited at most once.

**Space Complexity:** **O(n)**

- HashSet stores all visited nodes.

---

# Better Approach

There is no meaningful intermediate approach.

The optimization directly uses Floyd's Cycle Detection Algorithm.

---

# Optimal Approach

## Idea

Use two pointers:

- Slow pointer moves one step.
- Fast pointer moves two steps.

If there is no cycle,

the fast pointer eventually reaches `null`.

If there is a cycle,

the fast pointer moves faster than the slow pointer and will eventually meet it.

## Algorithm

1. Initialize:
   - `slow = head`
   - `fast = head`
2. While `fast` and `fast.next` are not `null`:
   - Move `slow` by one step.
   - Move `fast` by two steps.
3. If both pointers meet:
   - Return `true`.
4. If the loop ends:
   - Return `false`.

### Java Code

```java
class Solution {

    public boolean hasCycle(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;

            if(slow == fast) {
                return true;
            }

        }

        return false;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(n)**

- In the worst case, every node is visited at most once by the slow pointer.
- The fast pointer also traverses the list in linear time.

Overall,

```
O(n)
```

**Space Complexity:** **O(1)**

- Only two pointers are used.

C++
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow=head;   
        ListNode* fast=head;   
        while(fast!=nullptr && fast->next!=nullptr)
        {
            slow=slow->next;
            fast=fast->next->next;
            if(slow==fast)
            {
                return true;
            }
        }
        return false;
    }
};
```
