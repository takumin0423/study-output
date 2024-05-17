## これはなに
- [実践Next.js —— App Routerで進化するWebアプリ開発](https://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Next-js-%E2%80%94%E2%80%94-App-Router%E3%81%A7%E9%80%B2%E5%8C%96%E3%81%99%E3%82%8BWeb%E3%82%A2%E3%83%97%E3%83%AA%E9%96%8B%E7%99%BA-%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E9%81%B8%E6%9B%B8-ebook/dp/B0CW1KC9N8/ref=sr_1_1?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=LT8AJNGU7JJ4&dib=eyJ2IjoiMSJ9.vpQs928uj7OzIAi8CmBvPvRUPQERB3tPpvrE2vkgKFWhG7aU7eFu7nqi5APOEDtGxZRQ_eTYUarDFbLZnO0WponG-_LYPweI--oVhIlnpF6OBWuZJuLKbKAdUoz09T9KTo7y3nOc0zNSSAO72pbFdyCYTXQ97MSq71I1BneIMAYR76TpHB-mR7T5iKzipBMHcamWFKpczHsuvROh3bZzNX1ydsS4lKQxBGI_UO3YEzIseoLcHiwDHD7qGMf62jGUStXFk915r3WOzFr--uO_KjloCnd_4NUJutxoVFyd4fw.FOZcenUP-SG7DkxkxY8WiUzsWUgIpEPwvRHKPtuZQho&dib_tag=se&keywords=%E5%AE%9F%E8%B7%B5Next.js&qid=1715821487&sprefix=%E5%AE%9F%E8%B7%B5next.js%2Caps%2C179&sr=8-1)を読み直していくのでまとめ
- 特に重要だと感じた点を記載する

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

