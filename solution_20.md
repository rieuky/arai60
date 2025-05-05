## Step 1
### 考えたこと
括弧のペアを辞書で管理する。括弧の open 側があれば、close 側を stack に入れる。
close 側の括弧が検知されたとき、1) stack の一番上と対応しない、もしくは 2) stack がそもそも空であるならば、False を返す。
すべての括弧を走査したあと、stack が空なら True、そうでなければ False を返す。

```python3
class Solution:
    def isValid(self, s: str) -> bool:
        lookup_dict = {"(": ")", "{": "}", "[": "]"}
        stack = []

        for paren in s:
            if paren in lookup_dict.keys():
                stack.append(lookup_dict[paren])

            if paren in lookup_dict.values():
                if not stack:
                    return False
                if paren != stack.pop():
                    return False

        if stack:
            return False

        return True
```

## Step 2
### コードを読みやすくする

- `lookup_dict` は `lookup` だけでもいいかも。
- 最後の `if stack:` を簡潔にしたい。

    代替案1
    ```python3
    return len(stack) == 0
    ```

    代替案2
    ```python3
    return not stack
    ```
- open と close の分岐を `if ~ if ~` とするか、`if ~ else ~` とするか、`if ~ elif ~`とするかどちらが良いだろうか。
    - `if ~ if ~` は2条件の関係性が伝わらない、かつ独立に評価されて実行時間がかかりそうなので、まず選択されなさそう。
    - `if ~ else ~` は open, close の2択なので、最も簡潔になる（今回はこちらを選択）。
    - `if ~ elif ~` は未完全な印象を受ける (最後に `else` が欲しい)。
        - ただ、今回は制約の `s consists of parentheses only '()[]{}'.` から、`if ~ elif ~ else ~` として、括弧以外の文字に対する処理を書く必要はないと判断。
- 今後の拡張性を考慮した実装をすべき vs 必要になったら実装すれば良い、の使い分けの判断基準・境界はありますでしょうか？


### 修正版

```python3
class Solution:
    def isValid(self, s: str) -> bool:
        lookup = {"(": ")", "{": "}", "[": "]"}
        stack = []

        for paren in s:
            # open
            if paren in lookup.keys():
                stack.append(lookup[paren])

            # close
            else:
                if not stack or paren != stack.pop():
                    return False

        return not stack
```

完走者のコードを見る：
- https://github.com/hroc135/leetcode/pull/6/files
    - [collections.deque](https://docs.python.org/3/library/collections.html) や [queue.LifoQueue](https://docs.python.org/3/library/queue.html#:~:text=class%20queue.LifoQueue(maxsize%3D0)) も使える。
    - `import` するなら、そのデータ構造の実装も読んでおきたいが。
- https://github.com/olsen-blue/Arai60/pull/6/files
- https://github.com/Ryotaro25/leetcode_first60/pull/6/files
    - parentheses \(\), brackets \[\], braces \{\} のイメージだったが、括弧を総称する表現はある？（問題文は brackets だったので、素直に brackets とすれば良かったかもしれないが）

## Step 3 ~ 5
時間を計りながら、3回ミスなく書く。
- 2m40s, 1m20s, 1m10s