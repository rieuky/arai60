## Step 1
### 考えたこと
前から順番にノードを辿っていき、隣接するノードの値 (val) が同じなら、参照 (next) を繋ぎ直す。

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

    - Python では意識しなくても良さそう。Garbage Collection が、参照カウントがゼロのオブジェクトを自動で消してくれるらしい（[Stack Overflow へのリンク](https://stackoverflow.com/questions/59978988/delete-a-node-in-a-linked-list-is-any-form-of-garbage-collection-necessary)）（Official な文献の探し方が分からなかった）
    - GC を Python で明示的に書くケースはある？（ご存じの方、教えていただけるとありがたいです）

## Step 2

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

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。
- 30s, 35s, 30s 
