# ElixirとErlang

ElixirとErlangは切っても切れない関係にあります

- ElixirはErlang VM上で動いています
- Javaに対するKotlinやScalaと同じような位置づけです
- ElixirとErlangは相互に資産を利用し合えます（Erlangの持つ長い実績を持つライブラリを使えるのはかなり強力です）
- 上記のような理由から，Erlangの持つ言語的特性はElixirにも当てはまります

## Erlang

- 誕生は1986年（34年前）でかなり歴史がある
- 元々エリクソン社が電話交換機を作るために開発したため，以下のような用途に優れた特性を持っている
  - とにかくサービスを継続することが大事（耐障害性）
  - アップデート時でも止められない（ホットスワップ）
  - 多数の機器で同時に利用される（分散性）

上記のような特徴は現代におけるサーバーの分散システムでも必要とされていて，言語としてそもそもサーバーシステムに適している．Nintendoのゲームサーバーでも採用実績があるらしい([参考](https://employment.en-japan.com/engineerhub/entry/2019/08/01/103000))この特性を実現するために以下のような機能を持っている．

- 軽量プロセス
  - OSの提供するプロセスやスレッドとは異なる
  - Erlang VMによって管理されている
  - 生成オーバーヘッドがかなり小さく，大量のプロセスを性能低下無しに生成できる
    - イメージとしてはオブジェクト指向言語でクラスをインスタンス化するような気軽さで立ち上げられる
- 基本的にエラーをキャッチしない
  - エラー発生時にはプロセスを殺して，新しくプロセスを立ち上げ直す
  - （つまり死活監視をするプロセスが存在して・・・）

ここまで聞くと欠陥がないように見えるが，もちろんそんな事は無い．特に古典的な言語のため生産性・可読性が・・・

## Elixir

書きやすく読みやすい言語でErlangの特性を利用するために開発されている．

- 言語的特性はErlangの特性を引き継いでいる
- コードの見た目はRubyに似ている
  - 開発者がRuby on Railsのcontributorだったような・・・（うろ覚え）
  - ただし言語的特性は全く別
  - よく「Rubyの皮を被ったErlang」と言われる

Elixir独自の特徴（記法的な面から）としては

- パターンマッチが強力（大事．慣れると他言語に戻れないぐらい大事）
- コードの自動生成や言語拡張などができるマクロ（言語自体がマクロでどんどん拡張して発展させたもの）
  - むやみに濫用すると危険．黒魔術．
