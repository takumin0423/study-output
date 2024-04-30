## これはなに
- [Node学園 42時限目 Next.js AppRouterについて](https://nodejs.connpass.com/event/315443/)に参加したのでレポートまとめ
- 重要な箇所だけまとめる

## ServerAction で Progressive Enhancement はどこまで頑張れるか？ / takepepe
### Progressive Enhancement
- Progressive Enhancementとは？
	- 可能な限り多くのユーザーに不可欠なコンテンツと機能のベースラインを提供することを中心とした設計哲学
	- [MDN](https://developer.mozilla.org/ja/docs/Glossary/Progressive_Enhancement)
	- Server ComponentとServer Actionsでより身近になった
	- JSオフでも機能するFormなど
- Progressive Enhancement対応はどこまでやるべきか？
	- 実装アプローチが根本的に異なるので、線引きが必要
	- 要件定義が細かく決められているプロダクト開発の場合、合意形成が必要
		- どのように明文化するのか？
		- テストにおいてどのように検証するのか？
	- Progressive Enhancementは現状ベストエフォートではある
		- Server Actionsを積極的に活用できるのであれば挑戦してみる価値はある

## Next.js App Router 採用の振り返り / quramy
### App Routerに対する所感
- ポジティブ
	- Data Fetching関連について、従来よりも直感的な実装が可能
	- BFFのためのAPIエンドポイントなどの中間成果物にかかる設計/実装コストが削減
	- チューニングを行っていない状態でも、十分に高速なアプリケーションを作成できた
- ネガティブ
	- 周辺のエコシステムがRSCに追いついていない
		- とはいえニーズはあるので、時間が解決するとは思う

### 全体的なアーキテクチャ
- インフラ : AWS(EKS)
- バックエンドAPI : Spring Boot
- フロントエンド : Next.js（メインユースケースはSSR）
- Data FetchingはServer Actions
	- データの表示と更新ともにServer Actionsで完結する
	- Server ComponentもServer ActionsもReactの使い方でしかないので、Next.jsが廃れたとしても応用が効く
- スタイリングはCSS Modulesを採用
	- Runtime CSS in JS系が軒並みRSCで動作しなかった（2023.6時点）
	- 他パッケージも対応が進んでいるので、現状だと別の選択肢もあり
	- ビルドツールチェーンが過度期であることを考えると、PandaCSSのようなトランスパイルに侵襲しないアプローチのものを選ぶといいかも
- 状態管理ライブラリにはjotaiを採用
- Formライブラリは採用なし
	- 画面入力要件がラジオボタンやチェックボックスのみで、そもそもライブラリが不要なレベルだったため
- UIコンポーネントライブラリも採用なし
	- 選定している余裕がなかったという消極的な理由
	- 何か良いものがあれば採用したい
- キャッシュ戦略について
	- Data CacheとFull Route Cacheは使わない
		- プロダクトの要件上、頑張ってもあまり旨味がない
- Cacheの利用に消極的であってもそこそこ実用的なパフォーマンス
	- 同領域でのPages Routerなプロダクトと比較すると、最初のJS読み込み周りでは圧倒的に改善できた
- 粒度の細かいテストはJest + React Testing Library
- 粒度の粗いテストはPlaywrightによるE2E
	- Next Serverの挙動と密に連動する箇所はE2E側で動作を担保するようにしている

### App Routerに取り組む上で重要なこと
- RSCのメンタルモデルを理解する
	- [uhyoさんの記事がおすすめ](https://zenn.dev/uhyo/articles/react-server-components-multi-stage)
- データフェッチの粒度を考える
	- リソース志向のAPIと親和性が高い
- HTML/CSSの知識
	- 不必要なCCを削減する上でも重要
	- CSSで実現できるのであれば無理にJSを使わない
		- カルーセルっぽい見た目のコンポーネントなど

## 感想
- オフライン参加者のほとんどがApp Routerを使ったことがあるそうで、だいぶ浸透してるんだなという印象
- App Routerは事前知識として要求されているものが多い && 難易度が高い、採用するのであれば開発に携わるフロントエンドメンバーが周辺の知識含めて色々と理解しておく必要がありそう
	- フロントエンド界隈自体が技術の進歩とトレンドの移り変わりが早いので、そもそも継続的な勉強が特に必要な領域ではある
- quramyさんの技術検証は2023/6時点でのものということもあり、現状だと周辺エコシステムがどれくらい追従できているのかも気になる
- Progressive Enhancementはそこまでこだわらなくてもいいのかなと感じた
	- そもそもブラウザでJSオフにする機会ってある？