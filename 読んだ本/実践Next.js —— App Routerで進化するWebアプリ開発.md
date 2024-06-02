## これはなに
- [実践Next.js —— App Routerで進化するWebアプリ開発](https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Next-js-%E2%80%94%E2%80%94-App-Router%E3%81%A7%E9%80%B2%E5%8C%96%E3%81%99%E3%82%8BWeb%E3%82%A2%E3%83%97%E3%83%AA%E9%96%8B%E7%99%BA-%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E9%81%B8%E6%9B%B8-ebook/dp/B0CW1KC9N8/ref=sr_1_1?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=LT8AJNGU7JJ4&dib=eyJ2IjoiMSJ9.vpQs928uj7OzIAi8CmBvPvRUPQERB3tPpvrE2vkgKFWhG7aU7eFu7nqi5APOEDtGxZRQ_eTYUarDFbLZnO0WponG-_LYPweI--oVhIlnpF6OBWuZJuLKbKAdUoz09T9KTo7y3nOc0zNSSAO72pbFdyCYTXQ97MSq71I1BneIMAYR76TpHB-mR7T5iKzipBMHcamWFKpczHsuvROh3bZzNX1ydsS4lKQxBGI_UO3YEzIseoLcHiwDHD7qGMf62jGUStXFk915r3WOzFr--uO_KjloCnd_4NUJutxoVFyd4fw.FOZcenUP-SG7DkxkxY8WiUzsWUgIpEPwvRHKPtuZQho&dib_tag=se&keywords=%E5%AE%9F%E8%B7%B5Next.js&qid=1715821487&sprefix=%E5%AE%9F%E8%B7%B5next.js%2Caps%2C179&sr=8-1)を読み直していくのでまとめ
- 読んだら追記していく
- 第5章はサンプルアプリケーションの解説なのでスキップ

## 第1章 Next.jsの基礎
### Route定義に関わる用語
#### Tree
- 階層構造を示す規則

#### Subtree
- 任意のノードをRootとして始め、Leafで終わるTree

#### Root
- TreeまたはSubtreeにおける最初のノード

#### Leaf
- 子のない最後のノード

#### Segment
- スラッシュで区切られたURLの一部
- Segmentが複数存在する場合、ある一つのSegmentから見て「親Segment」や「子Segment」といいった呼び方をする

#### Path
- ドメインの後に続くURL文字列

#### Route Segment
- 特定のPathに対応するSegment
- Segmentを構成するためにappディレクトリにフォルダをネストして階層化する
- Route Segmentの中の一番上のSegmentをRoot Segmentと呼び、子Segmentを持たないSegmentをLeaf Segmentと呼ぶ

#### Dynamic Route
- Pathが動的に変わりうつもの、URLパスパラメーターを参照する全体のRoute
- ネストするフォルダ名に[]を含めることで構成できる
- Dynamic Routeを構成する[]が含まれたSegmentのことをDynamic Segmentと呼ぶ

### ナビゲーション
#### ハードナビゲーション
- ブラウザが画面を再読み込みする画面遷移

#### ソフトナビゲーション
- ブラウザが画面を再読み込みしない画面遷移
- Next.jsのSPAナビゲーションはこっちで、必要な箇所だけを再レンダリングするため、ナビゲーション体験は基本的に高速で快適
- App Routerでは、変更されたRoute Segmentのみが再レンダリングされる
	- 一度ブラウザで取得した値やレンダリングした画面をキャッシュすることで、画面をリロードするまで保持できる利点もある

#### レイアウトのネスト
- App Routerでは、親Segmentの中に子Segmentがネストされる
	- 子Segmentは親Segment Layoutの影響を受ける
- SegmentごとにLayoutファイルを設けられるので、特定のRoute、特定のSubtreeでのみ適用される共通UIを簡単に実装できる
	- タブやサイドバーなど

## 第2章 Server Componentとレンダリング
### Server Componentの概要
- SSR（サーバーサイドレンダリング）において、サーバーサイドでのみ実行されるコンポーネント
- async functionを用いて非同期関数として定義できる
	- コンポーネント上から直接、外部WebAPIのデータを取得してレンダリングできる
- App Router内で実装されるReactコンポーネントは、何も宣言しなければデフォルトでServer Componentとして扱われる
- 完全にサーバーサイドでのみ実行されるため、コンポーネントのJSファイルがブラウザに送られることがない

### Client Componentの概要
- ブラウザ/サーバー両方で実行されるコンポーネント
	- Webアプリケーションにおいて、ユーザーの何らかの操作に対してインタラクティブに反応する振る舞いを設定するためにはブラウザで実行されるJavaScriptが必要
	- 任意のDOMにイベントハンドラーをアタッチすることでUIはインタラクティブになる
		- この一連のプロセスをハイドレーションと呼ぶ
	- Client Componentのようなブラウザ/サーバー両方で実行されるコンポーネントが必要なのは、画面をハイドレーションする必要があるから
- コンポーネントファイルの先頭で`"use clinet"`ディレクティブを記述することでClient Componentになる
	- `"use client"`境界をまたいだ先のコンポーネントコードは、インタラクションに必要な処理が含まれていると判断され、ブラウザ向けにバンドルされて送られる
- Client Componentからimportされるコンポーネントや関連ファイルもブラウザ向けにバンドルされる

### Server ComponentとClient Componentの使い分け
#### Server Componentを使うべきケース
- データを取得する
- バックエンドリソースを取得する
- 機密情報を扱う

#### Client Componentを使うべきケース
- インタラクティブな機能を持つ
- コンポーネントに保持した状態を使う
- ブラウザ専用APIを使用する
- ブラウザ専用のHooksを使用する
- React Classコンポーネントを使用する

#### Client Componentになるタイミング
- ファイルの先頭で`"use client"`を宣言する
- Client Componentにimportされたコンポーネント

### Server Componentのデータ取得
- 以下のようなセキュリティおよびパフォーマンス観点から、データ取得は可能な限りServer Componentを使用することが推奨されている
	- バックエンドリソースへ直接アクセスできる
	- アクセストークンやAPIキーなどの機密情報がクライアントサイドに漏洩することを防げる
	- クライアント・サーバー間における通信の往復回数が削減される
	- クライアント・サーバー間におけるウォーターフォールが削減される
- Pageファイルだけではなく、Layoutファイルやそれらの子となるServer Componentでもデータを取得できる
- Next.jsでは標準のデータ取得方法として、Web標準APIのfetch関数を使う
	- 少ない設定で最適なデータ取得ができるようにNext.js内部で拡張されている
- コンポーネントに直接データ取得関数を書かず、別ファイルで定義したデータ取得関数をコンポーネントでimportして使うのがおすすめ
	- どういったデータを取得しているのかがわかりやすい
	- リクエストのメモ化の考慮漏れが起こりづらい
- Promise.allを使用すると、並列でデータを取得できるので、直列で取得するよりもおおむねレスポンスが速くなる

### 動的データ取得と静的データ取得
#### 動的データ
- 頻繁に更新されるデータ
	- 閲覧履歴
	- 商品在庫数

#### 静的データ
- 更新頻度が低く、誰もが共有できるデータ
	- 商品概要
	- ブログ記事
	- ニュース記事

### Next.jsのfetch関数
- Next.jsにおいてfetch関数はフレームワークレベルで拡張されている
- 何もキャッシュ設定を指定しなければ、静的データ取得と扱われて結果がキャッシュされる
	- データ取得結果のキャッシュを作成することで、データソースへのアクセスを減らし、レスポンスの高速化に寄与するため
- 動的データ取得にするには `{ cache: "no-store" }`を指定する
	- 閲覧履歴などの動的データは、都度最新のレスポンス結果でないと困るため
	- この設定をすることでキャッシュの作成を行われず、リクエストのたびにデータ取得が発生する
- Next.jsのキャッシュは、 `next dev`による開発環境だと正確な挙動を把握できない
	- 正確な挙動を把握するためには、 `next build`で一度ビルドし、 `next start`で本番環境としてNext.jsアプリケーションを起動する必要がある

### Routeのレンダリング
- Next.jsはRouteごとにレンダリング手法を選べる
- Next.jsはビルド時に、すべてのRouteにおいてレンダリングを試行する
- レンダリング手法は大きく分けて2通りある

#### 静的レンダリングRoute
- すべてのリクエストに対して、同一のレンダリング結果をレスポンスする
- レンダリング結果をHTMLなどの静的ファイルに出力しておき、レスポンスとして使用する
	- ユーザーと地理的に近いCDNから配信できることを意味する
	- 最もパフォーマンスの高い配信方法のうちの一つ
- このアプローチはSSG/ISRと呼ばれている
	- SSG : 全ページをビルド時に静的に生成し、高速な読み込みと簡単なホスティングが特徴
	- ISR : ビルド時に生成された静的ページを必要に応じて再生成するので、動的データの更新が容易
- 静的レンダリング結果がキャッシュファイルに出力されるのは以下のタイミング
	- ビルド時
	- リクエスト時
	- Revalidate時

#### 動的レンダリングRoute
- リクエスト内容を都度評価し、その内容に応じてレンダリング結果を動的に変えるRoute
- リクエスト内容を都度評価する必要があるため、基本的にパフォーマンス面では静的レンダリングRouteに劣る

### 静的/動的レンダリングRouteの判別方法
- Next.jsアプリケーションをビルドした際、正常にビルドが完了するとRoute一覧が表示され、行頭にアイコンが表示される
	- ○アイコンは静的レンダリングRouteを表し、λアイコンは動的レンダリングRouteを表す

### 静的/動的レンダリングの切り替え
- 静的/動的レンダリングの切り替えは実装者が意識的に行うものではない
- Next.jsは基本的にすべてのRouteを静的レンダリングしようと試みる
- Routeの実装に以下の要因が含まれる場合に静的レンダリングは不可能と判断され、Routeは動的レンダリングとして提供されることに成る
	- 動的データ取得の使用
	- 動的関数の使用
	- Dynamic Segmentの使用

#### 動的データ取得の使用
- 動的データ取得が含まれていると動的レンダリングになる

#### 動的関数の使用
- 動的関数とは、HTTPリクエストの内容を参照する関数のこと
	- Server Componentで `cookies()`を使用してCookieを参照する
	- Server Componentで `headers()`を使用してリクエストヘッダーを参照する
	- Server ComponentでPropsのsearchParamsを利用してURL検索パラメーターを参照する
	- Client Componentで `useSearchParams()`を使用してURL検索パラメーターを参照する
- 動的関数が含まれていると動的レンダリングになる

#### Dynamic Segmentの使用
- Dynamic Segmentが含まれるRouteはprops.paramsを参照することでパスパラメーターを参照できる
	- searchParamsと同様に不特定多数のパスパラメーターが送られてくるので、動的レンダリングになる

## 第3章 App Routerの規約
### Segment構成ファイル
- Route Segmentを構成するファイルは複数あり、それぞれ定められた名称のファイルを適切に配置することで有効になる

| 名称        | 拡張子           | 用途                       |
| --------- | ------------- | ------------------------ |
| layout    | .js .jsx .tsx | Segmentとその子の共有UI         |
| page      | .js .jsx .tsx | Segment独自のUI             |
| loading   | .js .jsx .tsx | Segmentとその子の読み込みUI       |
| not-found | .js .jsx .tsx | Segmentとその子の404UI        |
| error     | .js .jsx .tsx | Segmentとその子のエラーUI        |
| route     | .js .ts       | サーバーサイドAPIエンドポイント        |
| template  | .js .jsx .tsx | 再レンダリングされるレイアウトUI        |
| default   | .js .jsx .tsx | Parallel routeのフォールバックUI |

- Segment構成ファイルは、React Suspenseを前提としたコンポーネントツリーを構築する
- エラー発生時の表示は「Error Boundary」に委ねる
- Route Segmentを元に構築されるコンポーネントツリーのうち、 `<Suspense />`と `<ErrorBoundary />`は自動で挿入される
- 子Segmentは親Segmentのコンポーネント内にネストされる

#### React Suspense
- 子コンポーネントが読み込みを完了するまでフォールバックUIを表示する機能
- コンポーネントツリーの一部を表示する準備が出来ていない場合に、その部分の読み込み状態を宣言的に指定できる

#### Error Boundary
- 子コンポーネントでErrorがスローされた場合にフォールバックUIを表示する機能
- try...catch文のcatchブロックに似たような働きをする

### 各構成ファイルの役割
#### loading
- `<Suspense />`の上に構築できる簡易的なローディングUIを提供する
- Server Componentだけでなく、Client Componentとしても使用できる
- Propsは指定できないので、Segmentのデザインに応じて必要な実装を施す
	- ローディングスピナーのようなUIが一般的

#### not-found
- データ取得や画面提供の際にリソースが見つからなかった旨を通達する404 UIを提供する
- 関連するSegmentから `notFound()`が実行された際に表示される
- page.tsxやlayout.tsxで通常WebAPIを経由してデータを取得するので、その際にリソースが見つからなかった場合に慣習的に `notFound()`を実行する
- `notFound()`を実行すると例外がスローされ、内部的に例外をcatchするようにNext.jsは設計されている

#### error
- データの取得や画面提供の際に例外が発生した場合、エラーを通達するUIを提供できる
- `"use client"`ディレクティブを宣言してClient Componentとして定義する必要がある
- layout.tsxで発生した例外はその親階層にあるerror.tsxで処理されることに注意

#### route
- 画面ではなく、WebAPIを提供するためのファイル
- App Routerで実装するWebAPIのことをRoute Handlerと呼ぶ
	- Pages Routerで提供されていたAPI Routesと似たような機能

#### default
- Parallel Routesという機能のために使用するファイル

#### global-error
- Segment単位で設置しないファイル
- 他のerror.tsxでハンドリングされなかった例外が発生した場合の画面を定義する
- `"use client"`ディレクティブを宣言してClient Componentとして定義する必要がある

### Segment構成フォルダ
- App Routerでは、フォルダ名の命名規則でSegment構成をコントロールする

#### Dynamic Route Segment
- パスに含まれるパラメーターを参照できる
- フォルダ名称を[slug]のように[]で囲むことで、パスパラメーターに応じた画面を提供できる
	- LayoutファイルやPageファイルで、URLリクエストのうち[slug]にあたる文字列をパスパラメーター値として参照できる

| Route                    | URLリクエストの例 | params        |
| ------------------------ | ---------- | ------------- |
| app/blog/[slug]/page.tsx | /blog/a    | { slug: 'a' } |
| app/blog/[slug]/page.tsx | /blog/b    | { slug: 'b' } |
| app/blog/[slug]/page.tsx | /blog/c    | { slug: 'c' } |

#### Catch-all Segment
- [...slug]という命名規則でフォルダを定義すると、ネストされたPathに対応して配列としてパスパラメーターを参照できる

| Route                       | URLリクエストの例  | params                    |
| --------------------------- | ----------- | ------------------------- |
| app/shop/[...slug]/page.tsx | /shop/a     | { slug: ['a'] }           |
| app/shop/[...slug]/page.tsx | /shop/a/b   | { slug: ['a', 'b'] }      |
| app/shop/[...slug]/page.tsx | /shop/a/b/c | { slug: ['a', 'b', 'c'] } |

#### Optional Catch-all Segment
- [[...slug]]という命名規則でフォルダを定義する
- Catch-all Segmentとの違いは、一行目の/shopのリクエストにも応じる点

| Route                         | URLリクエストの例  | params                    |
| ----------------------------- | ----------- | ------------------------- |
| app/shop/[[...slug]]/page.tsx | /shop       | {}                        |
| app/shop/[[...slug]]/page.tsx | /shop/a     | { slug: ['a'] }           |
| app/shop/[[...slug]]/page.tsx | /shop/a/b   | { slug: ['a', 'b'] }      |
| app/shop/[[...slug]]/page.tsx | /shop/a/b/c | { slug: ['a', 'b', 'c'] } |

#### Route Groups
- フォルダの一部をURL Pathから除外したい時、フォルダ名称を(feature)のように()で囲むことで、Route Groupsとして識別される
- 利用者画面、管理画面のように用途ごとに異なるLayoutを適用したいケースなどでRoute Groupsを活用できる
- Route Groupsを適用したフォルダ配下にlayout.tsxを配置すれば、その配下のSegmentにだけ特定のlayout.tsxを適用させられる

#### Private Folderとコロケーション
- 特定機能の関連ファイルをまとめることをコロケーションと呼ぶ
- appディレクトリ内のフォルダ名称の先頭にアンダースコアをつけると、そのフォルダに含まれるすべてのファイルをルーティング対象外に出来る
	- アンダースコアを接頭辞に持つフォルダをPrivate Folderと呼ぶ
	- `_components`という名称のフォルダは、慣例的に閉じられたスコープに必要なコンポーネントの格納先であることを示す

#### Parallel RoutesとIntercepting Routes
- Parallel RoutesとIntercepting Routesを使用すると、ソフトナビゲーションを活用したUIを実装できる
- Parallel RoutesはSlotと呼ばれる、@folderというフォルダ名規約を使用したフォルダを使用して作成する
	- @folderというフォルダ名規約を使用したフォルダはSegmentではなくなり、その存在がURLに影響されなくなる
	- Slotは、LayoutにおいてReact Componentのchildrenと同じようにPropsとして渡され、子ノードとしてレンダリングされる
	- @folderをRootとしたSubtreeと、表示しているRoute Segmentが一致したタイミングでPropsに渡されたSlotがレンダリングされる
	- 一度表示されたあとは、一致しないRoute SegmentにソフトナビゲーションしてもSlotは表示され続ける
	- default.tsxは、一致するRoute Segmentが表示されるまでの間、Slotに表示されるフォールバックUI
- Intercepting RoutesはRouteをインターセプトするためのRoute定義
	- 通常定義されたSegmentをインターセプトし、このフォルダ内に格納された異なるSegment定義を画面として提供する
	- ソフトナビゲーションでモーダル画面を表示する、といったUI実装が可能になる
- 相対パス規則に似た（..）規則で定義する

| 規則       | インターセプトするSegment |
| -------- | ---------------- |
| (.)      | 同じレベル            |
| (..)     | 1つ上の階層           |
| (..)(..) | 2つ上の階層           |
| (...)    | appディレクトリのroot   |

- インターセプトが発動する条件はソフトナビゲーション時のみ
- 画面をリロードするとハードナビゲーションとなるので、インターセプトは発生せずに通常定義されたSegmentが画面として提供される

### Routeのメタデータ
- headタグ内のtitle要素やmeta要素などのSEO観点で重要なメタデータを定義するAPIが用意されている

#### 静的メタデータ
- 固定のメタデータを出力する
- 任意SegmentのPageファイルやLayoutファイルからmetadataオブジェクトをexportすると、レンダリングされるHTMLに適切なメタデータが出力される

#### 動的メタデータ
- リクエスト内容に応じてメタデータを出し分ける
- 任意SegmentのPageファイルやLayoutファイルから、非同期関数であるgenerateMetadata関数をexportする
- Server Componentと同じようにデータを取得でき、取得したデータのテキストを出力する
- 画面リクエストに対して同じ関数が2回呼ばれているが、Next.jsが内部的にRequestのメモ化を行って同じとみなせるデータ取得を1つにまとめてくれているため、WebAPIサーバーの負担が増えることはない
	- 同じとみなせないデータ取得はそれぞれ別のリクエストとして扱われる

#### メタデータの継承
- Next.jsでは、親SegmentからRoot Segmentまでをたどり、自Segmentのメタデータと結合してくれる
- Root Layoutでメタデータを設定した場合、すべての子孫Segmentにおいて何もメタデータが設定されていなければ、自動でRoot Layoutのメタデータが採用される
	- SEO観点で言えば、個別のRouteで異なるメタデータを設定することが望ましい

## 第4章 Route Handler
### Route Handlerとは
- App RouterでWebAPIを実装する方法
- Route Handlerが施されているRouteにリクエストが発生すると、JSONなどのHTML以外のレスポンスを返却する
- ブラウザ/サーバー間だけでなく、サーバー/サーバー間でのデータのやり取り、つまり外部APIとして使ったり、マイクロサービスアーキテクチャにも活用できる
- Route Handlerを定義するには、Segment構成フォルダにroute.tsというファイルを配置する
	- app/api/hello/route.tsというRoute Handler定義ファイルは、/api/helloのリクエストを処理する
- route.tsファイルからexportする関数は、それぞれHTTPリクエストのメソッドに対応している
	- HTTPリクエストのメソッドに対応する関数がexportされていない場合、Next.jsは405 Method Not Allowedレスポンスを返す
- Route HandlerはWeb標準のFetch APIに準拠していて、関数が返すNextResponseはFetch APIのResponseに基づいている
	- 関数の第1引数でFetch APIのRequestに基づくNextRequestを受け取ることもできる
- App Routerはファイルの配置場所によってはPageファイルとAPIファイルでコンフリクトを起こす
	- Pages Routerの慣例にならってapp/apiというディレクトリを用意し、そこでRoute Handlerを定義しておけば上記のコンフリクトは発生しない

### Route Handlerのレンダリング
- Route Handlerは主にJSONをレンダリングし、画面のRouteと同様に静的レンダリング/動的レンダリングを切り替えることが可能
	- サーバーのログで○アイコンが付与されたRouteは静的Route Handler、λアイコンが付与されたRouteは動的Route Handlerとなる

#### 静的Route Handler
- JSONなどのレスポンスボディをあらかじめキャッシュファイルとして出力するので、リクエストのたびに外部WebAPIサーバーやDBからデータを取得する必要がない
	- Pages RouterのころのAPI Routesではリクエストのたびに評価してレスポンスを返していた

#### 動的Route Handler
- 画面のRouteと同様に、Route Handlerもビルド時にRouteのレンダリングを試行するが、この時に以下の要因が検出されると、Next.jsは該当のRoute Handlerを動的Route Handlerとみなす
	- Dynamic Segment値の参照
	- Requestオブジェクトの参照
	- 動的関数の使用
	- GETとHEAD以外のHTTP関数のexport
	- Segment Config Optionsの指定

### Route Handlerの使用例
- Route Handlerのもっとも代表的な使用方法として考えられるのが、ブラウザからのHTTPリクエストに対応するというケース
	- UIはユーザー操作に応じ、最終的にデータベースに操作内容を保存するため、fetch関数などを使用してWebAPIをコールする
	- 例えば、認証認可機能を備えたWebアプリケーションサーバー（Next.js）自身がWebAPIを備えている場合、Cookieを頼りにログインユーザー情報の参照をNext.js自身に委ねることができる

## 第6章 データ取得とキャッシュ
### fetch関数でのデータ取得
- Reactには、同じfetch関数のリクエストは、自動でひとつのfetch関数リクエストにまとめるという拡張が施されており、これをRequestのメモ化と呼ぶ
	- 動的なメタデータを生成するためのgenerateMetadata関数など、1回のレンダリングで同じデータ取得を行いたいケースはよくある
	- コンポーネント単位でデータ取得できるのはメリットだが、Requestのメモ化を怠ると過剰なデータソースアクセスとなってしまうので、共通関数として定義して処理をまとめるなど、あらかじめ対策を検討しておくことが重要

### fetch関数のキャッシュ
- Next.jsにはIncremental Cacheという組み込みのキャッシュシステムが存在する
	- データ取得をキャッシュし、必要に応じて更新するメカニズムを持つ
	- デフォルトで有効（Next.js15で変わるかも）
- App Routerにおける代表的なIncremental Cacheは、fetch関数によるDataキャッシュ
	- Next.jsではfetch関数を拡張していて、何も指定しなければ取得したデータをキャッシュする
	- fetch関数を使用したキャッシュは更新されない限り、キャッシュされた内容を返し続ける
		- 有効期間はデフォルトでは1年間
- 頻繁に更新する必要はないが、ずっと古いキャッシュデータのままでは困るようなケースでは、next.revalidateオプションをfetch関数で指定して、キャッシュの有効期間を秒数で設定する
	- next.revalidateオプションで設定した有効期間が過ぎるとキャッシュを破棄し、新たにデータを取得する
	- 再取得内容は再びキャッシュされ、有効期間が過ぎるまでキャッシュを返す
- キャッシュを更新するプロセスをRevalidate（再検証）と呼ぶ
	- next.revalidateオプションのように、有効期間で指定するRevalidateをTime-based Revalidationと呼ぶ
- キャッシュを活用すると、WebAPIサーバーへのアクセス頻度を削減できる
	- WebAPIサーバーの稼働量を削減できるだけでなく、データ取得の遅延が削減されるため、画面を提供する速度が向上する

### Prisma Clientでのデータ取得
- 同じようなリクエスト関数を一つにまとめるRequestのメモ化はfetch関数でのみ適用されるので、Prisma Clientのようなfetch関数以外のデータ取得方法ではデフォルトで有効にならない
- fetch関数以外でRequestのメモ化を実現するには、Reactのcache関数でデータ取得関数を囲う
	- cache関数はReactコンポーネントの中で使わないこと
	- データ取得関数の引数はプリミティブ値でなければならない
		- プリミティブ値の引数が全く同じであれば同じデータ取得であるとみなされ、Requestのメモ化が有効になるから
- generateMetadata関数を使用する画面では、特にRequestのメモ化に注意する

### Prisma Clientのキャッシュ
- fetch関数以外のデータ取得の場合、自動ではDataキャッシュは作成されない
- fetch関数以外でもDataキャッシュを作成したい場合は、unstable_cache関数を使用する
	- 負荷の高いデータベースクエリなどの結果をキャッシュし、複数のリクエストにわたって再利用できる
	- 実践Next.js執筆時点では不安定な機能、将来的に名前が変更されたり、引数などの仕様が変わる可能性がある
- Reactのcache関数と異なり、Next.jsのunstable_cache関数はコンポーネントの中で利用可能