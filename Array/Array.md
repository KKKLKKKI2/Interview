# Practice again
- [] 206 Reverse Linked List
- [] 24 Swap Node in Pairs

# Key
1. Binary search
	1. **[Left, Right]**，左閉右閉，while(left<=right)，left = 0，right = size - 1
		- 當(target<mid) right = mid + 1
		- 當(target>mid) left = mid - 1
	
	2. **[Left, Right)**，左閉右開，while(left< right)，left = 0，right = size
		- 當(target<mid) right = mid + 1
		- 當(target>mid) left = mid - 1
	



# 206 Reverse Linked List
## must know
1. memorize
2. curr = new head


## code
```cpp{.line-numbers}
public:
    ListNode *reverseList(ListNode *head)
    {
		ListNode *tmp;
        ListNode *cur = head;
        ListNode *prev = nullptr;
        while (cur)
        {
            tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp;
        }
        return prev;
    }

```

# 24 Swap Node in Pairs

## must know






## code
```cpp
    ListNode *swapPairs(ListNode *head)
    {
        ListNode *dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode *cur = dummyHead;
        while (cur->next && cur->next->next)
        {
            ListNode *second = cur->next;
            ListNode *tmp = cur->next->next->next;

            cur->next = cur->next->next;
            cur->next->next = second;
            second->next = tmp;

            cur = cur->next->next;
        }
        return dummyHead->next;
    }
```


# 19 Remove nth Node

## Key point
Use "fast" and "slow" pointer. Fast pointer faster n node than slow pointer.


## Code 
```cpp
    ListNode *removeNthFromEnd(ListNode *head, int n)
    {
        ListNode *dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode *slow = dummyHead;
        ListNode *fast = dummyHead;
        for (int i = 0; i < n + 1; i++)
        {
            fast = fast->next;
        }
        while (fast != nullptr)
        {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode *tmp = slow->next;
        slow->next = tmp->next;
        delete tmp;

        return dummyHead->next;
    }

```

# 160 intersection-of-two-linked-lists-lcci

## Key point
Compare length of two linked-list. Move longer list N node that two list will have same length. Compare two list and return node


## Code 
```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0, lenB = 0;
        while (curA != NULL) { 
            lenA++;
            curA = curA->next;
        }
        while (curB != NULL) { 
            lenB++;
            curB = curB->next;
        }
        curA = headA;
        curB = headB;
        if (lenB > lenA) {
            swap (lenA, lenB);
            swap (curA, curB);
        }
        int diff = lenA - lenB;
        while (gap--) {
            curA = curA->next;
        }
        while (curA != NULL) {
            if (curA == curB) {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;
    }
```
