 
 
 
 
```markdown 
LeetCode 142: Linked List Cycle II Solution 
 
▎Problem Description 
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.  
- Cycle Definition: A node that can be reached again by continuously following the `next` pointer.  
- Special Parameter: The `pos` in the problem description is only used to define the linked list structure and is not passed as a function parameter.
 
---
 
▎Analysis of Initial Attempt Issues 
```java 
// Problematic Code Snippet 
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
```
 
❌ Key Issues 
1. Initial Condition Error: `slow` and `fast` start at the same position, causing the loop to never execute.
2. Null Pointer Risk: No null checks for `fast.next`, leading to potential `NullPointerException`.
3. Incorrect Return Value: Returns node value (`slow.val`) instead of the node itself.
4. Logical Flaw: Detects meeting point but fails to locate the cycle entrance.
 
---
 
▎Improved Algorithm: Floyd's Tortoise and Hare 
 
Algorithm Framework 
| Phase | Operation | Time Complexity | Space Complexity |
|-------|-----------|-----------------|------------------|
| 1. Cycle Detection | Find meeting point with slow/fast pointers | O(n) | O(1) |
| 2. Entrance Localization | Reset slow pointer and synchronize movement | O(n) | O(1) |
 
---
 
▶ Step-by-Step Implementation 
```java 
public ListNode detectCycle(ListNode head) {
    if (head == null || head.next == null) return null;
 
    // Phase 1: Detect cycle 
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) break; // Meeting point found 
    }
    
    // Check if there's no cycle 
    if (fast == null || fast.next == null) return null;
 
    // Phase 2: Locate entrance 
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow; // Cycle entrance node 
}
```
 
---
 
▎Mathematical Proof 
Define:  
- L1: Distance from head to cycle entrance  
- L2: Distance from cycle entrance to meeting point  
- C: Total length of the cycle  
 
When slow and fast pointers meet:  
- Distance traveled by slow = `L1 + L2`  
- Distance traveled by fast = `L1 + L2 + n*C` (where n ≥ 1, number of full cycles)  
 
Since fast moves twice as fast as slow:  
`2(L1 + L2) = L1 + L2 + n*C`  
→ `L1 = n*C - L2`  
 
Conclusion: After resetting slow to the head, both pointers will meet at the cycle entrance after moving `L1` steps.
 
---
 
▎Example Walkthrough 
For the linked list `1→2→3→4→5→3` (cycle from 5 back to 3):
 
Phase 1: Cycle Detection 
| Step | slow Position | fast Position | Notes                      |
|------|---------------|---------------|----------------------------|
| Init | 1             | 1             |                            |
| 1    | 2             | 3             |                            |
| 2    | 3             | 5             |                            |
| 3    | 4             | 4             | Meeting point detected |
 
Phase 2: Entrance Localization 
| Step | slow Position | fast Position | Notes                      |
|------|---------------|---------------|----------------------------|
| Reset| 1             | 4             |                            |
| 1    | 2             | 5             |                            |
| 2    | 3             | 3             | Cycle entrance found   |
 
---
 
▎Common Pitfalls & Key Takeaways 
 
⚠️ Common Errors 
1. Incorrect Loop Condition: Check meeting *after* moving pointers (initial `slow == fast` prevents loop entry).
2. Null Pointer Exceptions: Always check `fast != null && fast.next != null`.
3. Return Value Mistake: Return the node itself (`ListNode`), not its value.
4. Edge Cases: Handle single/double-node non-cyclic lists.
 
✅ Key Insights 
1. Two-Phase Logic: Detect cycle first, then locate entrance.
2. Mathematical Relationship: `L1 = nC - L2` is the algorithm's foundation.
3. Synchronized Movement: Both pointers must move at the same pace in Phase 2.
4. Optimal Efficiency: O(n) time and O(1) space complexity.
```
