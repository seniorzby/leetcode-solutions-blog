LeetCode 142: Linked List Cycle II
Problem Description
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

A cycle in a linked list exists if there is some node in the list that can be reached again by continuously following the next pointer. The pos parameter is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is not passed as a parameter to the function.

Initial Approach and Challenges
My initial solution attempt:

java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow.val;
    }
}
This initial attempt had several issues:

The loop condition (slow != fast) would fail on the first iteration since both pointers start at the same position
No null checks for fast and fast.next, which could lead to NullPointerException
Returning slow.val instead of the node itself
The algorithm only detected a cycle but did not find the entry point of the cycle
Improved Algorithm: Floyd's Tortoise and Hare
The correct approach uses Floyd's Cycle-Finding Algorithm (also known as the "Tortoise and Hare" algorithm), which has two phases:

Phase 1: Detect if a cycle exists
Use two pointers moving at different speeds
Slow pointer moves one step at a time
Fast pointer moves two steps at a time
If they meet, a cycle exists; otherwise, the fast pointer will reach the end
Phase 2: Find the cycle entrance
Reset one pointer to the head of the list
Move both pointers one step at a time
The point where they meet is the entrance of the cycle
Step-by-Step Solution
java
public ListNode detectCycle(ListNode head) {
    if (head == null || head.next == null) {
        return null; // No cycle possible with 0 or 1 nodes
    }
    
    // Phase 1: Detect cycle
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;           // Move slow one step
        fast = fast.next.next;      // Move fast two steps
        
        if (slow == fast) {         // Cycle detected
            // Phase 2: Find cycle entrance
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;            // Return cycle entrance
        }
    }
    
    return null;  // No cycle
}
Mathematical Proof
The mathematical proof behind this algorithm explains why the meeting point in Phase 2 is the cycle entrance:

Let's define:

L1 = Distance from head to cycle entrance
L2 = Distance from cycle entrance to meeting point
C = Total length of the cycle
When slow and fast pointers meet:

Slow pointer has traveled: L1 + L2
Fast pointer has traveled: L1 + L2 + n×C (where n is some integer)
Since the fast pointer moves twice as fast as the slow pointer:

Distance traveled by fast = 2 × Distance traveled by slow
L1 + L2 + n×C = 2(L1 + L2)
Simplifying: L1 = n×C - L2
This tells us: The distance from the head to the cycle entrance (L1) equals the distance from the meeting point to the cycle entrance moving forward (n×C - L2).

Since n×C represents complete cycles (returning to the same point), we only need to move (C-L2) steps from the meeting point to reach the cycle entrance. Therefore, if we reset one pointer to the head and move both pointers one step at a time, they will meet at the cycle entrance.

Example Tracing
For a linked list 1->2->3->4->5->3 (with a cycle from 5 back to 3):

Initial state: Both slow and fast at node 1
After iterations:
Slow moves to node 4
Fast moves to node 4 (after moving through the cycle)
They meet at node 4
Phase 2:
Reset slow to head (node 1)
Keep fast at node 4
Move both one step at a time
They meet at node 3, which is the cycle entrance
Common Issues and Pitfalls
Incorrect loop condition: Starting both pointers at head requires checking if they're equal after movement, not before
Missing null checks: Always check if fast and fast.next are null before accessing fast.next.next
Moving pointers outside the loop: Pointer movement must be inside the loop to correctly iterate
Returning incorrect value: Return the node itself, not its value
Mathematical understanding: Understanding why the algorithm works helps implement it correctly
Key Takeaways
Floyd's Cycle Detection algorithm efficiently finds cycles using O(1) space
The algorithm has two distinct phases: cycle detection and finding the cycle entrance
Proper null checking is essential to avoid runtime exceptions
The mathematical relationship between distances is what makes the algorithm work
This problem demonstrates how a seemingly complex task can be solved with a elegant algorithm
