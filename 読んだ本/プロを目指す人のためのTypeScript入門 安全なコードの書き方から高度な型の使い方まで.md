## 第1章 イントロダクション
### 型について
- 動的型付け言語の場合、変数や式ではなく**値**が型を持っていることがある
    - 静的型付けは変数の宣言や式といったプログラムの字面上の要素に対して型が紐づけられ、それがランタイムの値の型と整合する

### TypeScriptでは関数は第一級オブジェクト
- 関数を変数に入れたり動的に作ったりと、文字列やオブジェクトといった他の値と同様の取り扱いが可能

### 静的型付けのメリット
- 型安全性
- ドキュメント化と入力補完

## 第2章 基本的な文法・基本的な型
### 文と式の違い
- 式には結果があるが、文には結果がない
- 式の後にセミコロンを書くようなものは式文で、与えられた式を実行するだけ、というもの

### letはなるべく使わず、constで変数宣言を行う
- ほとんどの変数は再代入の必要がないから
- letが使われているということは、コードを読む人に「この変数は再代入されますよ」という意思表示をすることになり、コードを読む人がどこで再代入されるのかに意識を割かなくてはいけなくなる

### プリミティブ
- TypeScriptプログラムにおける基本的な値
- 文字列、数値、真偽値、BigInt、Null、undefined、シンボルの7種類

### リテラル
- 何らかの値を生み出すための式のこと
- 例えば数値リテラルは5や3.5など、プログラムに数を書くことで表現できる

### TypeScriptにおけるnumberはIEEE754倍精度浮動小数点
- 他の言語ではdouble型として扱われている、64ビットで表される小数型

### 論理演算子の短絡評価
- ` true || somethingMethod();`のようなコードがあった場合、somethingMethod()は評価すらされない、実行されない

### ??論理演算子
- `x ?? y`の場合、xがnullもしくはundefinedのときのみ、yを返す

### 論理演算子に対応した代入演算子
- `&&=`, `||=`, `??=`のこと
- 論理演算子による評価と代入を同時に行える
- `sample ||= true`は`sample = sample || true`とほぼ同じ

## 第3章 オブジェクトの基本とオブジェクトの型
### TypeScriptではオブジェクトが幅広く利用される
- ここでいうオブジェクトとはプレーンオブジェクト、ただの連想配列のこと
- TypeScriptではほとんどこのオブジェクトが使われるので、クラスはほとんど使われないといっても過言ではない

### オブジェクトがいつ同じなのか？
- 明示的にコピーしなければ常に同じ
- オブジェクトを明示的にコピーする方法
    - スプレッド構文を使う（ただしネストしたオブジェクトはコピーされない）
    - lodashというライブラリをインストールして、cloneDeepを使う
    - HTML標準のstructuredCloneを使う（関数、独自クラスのインスタンス、Symbolはコピーできない）

### interface宣言はtype文で代用可能
- ほとんどの場面でtype文でOK、type文の方がより多くの場面で使える

### オブジェクト型にインデックスシグネチャを持たせるのは非推奨
- 型安全性が破壊されるおそれがある
- 多くの場合は型安全なMapオブジェクトで代用できる

### typeofは濫用すべきではない
- 基本的には、typeofで型推論結果を取り出すより、type文で明示的に型を宣言する方がわかりやすいプログラムになる
- 型ではなく値が最上位の事実になるような場合には活用できる

### 部分型とは
- 2つの型の互換性を表す概念
- 型Sが型Tの部分型であるとは、型Sの値が型Tの値でもある、という意味
- TypeScriptにおける部分型関係は構造的部分型である
    - オブジェクト型のプロパティを実際に比較して、部分型かどうかを決める方式
    - 実用プログラミング言語の中では、比較的珍しい特徴

### 配列へのインデックスアクセスの危険性
- インデックスシグネチャの危険性と同じく、ランタイムでは存在しない要素にアクセスできてしまう
- 回避する1つの方法は、インデックスアクセスは極力使用せず、配列の要素を用いる時はfor-of文などを使うこと

### 分割代入で宣言された変数には型注釈がつけられない
- 変数の型は型推論で決められ、基本的にはその変数に入るプロパティの型と同じになる

### 分割代入のデフォルト値
- デフォルト値はundefinedのみ対して適応される

### Dateオブジェクトは使いづらい
- 日時操作のやりにくさ、ミュータブルであることなどが理由
- Temporalという新しい組み込みオブジェクト群の導入が進んでいるので、将来的にはそっちを使うのを推奨

### プリミティブなのにプロパティがある
- `"test".length`のように、プリミティブな値に対してプロパティアクセスを行うたびに、一時的にオブジェクトが作成されていて、プロパティアクセスが終了するとそのオブジェクトは消える

## 第4章 TypeScriptの関数
### 関数の定義はアロー関数式がおすすめ
- 簡潔な構文、省略記法が使える、thisの扱いも有利

### 戻り値の型注釈の省略について
- 戻り値の型注釈は省略することができ、省略した場合は関数内部の処理によって型推論で型が決められる
- 一長一短ではあるが、適切なコンパイルエラーが発生しなくなってしまう可能性があるので、なるべく書いた方が良いかも

### 引数の型注釈の省略について
- その関数式の引数の型がすでに決まっているような状態の時はcontextual typingが働くので、引数の注釈は省略できる

### コールシグネチャ
- コールシグネチャという方法を使うと、独自のプロパティを持った関数型を定義できる
- 一昔前のJavaScriptでこういうやり方がよく行われていたから一応用意されているだけなので、あまり使わなくて大丈夫

### 関数型の部分型関係について
- 変数の型よりも情報量の多いオブジェクト（型情報に書かれている型の部分型に属するオブジェクト）が得られる場合もある
    - TypeScriptの型情報はあくまでも型システムの中での話であり、ランタイムの挙動には影響を及ぼさないから

- どんな値を返す関数型も、void型を返す関数型の部分型として扱われる
- 関数型の戻り値の型は、関数型の共変の位置にあるといい、関数型の引数の型は反変の位置にあるという
- メソッド記法で宣言された関数型においては、部分型と見なされるための条件が本来よりも緩くなっていて型安全性を破壊してしまう原因になり得るので、オブジェクトリテラルで関数型を定義する際はメソッド記法ではなく通常の記法を用いるべき
- 関数の引数の型は必要以上に厳しくせず、関数の処理に必要な必要最低限の型にすべき
    - オブジェクトの場合は、読み取り専用プロパティも普通のプロパティの部分型として見なされてしまうので、readonlyプロパティを過信しすぎないようにする

### ジェネリクス
- どんな型に対しても通用するようなロジックを持つ関数に適している
- 関数呼び出しの際は、明示的に型引数を指定しなくても型推論で補ってくれる

## 第5章 TypeScriptのクラス
### クラスも変数
- `new User()`は、正確に言うと「Userという変数に入っているクラスのインスタンスを作成する」という意味
- TypeScriptでは、式に表れる識別子は必ず変数を表す
- 関数やクラスもすべて「変数に入ったオブジェクト」として統一的に扱われる

### private修飾子と`#`のどちらを使うべきか
- ランタイム時での挙動などで細かい違いがあるので、迷ったら`#`を使っておけばOK

### クラス宣言はインスタンスの型を作る
- クラスオブジェクトの実体と、そのインスタンスを表す型をセットで作成できる
- クラス式だと型は作られなくなる。不便なので基本的にはクラス宣言を使うべき
- クラス宣言というのは、型名の名前空間と変数名の名前空間、その両方に同時に名前を作成する構文

### `instanceof`はあまり使うべきではない
- クラスに依存したロジックだから
- TypeScriptにおいては、クラスを使わずともオブジェクト型で色々な表現が可能

### 継承
- 前提として、継承自体が複雑な機能で、プログラムの設計を複雑化してしまうので非推奨
    - TypeScriptはクラスを使わなくてもなんとかなる言語
- 継承を用いることで、あるクラスの部分型となる別のクラスを作ることが出来る

### `implements`によるクラスの型チェック
- `implements`キーワードにより、そのクラスのインスタンスは与えられた型の部分型である、という宣言になる
- クラス定義の差異、そのクラスがある型の部分型となることを意図しているならば`implements`を積極的に使う

### 例外処理について
- 例外を使うより、失敗を表す戻り値を使う方がおすすめ
- 例外の大域脱出という特徴を活かせる、エラーが発生したら処理を中断して共通のエラー処理を行いたい場合には例外がおすすめ

### クロージャについて
- 関数の中にデータが内包されているもの
- 関数内で特定の変数を参照し続けることで、関数が状態を保てる仕組み
- クロージャは、クラスのプライベートプロパティとは別の手段によるカプセル化と見なすことができる

### クラスを使うべきか？
- 基本的にオブジェクトで事足りる
- プライベートプロパティなどによるカプセル化が有用だったり、一つのクラスが複数のメソッドを提供するのが責務としてふさわしいような場合にはクラスを使うことも検討
- 実際はパフォーマンスの観点からも、データと値を分離した方が便利なことが多くあるので、関数もメソッドではなく独立した関数にした方がいい場合が多い

## 第6章 高度な型
### ユニオン型
- 「型Tまたは型U」のような表現ができる型
- 直和型と呼ばれる場合もあるが、直和型はユニオン型とは別の概念として存在しているので正しくない

### インターセクション型
- 「T型でありかつU型でもある値」を意味する型
- 日本語では交差型と呼ぶのが一般的
- オブジェクト型を拡張した新しい型を作るというユースケースでよく使われる
- 異なるプリミティブ型同士のインターセクション型を作った場合、never型が出現する

### リテラル型
- プリミティブ型をさらに細分化した型
- stringではなく`foo`という型として定義することができ、`foo`という文字列以外をエラーに出来る
- リテラル型はユニオン型と組み合わせて使うことで、特定のリテラル型のみ扱えるようになる
- リテラル型が自動的に対応するプリミティブ型に変換されることをwideningという
    - letで宣言された変数に代入される時
    - オブジェクトリテラルの型が推論される時、各プロパティの型がリテラル型となる場合
    - ただし、式としてのリテラルに対して型推論で推論されたもののみがwideningされ、プログラマーが明示的に書いたリテラル型はwideningされない

### 型の絞り込み
- ユニオン型を持つ値が実際にはどの値なのかをランタイムに特定するコードを書くことで、型情報がそれに応じて変化する仕組みのこと
- コントロールフロー解析とも呼ばれる
- 等価演算子を用いた絞り込みが最もベーシック
- tyoepf演算子を用いた絞り込みでは、typeof演算子が戻す式の評価結果に応じた文字列を使う

### 代数的データ型をユニオン型で再現する
- TypeScriptで擬似的な代数的データ型を作るには、ユニオン型の構成要素であるオブジェクト型のそれぞれにタグとなるプロパティを付け加える
    - プロパティ名はtypeとかが一般的

### lookup型
- `T[K]`という構文で、Tというオブジェクト型が持つKというプロパティの型、となる
- 型情報を再利用して、コードをDRYにすることが主な目的
- 一見して具体的な型がわからないという欠点もあるので使いすぎには注意

### keyof型
- 型Tに対して`keyof T`と書くことで、オブジェクト型からそのオブジェクトのプロパティ名のリテラル型を得る
    - プロパティが複数ある場合はユニオン型

- `typeof`を使うことで値から型を作り出し、`keyof`でそれを加工して使うというパターンがよくある
- ジェネリクスと組み合わせると便利で、任意のオブジェクト型とそのプロパティ名のリテラル型を受け取る関数などを定義できる

### 型アサーション
- 結論として、TypeScriptの型安全性を意図的に破壊する機能なので、型アサーションはできるだけ使わない
- 型推論による型情報が不正確だと明確なときにだけに使う
    - ユーザー定義型ガードを使うことで型アサーションを使わないでも解決できることがある

- 後置の`!`は、その式がnullまたはundefinedである可能性を無視する記法であり、こちらも型アサーション同様出来るだけ使わないようにする

### `as const`
- 式への型推論に対して、4つの効果を及ぼす
    - 配列リテラルの型推論結果を配列型ではなくタプル型にする（readonly）
    - オブジェクトリテラルから推論されるオブジェクト型はすべてのプロパティがreadonlyになる
    - 文字列・数値・BigInt・真偽値リテラルに対してつけられるリテラル型がwideningしないリテラル型になる
    - テンプレート文字列リテラルの型がstringではなく、テンプレートリテラル型になる

- 基本的には、`as const`がつけられた式の各種リテラルを「変更できない」ものとして扱うことを表す

### any型
- 型チェックを無効化する型
- 型安全性を破壊するという意味で危険な機能なので、とにかく避けるべき
- `as`やユーザー定義型ガードを使えばanyは使わなくてすむ

### unknown型
- 何でも入れられる型
- anyとは違い、存在しないプロパティへアクセスした際などはコンパイルエラーを出してくれる
- ユーザー定義型ガードと一緒に使って、型の絞り込みを行うことが多い
- 「何が入るか、定義の段階ではわからない」というような場合に適している

### object型
- プリミティブな型を除いた、本当にオブジェクト（`{}`）だけを受け入れたい時に使う
- 単体で使うというよりかは、インターセクション型の要素として使うことが多い

### never型
- 「当てはまる値が存在しない」という性質を持つ型
- 例えば関数の引数としてnever型を定義して関数内部の処理で任意の型に当てはめたりできるが、「never型に当てはまる値」は存在しないので、その関数を呼び出すことができず、問題は発生しない
- 言い換えると「never型はすべての型の部分型である」となる
    - どんな型の変数にでも代入できるのはこれが理由

### ユーザー定義型ガード
- 型の絞り込みを自由に行うための仕組み
- 型安全性を破壊するおそれがある機能
    - `any`や`as`よりも取り回しが良いので、必要な場合は積極的に使ってOK

- 戻り値の型として型述語を書く
- 型の絞り込みを行う関数として定義して使うが、TypeScriptコンパイラはユーザー定義型ガードの実装が型述語に書かれているとおりの絞り込みを行っているかどうかは保証しない

### mapped types
- `{ [P in K]: T }`という構文で使い「Kというユニオン型の各構成要素Pに対して、Pというプロパティが型Tを持つようなオブジェクトの型」という意味になる
- `{ [P in keyof T]: U }`という形（Tは既存の型引数、Uは任意の型）は特別で、homomorphic mapped typeと呼ぶ
    - 結果がTの構造を保存する（配列型に対するmapped typeの結果が配列型のままになるなど）

### conditional types
- 型の条件分岐を行うための型
- `X extends Y ? S : T`という構文で、それぞれ何らかの型
    - XがYの部分型ならばS、そうでなければTという意味になる

- union distributionという性質があり、これは上述のhomomorphic mapped typeにも類似の性質がある
    - `X extends Y ? S : T`でXが型変数かつその中身がユニオン型の場合、「ユニオン型のconditional type」が「conditional typeのユニオン型」になる

### 組み込みの型
- `Readonly<T>`
    - Tに与えられたオブジェクト型のすべてのプロパティを読み取り専用にしたオブジェクト型

- `Partial<T>`
    - Tのすべてのプロパティをオプショナルにした型

- `Required<T>`
    - Tのすべてのプロパティをオプショナルではなくした型

- `Pick<T, K>`
    - Tというオブジェクト型のうち、Kで指定した（Kの部分型である）名前のプロパティのみ残したオブジェクト型

- `Omit<T, K>`
    - Tというオブジェクト型のうち、Kで指定したプロパティを除いたオブジェクト型

- `Extract<T, U>`
    - T（普通はユニオン型）の構成要素のうち、Uの部分型であるものだけを抜き出したユニオン型

- `Exclude<T, U>`
    - Tの構成要素のうち、Uの部分型であるもののみを除いたユニオン型

- `NonNullable<T>`
    - `Exclude<T, null | undefined>`と同様

## 第7章 TypeScriptのモジュールシステム
### モジュールの実体は1つ
- 複数回、複数の場所でインポートされたとしても実体は1つ

### defaultエクスポートは使わない方が良い
- エディタによるサポートが弱くなるから
- import時のasによって変数名を変える機能も、エディタのサポートが弱くなるのであまり使わない

### CommonJSモジュール
- ES Modulesが普及する前から存在するモジュールシステムで、Node.jsではライブラリ含め広く使われている
- importがrequireだったり、exportのやり方が違っていたりする

## 第8章 非同期処理
### 非同期処理とは？
- 裏で行われる処理、もしくは時間がかかる処理、またはその両方の特徴を持つ処理のこと

### シングルスレッドモデル・ノンブロッキング
- TypeScriptでは、非同期処理はノンブロッキングなものを指す
- まずブロッキングな処理とは、その処理の実行が完了するまでプログラムがそこで停止するような処理のこと
    - JavaScript・TypeScriptでは実行モデルとしてシングルスレッドが採用されているので、時間がかかる処理がブロッキングなのは基本的に歓迎されない

- シングルスレッドとは、1つの実行環境内でプログラムの複数箇所が同時に実行されることはない、ということ
    - 1つの実行環境とは、プロセス1つのことだと理解してだいたい問題ない
    - 複数の実行環境が協調するようなプログラムを作るには、Node.jsのworker_threadsモジュールや、WebブラウザのWeb Workersを使う
    - 複数の実行環境の間のデータのやり取りは、SharedArrayBufferを除いて非同期通信によって行われる

- JavaScriptで書かれたサーバーはクライアントからのリクエストに対してレスポンスを計算する部分（アプリケーションロジック）を担当するのであって、クライアントからのリクエストやレスポンスを実際に送受信するのはOSに任せている
    - JavaScriptがシングルスレッドというのは、あくまでアプリケーションロジックが書かれたJavaScriptプログラムの部分が並列に実行できない、という意味

## 第9章 TypeScriptのコンパイラオプション
### tsconfig.json
- TypeScriptコンパイラが自動的にこのファイルを読み込んで、コンパイラ時にその設定を適用してくれる
- 基本的にはコンパイラオプションを指定したり、コンパイル対象となるファイルの一覧を指定したりする時に使う

## 感想
- プログラミングにおける文や式の説明から、JavaScriptとTypeScriptの仕様に関わる部分まで網羅的に説明してくれていてわかりやすかった
- ただ構文を説明するだけでなく「こういう書き方の方がおすすめです」「こちらの方が広く使われているようです」など、具体的かつ実践的なコードの書き方にまで言及してくれているので「わかったけど、どっち使えばいいんだ？」という迷いが生じにくい
- やはり第6章が特に学びが多い
    - この章を深く理解できているかどうかでTypeScriptプログラミング力をある程度判断できると思う
    - それぞれの型の使い方や構文はわかっているが、仕様の細かい部分を把握できていなかったので読んでよかった
    - union distributionがまだちょっと理解しきれていないので、別途調べる

- この本が出版されてからある程度TypeScriptのバージョンも進んでいるので、そこは別途キャッチアップ出来ると良さそう
    - [as const satisfiesとか](https://zenn.dev/tonkotsuboy_com/articles/typescript-as-const-satisfies)