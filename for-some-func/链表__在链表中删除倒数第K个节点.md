# **链表__在链表中删除倒数第K个节点**

题目描述

给出一个单链表，返回删除单链表的倒数第 K 个节点的链表。

输入描述:

```
n 表示链表的长度。val 表示链表中节点的值。
```

输出描述:

```
在给定的函数内返回链表的头指针。
```

示例1

输入

```
5 4
1 2 3 4 5
```

输出

```
1 3 4 5
```

```cpp

list_node * remove_last_kth_node(list_node * head, int K)
{
    //////在下面完成代码
    list_node * cur = head;
    while(cur!=NULL){
        cur=cur->next;
        K--;
    }
    if(K==0) head = head->next;
    if(K<0){
        cur = head;
        //因为要删除cur这个节点就要到其前面的节点进行操作
        while(K!=-1){
            cur = cur->next;
            K++;
        }
        cur->next = cur->next->next;
        //free(cur);
    }
    
    return head;

}
```





## 方法二 双指针间距

```cpp
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode* p = pListHead;
        ListNode* q = pListHead;
        while(k>0&&p!=NULL){
            k--;
            p=p->next;
        }
        if(k>0) return NULL;
        
        //这时候k=0;q相对p就是k的距离
        while(p!=NULL){
            p=p->next;
            q=q->next;
        }
        return q;
       
    }
};
```

