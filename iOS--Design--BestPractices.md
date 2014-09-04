iOS の設計 / クラス責務分担
===

使えるの

- Behavior クラス
- APIClient の Service への分離. (NSNotificationCenter で Pub-Sub 的やり取り)
  * 何故か？ViewController は LifeCycle が複雑なため。短命。ViewController に View だけで閉じない操作（たとえば永続的な操作）を含めるのはダメ。
- 独自 View への分離 (View に自分自身の面倒を見てもらう。Gesture とか。)
- Datasource の分離
- Delegate の分離
- GestureDetector の分離
- JLRoutes の導入 (ルーティング / パラメータ渡しの統一)
- rdotm の導入 (まあ。typo によるミスの防止)
- RAC の導入 (MVVM, Model-View binding)
- Realm の導入 (Model)

とにかく ViewController に全てを書くのをやめる。

references
---

- [BehaviorクラスによるiOSアプリケーションの設計](http://tech.vasily.jp/ios_behavior_pattern)
- [iOS/Androidアプリエンジニアが理解すべき「Model」の振る舞い](http://www.slideshare.net/mokemokechicken/iosandroidmodel)
- [JLRoutes](https://github.com/joeldev/JLRoutes)
- [Apple Templates Considered Harmful](http://modocache.svbtle.com/apple-templates-considered-harmful)
