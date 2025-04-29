## Step 1
### 考えたこと
前から順番にノードを辿っていき、next ポインタがすでに訪問したノードを指したら、サイクルありと判定し、True を返すと良さそう。

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited = set()
        current = head
        while current:
            if current in visited:
                return True
            visited.add(current)
            current = current.next
        return False
```

### (Accept 後に) 調べたこと
- 疑問点1：制約に対して、set() の時間・空間計算量は問題ないか？

    時間計算量は問題なさそう。制約 ``number of nodes [0, 10^4]`` だが、
    - set.add(), if x in set ともに、各ノード O(1) でできるらしい（[ソース](https://stackoverflow.com/questions/7351459/time-complexity-of-python-set-operations)）。
    - 空間計算量 (memory 使用量) の見積りの仕方が分からない。1ノードあたりのデータサイズを仮定して、10^4 すればよい？

- 疑問点2：type hints の Optional はカスタムの型を指定するだけか？

    - ``Optional[X] is equivalent to X | None (or Union[X, None])`` らしい。なので、None を取ることも許容しているっぽい（サイクルがない場合は、最後のノードの next は None になるので理にかなっている）。
    - 参照：https://docs.python.org/3/library/typing.html


- 疑問点3：``Follow up: Can you solve it using O(1) (i.e., constant) memory?`` の解法は？

    - Solutions を見ていると、``Floyd's Tortoise and Hare Algorithm`` というのがあるらしい。
    - 1ステップずつ進むカメと、2ステップずつ進む野ウサギを考えたとき、もしサイクルがあれば必ず2つがどこかで出会う（本当か？）
    - サイクルに入るまでの時間を t_in, サイクルの長さを l_cycle とすると、時刻 t における2匹の位置は、t_in + t (mod l_cycle), t_in + 2t (mod l_cycle) となる。
    - 時刻 t における差は、(t_in + 2t) - (t_in + t) = t (mod l_cycle) であり、時刻が1進むごとに差も1 (mod l_cycle) で増える。サイクル内では、これは差が1ずつ狭くなっていくことと同じであり、最大 l_cycle ステップ以内には必ず両者は一致するので、連結リスト内にサイクルが存在すれば検知できる。

    ```python3
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None

    class Solution:
        def hasCycle(self, head: Optional[ListNode]) -> bool:
            slow, fast = head, head
            while fast and fast.next:
                slow = slow.next
                fast = fast.next.next
                if slow == fast:
                    return True
            return False
    ```

    - 時間計算量：t_in + c なので、o(n)
    - 空間計算量：2つのポインタを持つだけ → O(1)

## Step 2

```python3
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。Step 1, 2 の解法を交互に書いた。
- Step 1 の方法：30s, 40s, 30s
- Step 2 の方法：40s, 35s, 35s
