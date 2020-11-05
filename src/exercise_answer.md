# 練習問題解答

## 特徴的な演算子

1. 
    1. 必要なキーのみマッチさせればよい
    ```
    iex(1)> %{c: x} = %{a: 1, b: 2, c: 3}
    %{a: 1, b: 2, c: 3}
    iex(2)> x
    3 
    ```
    2. リストの順番通りにマッチさせる．必要のない要素は`_`で無視する．
    ```
    iex(3)> [_, x, _, y] =  [1, :a, "a", 2.3]
    [1, :a, "a", 2.3]
    iex(4)> x
    :a      
    iex(5)> y
    2.3  
    ```
    3. xとyについては前問と同じ．任意の要素以降のリストは`|`を使えば取り出せる
    ```
    iex(6)> [_, x, _, y | z] =  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    iex(7)> x
    2       
    iex(8)> y
    4       
    iex(9)> z
    [5, 6, 7, 8, 9, 10]
    ```
    4. 入れ子になっている場合も形を同じにすれば複数回のマッチを用いる必要はない．`:d`のリストのマッチングは`[_, y | _]`とすれば長いリストでも対応ができる．
    ```
    iex(10)> %{a: %{b: x, d: [_, y, _]}} = %{a: %{b: 1, c: 2, d: [3, 4, 5]}}
    %{a: %{b: 1, c: 2, d: [3, 4, 5]}}
    iex(11)> x
    1
    iex(12)> y
    4
    ```
    5. `|`は右側に要素が存在していなくても使用できる（これを実際に用いる場面は多分ない）
    ```
    iex(13)> [_, x, _ | y] =  [1, 2, 3]
    [1, 2, 3]
    iex(14)> x
    2
    iex(15)> y
    []
    ```

2. 
    1. `[_, x, 2]`
    ```
    iex(1)> [_, x, 2] = [1, 3, 2]
    [1, 3, 2]
    iex(2)> x
    3
    iex(3)> [_, x, 2] = [1, 2]   
    ** (MatchError) no match of right hand side value: [1, 2]
    iex(3)> [_, x, 2] = [1, 3, 1]
    ** (MatchError) no match of right hand side value: [1, 3, 1]
    ```
    2. `%{"c" => x}`
    ```
    iex(3)> %{"c" => x} = %{"a" => 1, "b" => 2, "c" => 3}
    %{"a" => 1, "b" => 2, "c" => 3}
    iex(4)> x
    3
    iex(5)> %{"c" => x} = %{"a" => 1, "b" => 2}          
    ** (MatchError) no match of right hand side value: %{"a" => 1, "b" => 2}
    iex(5)> %{"c" => x} = %{:a => 1, :b => 2, :c => 3}
    ** (MatchError) no match of right hand side value: %{a: 1, b: 2, c: 3}
    iex(5)> %{"c" => x} = %{a: 1, b: 2, c: 3}         
    ** (MatchError) no match of right hand side value: %{a: 1, b: 2, c: 3}
    ```
    3. `[_, _, x, _ | _]`
    ```
    iex(5)> [_, _, x, _ | _] = [1, 2, 3, 4, 5]
    [1, 2, 3, 4, 5]
    iex(6)> x
    3
    iex(7)> [_, _, x, _ | _] = [1, 2, 3, 4]   
    [1, 2, 3, 4]
    iex(8)> x
    3
    iex(9)> [_, _, x, _ | _] = [1, 2, 3]   
    ** (MatchError) no match of right hand side value: [1, 2, 3]
    ```

## 関数とモジュール

```
iex(1)> add = &(&1 + &2)
&:erlang.+/2
iex(2)> mul = &(&1 * &2)
&:erlang.*/2

iex(3)> func1 = fn (a, b, f) -> f.(a, b) end 
#Function<42.97283095/3 in :erl_eval.expr/5>

iex(4)> func1.(1, 2, add)
3
iex(5)> func1.(1, 2, mul)
2

iex(6)> func2 = &(&3.(&1, &2))
#Function<42.97283095/3 in :erl_eval.expr/5>

iex(7)> func2.(1, 2, add)
3
iex(8)> func2.(1, 2, mul)
2
```