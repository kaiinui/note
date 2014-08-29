Virtual DOM
---

### Virtual DOM + Web Worker で Web App は変わるか？

以前より、MVC + Web Worker の構想はあったが、DOM 関連で結局パフォーマンス損なわれる感じで実現し辛かった。

が、Virtual DOM を上手く使えばかなりの部分が Background に押し込められてしまうのでは？
UI スレッドで動く必要のあるものといえば、以下の 3 つくらい。

1. `requestAnimationFrame` で差分 DOM の更新（これは軽い）
2. DOM Event の受付と `postMessage()`
3. `localStorage`

Virtual DOM を使えば表でやることを最小限に抑えることが出来、相当軽くなるような気がする。
（TODO: DOM の部分でどの程度重いか調査）

UI スレッドでの責務（というか処理）を極限まで減らすことで、 testability も相当上がる。と思う。

FAQ?
---

#### 通信？

XHR は Web Worker 対応。

#### 複数スレッドでつらみありそう？

UI Thread + Background 1 つだから Android とかでよくやる UI + Model Service と同じパターン。

`postMessage()` (スレッド間の協調)が必要な部分はおそらくたった二つしかない

1. Virtual DOM に変更があったとき、変更点を UI Thread に `postMessage()`
2. 観測が必要な DOM Event が起こった時、このイベントの中身を Background に `postMessage()`

#### 対応ブラウザ？

http://caniuse.com/#feat=webworkers

Android Browser が 4.3 以下未対応なことを除けば良し。 4.3 以下…

Global で 77.31% のシェアがある。まだまだ辛い。

#### 対応フレームワーク？

無し。

https://github.com/Raynos/mercury をイイ感じに弄ることで実現出来る気がする。

References
---

- http://efcl.info/2014/08/28/mercury/
- https://github.com/Raynos/mercury
