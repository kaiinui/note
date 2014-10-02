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
- コミュニケーションパターン (Communication Patterns - Foundation - objc.io issue #7 : http://www.objc.io/issue-7/communication-patterns.html)

とにかく ViewController に全てを書くのをやめる。

妄想
---

VIPER のやつを参考に。

- `Router` - ViewController の instantiate と画面遷移
- `Decorator` - View を Decoration する. (`view.layer.cornerRadius = 12.0f;`)
- `ViewFactory` - View を作り、初期化する
- `ViewController` - 
- `Interactor` - 操作を担当。「アルバムを作る」 => `AlbumInteractor`
- `Presenter` - View を操作するロジック (DataSource も delegate もここが持つ。)
- `Repository` - データを要求する。全てのデータはここを経由して取り出される。`NSNotification` で通知。
- `Entity` - NSManagedObject
- `Service` - ViewController のライフサイクルに依存しないサービス。Singleton である `ServiceLocator` を用いて retain される。サービスはビジネスロジックのことを知らずに、与えられた命令を只実行する。汎用コンポーネントでなければならない。
  * `Operation` - サービスが何かしらやる場合は、`NSOperation` を用いることが推奨される

### 命名規則

- Storyboard は分割する
- `SomeViewController` に対して Storybaord 名は `SomeViewController.storybaord` であり、 identifier は `SomeViewController`
- `SomeView` に対して、`.xib` は `SomeView.xib`

references
---

- [Architecting iOS Apps with VIPER](http://www.objc.io/issue-13/viper.html)
- [BehaviorクラスによるiOSアプリケーションの設計](http://tech.vasily.jp/ios_behavior_pattern)
- [iOS/Androidアプリエンジニアが理解すべき「Model」の振る舞い](http://www.slideshare.net/mokemokechicken/iosandroidmodel)
- [JLRoutes](https://github.com/joeldev/JLRoutes)
- [Apple Templates Considered Harmful](http://modocache.svbtle.com/apple-templates-considered-harmful)
