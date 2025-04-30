## Step 1
### 考えたこと
ListNode に「前のノードを指す prev」があると良さそう。それを実装する。current.val と current.next.val が同じならば、prev.next を current.next.next に繋ぐ？

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next


class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head
        while current and current.next:
            if current.val == current.next.val:
                current.next = current.next.next
            else:
                current = current.next
        return head
```

### (Accept 後に) 考えたこと
- 疑問点：スキップされたノードはどう処理されるか？


## Step 2

```python3

```

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。
- 30s, 35s, 30s 
