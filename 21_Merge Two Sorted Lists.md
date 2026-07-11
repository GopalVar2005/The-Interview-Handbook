# 21. Merge Two Sorted Lists

**LeetCode:** https://leetcode.com/problems/merge-two-sorted-lists/

**Difficulty:** Easy

---

# Pattern(s)

- Linked List
- Two Pointers
- Recursion

---

# Pattern Recognition

Look for this approach when:

- Two sorted linked lists are given.
- You need to merge them while maintaining sorted order.
- You're comparing the current node of two lists.

---

# Intuition

Since both linked lists are already sorted,

we only need to compare their current nodes.

- The smaller node should come first.
- Move the pointer of the list from which the node was taken.
- Repeat until one list becomes empty.
- Attach the remaining list.

The brute force creates a new list.

The optimal approach reuses the existing nodes.

---

# Brute Force

## Idea

Traverse both linked lists.

Store all values in an ArrayList.

Sort the ArrayList.

Create a new linked list using the sorted values.

## Algorithm

1. Traverse the first list and store all values.
2. Traverse the second list and store all values.
3. Sort the ArrayList.
4. Create a new linked list.
5. Return the new list.

### Java Code

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        ArrayList<Integer> list = new ArrayList<>();

        while(list1 != null) {
            list.add(list1.val);
            list1 = list1.next;
        }

        while(list2 != null) {
            list.add(list2.val);
            list2 = list2.next;
        }

        Collections.sort(list);

        ListNode dummy = new ListNode(-1);
        ListNode curr = dummy;

        for(int val : list) {
            curr.next = new ListNode(val);
            curr = curr.next;
        }

        return dummy.next;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O((m + n) log(m + n))**

- Traversing both lists: `O(m + n)`
- Sorting: `O((m + n) log(m + n))`

**Space Complexity:** **O(m + n)**

- ArrayList stores all elements.
- New linked list is created.

---

# Better Approach

There is no meaningful intermediate approach.

The optimization directly merges the two lists in one traversal.

---

# Optimal Approach (Iterative)

## Idea

Create a dummy node.

Compare the current nodes of both lists.

Attach the smaller node to the merged list.

Move the corresponding pointer.

When one list finishes,

attach the remaining list directly.

## Algorithm

1. Create a dummy node.
2. Maintain a current pointer.
3. Compare the heads of both lists.
4. Attach the smaller node.
5. Move the pointer of that list.
6. Repeat until one list becomes empty.
7. Attach the remaining list.
8. Return `dummy.next`.

### Java Code

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        ListNode dummy = new ListNode(-1);
        ListNode curr = dummy;

        while(list1 != null && list2 != null) {

            if(list1.val <= list2.val) {

                curr.next = list1;
                list1 = list1.next;

            }
            else {

                curr.next = list2;
                list2 = list2.next;

            }

            curr = curr.next;

        }

        if(list1 != null) {
            curr.next = list1;
        }
        else {
            curr.next = list2;
        }

        return dummy.next;
    }
}
```

### Complexity Analysis

**Time Complexity:** **O(m + n)**

- Every node is visited exactly once.

**Space Complexity:** **O(1)**

- No extra data structure is used.
- Existing nodes are reused.

---

# Optimal Approach (Recursion)

## Idea

Compare the first nodes of both lists.

The smaller node becomes part of the answer.

Recursively merge the remaining lists.

The recursion automatically builds the merged list.

## Algorithm

1. If one list is empty, return the other.
2. Compare the first nodes.
3. Attach the smaller node.
4. Recursively merge the remaining nodes.
5. Return the selected node.

### Java Code

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        if(list1 == null) {
            return list2;
        }

        if(list2 == null) {
            return list1;
        }

        if(list1.val <= list2.val) {

            list1.next = mergeTwoLists(list1.next, list2);

            return list1;

        }

        list2.next = mergeTwoLists(list1, list2.next);

        return list2;

    }
}
```

### Complexity Analysis

**Time Complexity:** **O(m + n)**

- Every node is processed exactly once.

**Space Complexity:** **O(m + n)**

- Due to the recursion stack in the worst case.
- No additional data structures are used.

  C++
  ```cpp
  class Solution {
  public:
      ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
          ListNode* dummy=new ListNode();
          ListNode* temp=dummy;
          while(list1!=nullptr && list2!=nullptr)
          {
              if(list1->val<list2->val)
              {
                  temp->next=list1;
                  list1=list1->next;
              }
              else
              {
                  temp->next=list2;
                  list2=list2->next;
              }
              temp=temp->next;
          }
          if(list1!=nullptr)
          {
              temp->next=list1;
          }
          else if(list2!=nullptr)
          {
              temp->next=list2;
          }
          return dummy->next;
      }
  };
  ```
