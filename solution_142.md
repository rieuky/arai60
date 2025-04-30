## Step 1
### 考えたこと
前から順番にノードを辿っていき、next ポインタがすでに訪問したノードを指したら、その参照されたノードがサイクルの開始点。

今後、このクラスに``__bool__``が実装された際の、意図しない挙動変化を回避するために、``while current:`` でなく ``while current is not None:`` で実装した (前回の[フィードバック](https://github.com/rieuky/arai60/pull/1#discussion_r2065586676)を受けて)。

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited = set()
        current = head
        while current is not None:
            if current in visited:
                return current
            visited.add(current)
            current = current.next
        return None
```

### (Accept 後に) 考えたこと
- 疑問点：``Floyd's Tortoise and Hare Algorithm`` を使って解答すると？

    - サイクル検知をした後に、サイクルの入口を探す処理を入れると良さそう。
    - 2ポインタが出会うノードは分かる。それとサイクル入口の関係を導出したい。
    - 記号を定義する：
        - サイクルに入るまでの移動距離 l_in
        - サイクルの長さ l_cycle
        - 合流までに slow が head から進んだ距離 l_meet
    - slow の移動距離：l_meet = l_in + d
    - fast の移動距離：2 * l_meet = 2 * l_in + 2 * d
    - 2つのポインタが合流するためには、この差はサイクル長の倍数でないといけない（ちょうどサイクル長の倍数だけ fast が進んでいないといけない）。よって、k を自然数として、
        - l_in + d = k * l_cycle
        - よって、d = k * l_cycle - l_in
        - 両辺 mod l_cycle を取ると、d (mod l_cycle) = (- l_in) (mod l_cycle)
        - これより、合流地点から距離 l_in 進めば、サイクルの入口に達することが分かる。
    - ただ、具体的な l_in は分からない（それを求めるのが題意なので）。
    - だが偶然にも、l_in は head からサイクル入口までの距離なので、片方のポインタを head に戻せば、合流地点からスタートするポインタ A と head から移動するポインタ B を同じ速度で動かせば、ちょうどサイクル入口で合流することになる。

    ```python3
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None


    class Solution:
        def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
            slow = head
            fast = head
            while fast and fast.next:
                slow = slow.next 
                fast = fast.next.next
                if slow is fast:
                    fast = head
                    while fast is not slow:
                        slow = slow.next
                        fast = fast.next
                    return fast
            return None
    ```

## Step 2

```python3
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                fast = head
                while fast is not slow:
                    slow = slow.next
                    fast = fast.next
                return fast
        return None
```

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。Step 1, 2 の解法を交互に書いた。
- Step 1 の方法：45s, 40s, 35s 
- Step 2 の方法：55s, 55s, 55s
