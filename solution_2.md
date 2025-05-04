## Step 1
### 考えたこと
新しい連結リストを作り、そこに値を格納していく。繰り上がりを格納する変数 `carry_over` を作り、各桁の合計値に都度足して、次の桁の繰り上がりを計算する。2つの連結リスト走査のための `while` の条件はすぐには思いつかないので、実装しながら考える。

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next


class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        result_head = ListNode()
        current_tail = result_head
        carry_over = 0

        # while l1 or l2: <- Test case 3 でエラー
        while l1 or l2 or carry_over:
            sum = carry_over

            if l1:
                sum += l1.val
                l1 = l1.next
            if l2:
                sum += l2.val
                l2 = l2.next

            carry_over = sum // 10
            current_tail.next = ListNode(val=sum % 10)
            current_tail = current_tail.next

        return result_head.next
```

`while l1 or l2:` に繰り上がりの条件を加えるのを忘れていて、デバッグに時間がかかった。

### (Accept 後に) 考えたこと

- 疑問点：慣れてくると `while` の条件が一発で書けるようになる？

    想像だがプロの方は、不完全な `while` を書いていても、その先の実装を進める過程で、ミスに気づいて修正できそう（実行 → error message → デバッグでなく、頭の中で実行 → 修正？）。なので、実行前の段階という意味で一発で書けるように他人からは見えそう。

## Step 2

コードを読みやすくする（変数名・論理構造）。
- `sum` -> `digit_sum` として明確に意図を説明する命名に変更。
- ネストの深さは今回は問題ないと判断。

```python3
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        result_head = ListNode()
        current_tail = result_head
        carry_over = 0

        while l1 or l2 or carry_over:
            digit_sum = carry_over

            if l1:
                digit_sum += l1.val
                l1 = l1.next
            if l2:
                digit_sum += l2.val
                l2 = l2.next

            carry_over = digit_sum // 10
            current_tail.next = ListNode(val=digit_sum % 10)
            current_tail = current_tail.next

        return result_head.next
```

完走者のコードを見る：
- https://github.com/hroc135/leetcode/pull/5/files
    - `result_head` は `sentinel` でも良さそう（SWE の常識にギリギリ入るか入らないからしい）
- https://github.com/olsen-blue/Arai60/pull/5/files
    - 小田さんのコメント「sum は built-in にあるので私は避けます」。完全にその考えが抜け落ちてた。
- https://github.com/Ryotaro25/leetcode_first60/pull/5/files
 - 再帰でも解けるらしい。ほぼ書いたことないけどやってみる。

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。
- 2m, 1m50s, 1m30s

## 再帰で書く

- もう少し整える余地はありそうだが、再帰は読みにくい気がする。これは自分が慣れていないからなのか、SWE 共通なのか？
- Runtime が 0 ms になり、先ほどの 7ms から結構短くなったが、これはなぜだろうか？メモリは再帰 18 MB、非再帰 17.8 MB なので、メモリは再帰の方が使用しているらしい。

```python3
class Solution:
    def addTwoNumbers(
        self, l1: Optional[ListNode], l2: Optional[ListNode]
    ) -> Optional[ListNode]:
        def add_two_nodes(
            n1: Optional[ListNode], n2: Optional[ListNode], carry_over: int
        ) -> Optional[ListNode]:
            if not (n1 or n2 or carry_over):
                return None
            digit_sum = carry_over
            if n1:
                digit_sum += n1.val
            if n2:
                digit_sum += n2.val

            n1_next = n1.next if n1 else None
            n2_next = n2.next if n2 else None

            node = ListNode(digit_sum % 10)
            node.next = add_two_nodes(
                n1_next, n2_next, digit_sum // 10
            )
            return node
        return add_two_nodes(l1, l2, 0)
```
