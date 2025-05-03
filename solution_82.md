## Step 1
### 考えたこと
ListNode に「前のノードを指す prev」があると良さそう。それを実装する。current.val と current.next.val が同じならば、prev.next を current.next.next (next を何個つけるかは while 回して決める) に繋ぐイメージ。

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next


class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(-1, head)
        prev = dummy
        current = head

        while current:
            if current.next and current.val == current.next.val:
                duplicate_value = current.val
                while current and current.val == duplicate_value:
                    current = current.next
                prev.next = current
            else:
                prev = current
                current = current.next

        return dummy.next
```

### (Accept 後に) 考えたこと
- 疑問点1：dummy.val は参照されないので、それを示すべき？

    ```python3
    dummy = ListNode(-1, head) # <- 上の実装
    dummy = ListNode(0, head) # <- dummy = ListNode(head) も同じ
    dummy = ListNode(None, head) # <- 参照しなさそう
    ```
    初期値を工夫するよりは、dummy の命名そのものを工夫したほうがいいかもしれないが、良いものが思い浮かばない。

- 疑問点2：ネスト深い？

    重複値スキップ処理を内部関数に分離してみる（see Step 2）

## Step 2

ネストが3段→2段になったが、そのために内部関数を作るのは得策か？（内部関数の再利用性が高ければ有効そう）

```python3
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def skip_duplicates(
            node: Optional[ListNode], duplicate_value: int
        ) -> Optional[ListNode]:
            while node and node.val == duplicate_value:
                node = node.next
            return node

        dummy = ListNode(-1, head)
        prev = dummy
        current = head

        while current:
            if current.next and current.val == current.next.val:
                duplicate_value = current.val
                current = skip_duplicates(current, duplicate_value)
                prev.next = current
            else:
                prev = current
                current = current.next

        return dummy.next
```

念のため他のひとのコードも見てみる：
- https://github.com/hroc135/leetcode/pull/4/files
- https://github.com/olsen-blue/Arai60/pull/4/files
- https://github.com/Ryotaro25/leetcode_first60/pull/4/files

十人十色で面白い。内部関数化しなくても許容範囲ではありそう。

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。
- 4m, 2.5m, 2.5m

これまで解いた問題を復習したい気分になってきた。