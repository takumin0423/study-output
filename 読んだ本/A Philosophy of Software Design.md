## これはなに
- 読んでいて重要だと感じた部分の読書メモ
- 読んだら追記する

## 第1章 序論（すべては複雑性との戦い）
- ソフトウェア開発における最大の制約は、開発者が自身の作り出しているシステムをどれだけ理解できるかという点にある
- 複雑性は、時間の経過とともに必然的に増加する
	- プログラムが大きくなればなるほど、開発に関わる人数が増えれば増えるほど、複雑性を管理することは難しくなる
- ソフトウェアをより簡単に開発できるようにし、より強力なシステムを低コストで構築できるようにするためには、ソフトウェア自体をよりシンプルにしなければいけない
	- 設計がシンプルであれば、複雑性が手に負えなくなる前に、より大規模で強力なシステムを構築できるようになる
- 複雑性に対抗する方法には、大きく分けて2つのアプローチがある
	- コードをより単純で明快なものにすることで、複雑性そのものを排除する
		- 特殊なケースを減らしたり、一貫性のある識別子を使用することで複雑性を低減できる
	- 複雑性をカプセル化し、開発者がシステム全体の複雑性に直面しなくても作業できるようにする
		- モジュール設計と呼ばれるアプローチ
		- ソフトウェアシステムを複数のモジュールに分割し、それぞれが独立性を保てるように設計することで、開発者は他のモジュールの詳細を深く理解せずとも作業を進められる
	- ソフトウェアは非常に柔軟なので、設計はシステムのライフサイクル全体にわたる継続的なプロセス
	- ソフトウェアは物理システムよりも本質的に複雑で、大規模なシステムの設計を完全に把握し、その影響を事前にすべて理解することは不可能
	- 現在の多くのソフトウェア開発プロジェクトでは、アジャイル開発のような漸進的アプローチを採用している
		- 最初に全体の機能のごく一部に焦点を当て、設計・実装・評価を行う
			- 元の設計に関する問題が発生されたらそれらを修正、このプロセスを繰り返す
		- 漸進的アプローチがソフトウェア開発に適している理由は、ソフトウェアは途中で大幅な設計変更が可能だから
- ソフトウェア開発の最大の課題は複雑性の管理で、それを克服するために設計の単純化とモジュール化が重要な役割を果たす
- 漸進的開発とは、ソフトウェアの設計が決して完了しないことを意味する
- ソフトウェア設計はシステムのライフサイクル全体を通じて継続的に行われるべきであり、開発者は常に設計上の問題について考える必要がある
- 本書を最大限活用するには、コードレビューと併用するのが最も効果的
	- 他人のコードを読む際に、本書で紹介する概念に照らし合わせて勧化、そのコードの複雑性との関係を見極める
- 設計スキルを向上させる最良の方法の一つは、危険信号を認識すること
	- 危険信号とは「そのコードが必要以上に複雑である可能性を示す兆候」のこと
	- コードを書く際には、危険信号を見つけたら一旦立ち止まり、その問題を解決できる別の設計を探すことを習慣にする
- 本書のアイデアを実践する際には、柔軟性を持ち、状況に応じた判断をすることが重要
	- すべてのルールには例外があり、すべての原則には限界がある
	- 美しい設計とは相反するアイデアや手法のバランスが取れているもの