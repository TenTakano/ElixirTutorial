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

## 制御構文

1. 以下のいずれでも同じ結果が得られます．（個人的に前者の方がシンプルで美しいと思います）
    ```
    iex(1)> func = fn
    ...(1)>   %{a: a} -> a
    ...(1)>   _       -> nil
    ...(1)> end
    #Function<44.97283095/1 in :erl_eval.expr/5>

    iex(2)> func.(%{a: 1})
    1
    iex(3)> func.(%{b: 1})
    nil
    ```
    ```
    iex(1)> func = fn map ->
    ...(1)>   case map do
    ...(1)>     %{a: a} -> a
    ...(1)>     _       -> nil
    ...(1)>   end
    ...(1)> end
    #Function<44.97283095/1 in :erl_eval.expr/5>

    iex(2)> func.(%{a: 1})
    1
    iex(3)> func.(%{b: 1})
    nil
    ```
2. 前問同様に`case`文を用いても解くことができます．
    ```
    iex(1)> func = fn
    ...(1)>   %{a: a}              -> a
    ...(1)>   arg when is_map(arg) -> nil
    ...(1)>   _                    -> :error
    ...(1)> end

    #Function<44.97283095/1 in :erl_eval.expr/5>
    iex(2)> func.(%{a: 1})
    1
    iex(3)> func.(%{b: 1})
    nil
    iex(4)> func.(1)
    :error
    ```

3. `func/1`, `func1/1`, `func2/1`いずれでも課題を解くことができます．`func/1`と他2つの違いとして，`func/1`は関数呼び出し時に`rem/2`が実行できることを暗黙的に期待するので，数値以外の引数が与えられるとクラッシュします．
    ```
    defmodule Sample do
    def func(n) do
        case {rem(n, 15), rem(n, 5), rem(n, 3)} do
        {0, _, _} -> "fizz buzz"
        {_, 0, _} -> "buzz"
        {_, _, 0} -> "fizz"
        _         -> n
        end
    end

    def func1(n) do
        case n do
        n when rem(n, 15) == 0 -> "fizz buzz"
        n when rem(n, 5)  == 0 -> "buzz"
        n when rem(n, 3)  == 0 -> "fizz"
        _                      -> n
        end
    end

    def func2(n) when rem(n, 15) == 0, do: "fizz buzz"
    def func2(n) when rem(n, 5)  == 0, do: "buzz"
    def func2(n) when rem(n, 3)  == 0, do: "fizz"
    def func2(n),                      do: n
    end
    ```
    `func2/1`で使用しているのは1行で関数を書くための記法です．1行で書かず，以下のように書いても同じ結果が得られます．
    ```
    def func2(n) when rem(n, 15) == 0 do
    "fizz buzz"
    end
    ...
    ```
    `func2/1`では関数レベルでマッチングをしています．`case`の時と同じように先に書かれた関数から順次マッチングされていきます．

    いずれの関数でも以下のような出力が得られます
    ```
    iex(1)> Sample.func(1)
    1
    iex(2)> Sample.func(3)
    "fizz"
    iex(3)> Sample.func(5)
    "buzz"
    iex(4)> Sample.func(15)
    "fizz buzz"
    ```

## Enumモジュール

1. 
    ```
    iex(3)> Enum.map(prime_ministers, fn %{first: first, last: last} -> first <> last end)
    ["博文伊藤", "清隆黒田", "實美三條", "有朋山縣", "正義松方"]
    ```
2. 
    ```
    iex(6)> [head | tail] = ["博文伊藤", "清隆黒田", "實美三條", "有朋山縣", "正義松方"]
    ["博文伊藤", "清隆黒田", "實美三條", "有朋山縣", "正義松方"]
    
    iex(7)> Enum.reduce(tail, head, fn (name, acc) -> acc <> " -> " <> name end)
    "博文伊藤 -> 清隆黒田 -> 實美三條 -> 有朋山縣 -> 正義松方
    ```
    `Enum.join/2`という便利な関数もあり，それを使うと前処理の要らない処理を書くこともできます．
    ```
    iex(8)> Enum.join(["博文伊藤", "清隆黒田", "實美三條", "有朋山縣", "正義松方"], " -> ")
    "博文伊藤 -> 清隆黒田 -> 實美三條 -> 有朋山縣 -> 正義松方"
    ```