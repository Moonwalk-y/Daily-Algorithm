- 第一题自己完成
- 第二题自己完成普通方法，学习栈方法
- 第三题看解析，有自己的暴力解法
- 第四题看解析

---
## 两两交换链表中的节点
题目链接/文章讲解/视频讲解： [https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html]
#### 解法
设置一个虚拟头节点，p、q分别指向头节点和虚拟头节点，循环中设置l指向p->next，这样只需要交换l和p即可，交换完使用q将这一对节点接到前面的链表上。然后移动指针，q = p，指向下一对需要交换的节点前面，然后p = q->next。以此类推
每次交换完后会有三种情况：
- 后面还有成对的节点需要交换
- 后面只剩一个节点
- 后面没有节点
第一种情况无需多管；第二种情况可以加一个判断if(l == nullptr) break; 因为只剩一个节点的话不用交换，直接跳出循环即可；第三种情况用while循环中的判断即可实现：p != nullptr。
```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* newhead = new ListNode();
        newhead->next = head;
        if(head == nullptr || head->next == nullptr) return head;
        ListNode* p = head;
        ListNode* q = newhead;
        ListNode* l;
        while(p != nullptr){
            l = p->next;if(l == nullptr) break;
            p->next = l->next;
            l->next = p;
            q->next = l;
            q = p;//指针向后移动
            p = q->next;
        }
        return newhead->next;
    }
};
```
#### 思考
if(head == nullptr || head->next == nullptr) return head;这里必须先判断head，如果先判断head->next，当head = nullptr的时候会报错。  

---
## 删除链表的倒数第N个节点
[https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html]、

#### 思路
题解中的栈方法和双指针法都很巧妙，不需要知道链表长度，只需要遍历一遍链表即可。栈方法是将链表从头节点挨个放到栈里面，然后从尾开始出栈，第n个出来的就是要删除的那个节点，而此时栈顶的节点的next连接到第n个节点的next上面就行；双指针法更巧妙，一个快指针，一个慢指针 ，快指针bi慢指针快n个节点，这样当快指针到链表尾时，慢指针刚好指向被删除的元素之前。
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* newhead = new ListNode();
        newhead->next = head;
        int num = 0;//链表长度
        ListNode* p = newhead;
        while(p->next != nullptr){
            p = p->next;
            num++;
        }
        num = num - n;//这里num变成正向的步数
        p = newhead;
        while(num != 0){
            p = p->next;
            num--;
        }
        ListNode* q = p->next;
        p->next = q->next;
        delete q;
        return newhead->next;
    }
};
```
#### 思考
各种问题在想到普通解法或者没思路时，都可以试试双指针，尤其是数组题和链表题，这里的栈用的更是巧妙，要灵活应用已经学过的知识。

---
## 链表相交
[https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html]

#### 解法
```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* p = headA;
        ListNode* q = headB;
        int lenA = 0;int lenB = 0;
        while(p != nullptr){
            p = p->next;
            lenA++;
        }
        while(q != nullptr){
            q = q->next;
            lenB++;
        }
        p = headA;q = headB;
        if(lenA > lenB){
            int res = lenA - lenB;
            for(int i = 0;i < res;i++){
                p = p->next;
            }
            while(p != nullptr){
                if(p == q) return p;
                p = p->next;q = q->next;
            }
            return nullptr;
        }else{
            int res = lenB - lenA;
            for(int i = 0;i < res;i++){
                q = q->next;
            }
            while(q != nullptr){
                if(p == q) return p;
                p = p->next;q = q->next;
            }
            return nullptr;
        }
    }
};
```

---
## 环形链表Ⅱ
[https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html]

#### 解法
1.使用哈希表
2.使用快慢指针，相遇后，说明有环，然后慢指针再跑一圈算出环的长度，之后再用两个指针在头节点，一个先跑环的长度，然后再一起跑，相遇的地方就是环的起点
- 证明：假设b = 圆的周长，a = 头结点到圆起点的位置：那么甲先走b长度，乙再一起走 ——— a + b -> 甲回到圆起点， a -> 乙刚到起点
```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> visited;
        while(head != nullptr){
            if(visited.count(head)){
                return head;
            }
            visited.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```
#### 思考
这种设计到是否访问过的问题都可以试试哈希表，很方便，但是那个快慢指针的方法也非常妙，以前只会用快慢指针判断是否相交，不知道怎么判断在哪相交。
