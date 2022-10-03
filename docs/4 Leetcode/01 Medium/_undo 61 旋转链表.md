---
Author: sync
date: 2022-06-13 16:08 Monday
tag: undo 
---

#链表 #迭代 #快慢指针

倒数第k个节点

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220605193615.png)

# 快慢指针错了

没有考虑到只有一个节点的情况，没有考虑到只有两个节点的情况

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateAtk(ListNode* head, int k){
        if(k == 0) return head;
        auto fast = head, slow = head;

        while(k--){
            fast = fast->next->next;
            slow =slow->next;
        }

        while(fast->next){
            fast = fast->next;
            slow = slow->next;
        }

        auto res = slow->next;
        slow->next = nullptr;
        fast->next = head;
        return res;
    }
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == nullptr) return head;
        int len = 0;
        auto cur = head;
        while(cur){
            cur = cur->next;
            len++;
        }
        return rotateAtk(head, k%len);
    }
};
```

# 简单的迭代即可 标准答案

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (k == 0 || head == nullptr || head->next == nullptr) {
            return head;
        }
        int n = 1;
        ListNode* iter = head;
        while (iter->next != nullptr) {
            iter = iter->next;
            n++;
        }
        int add = n - k % n;
        if (add == n) {
            return head;
        }
        iter->next = head;
        while (add--) {
            iter = iter->next;
        }
        ListNode* ret = iter->next;
        iter->next = nullptr;
        return ret;
    }
};

```

# 6月19日追加 通过

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:

    ListNode* rotateAtK(ListNode* head, int k, int len){
        ListNode *dummy = new ListNode(0);

        dummy->next = head;
        auto fast = dummy, slow = dummy;
        
        while(k--){
            fast = fast->next;
        }

        while(fast->next){
            slow = slow->next;
            fast =fast->next;
        }

        
        ListNode* firstHead = dummy->next;
        ListNode* firstTail = slow;
        ListNode* secondHead = slow->next;
        ListNode* secondTail = fast;

        dummy->next = secondHead;
        secondTail->next = firstHead;
        firstTail->next = nullptr;
        
        return dummy->next;
    }

    ListNode* rotateRight(ListNode* head, int k) {
        if(k == 0 || head == nullptr || head->next == nullptr) return head;

        int len = 0;
        ListNode* cur = head;
        while(cur){
            cur = cur->next;
            len++;
        }

        k = k % len;

        if(k == 0) return head;

        head = rotateAtK(head, k, len);


        return head;
    }
};
```