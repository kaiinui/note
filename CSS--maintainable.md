Maintainable CSS
---

まともな CSS を書く。

Why?
---

CSS, 継承関係の把握とか、スタイルの汚染とか、命名規則とかガバガバすぎてまともに管理出来ない。

How?
---

- SMACSS https://smacss.com/

とかの CSS Convention に準拠して書く。

わりと複雑で導入しにくさはある。

実際的には？
---

* FLOCSS https://github.com/hiloki/flocss

これくらいが準拠しやすそう。ルールは

* Component - 再利用可能なコンポーネント
* Project - 複数のコンポーネントと諸処のスタイルから構成されるパターン。プロジェクトに固有のものだから Project.
* Utility - 色んな便利クラス

がトッブレベルにあり、命名規則として

* Block
* Element
* Modifier

の BEM システム。

Component と一括りにされがちなものを Component という再利用可能なものと、Project という再利用不可能なものに分けたのは良いアイデアと思う。

###命名規則

##### トップレベル

- `.c-xxx` - components
- `.p-xxx` - projects
- `.u-xxx` - utilities

##### BEM

- `.block__element--modifier`

###運用規則

安易なカスケードと依存関係を抑える。

- 原則として、モジュール間のカスケーディング、他のモジュールを親とするセレクタを用いたカスケーディングは禁止とします。
- Projectレイヤーモジュール内に、同一のComponentレイヤーモジュールが複数点在する場合を考慮し、出来る限り子供セレクタなどを活用して、適用範囲を限定する。
- 他のモジュールを親セレクタに持つのは1つだけとする。
