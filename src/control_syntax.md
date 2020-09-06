# 制御構文

プログラムを書くには制御構文が必須となります．他言語で最も一般的な制御構文は`if`文でしょう．しかし，Elixirは`if`文を有しているにも関わらず，使用頻度はそれほど高くはありません．Elixirの制御構文には`case`，`cond`，`if`がありますが，`case`を最も多く使用します．ここでは`case`文を解説します．

Elixirらしいコードになれるため，まずは`if`文を使わずに`case`で表現してみましょう

## case文

以下は，`some_list`の中身が空の場合`"empty!"`と表示し，それ以外の場合は`"list has value"`と表示するcase文のサンプルです．

```
case some_list do
  [] ->
    "empty!"
  _ ->
    "list has value"
end
```

Elixirでは`case`，`cond`，`if`は関数として実装されています．なので，これらは「条件によって制御を分岐させる」のではなく，「引数の形や値によって処理を変える」という解釈にイメージを切り替えてください．（それぞれのマッチの中で最終の行が戻り値として返ります）

また，case文の中ではマッチが行われているので値の束縛を行うことも出来ます．

```
case some_list do
  [] ->
    "empty!"
  [head | _] ->
    "first value: #{to_string(head)}"
end
```

マッチングに`when`を用いる事でさらに細かいマッチングを行うことも出来ます．ただし，`when`の後に用いる事ができる式はマクロのみという制約があります．（つまり，任意の式や関数を用いる事は出来ません）

```
case some_value do
  [] ->
    "empty!"
  [head | _] when is_number(head) ->
    "first value is a number"
  [head | _] when is_atom(head) ->
    "first value is an atom"
  [head | _] ->
    "first value is an unexpected type"
end
```

## 関数とcase

case文は無名関数の引数部分と組み合わせることが出来ます．この場合，書き方が少し変化します

```
fn
  [] ->
    "empty!"
  [head | _] ->
    "first value: #{to_string(head)}"
end
```