オレオレ Effective Objective-C
===

- public な Method, Property を減らす。
- Interface を分割する（あるいはカテゴリでも良い。）
- Instance variable ではなく無名カテゴリでの Property を使う
- メソッドを細かく分ける
- `#pragma mark` でメソッド群をこまめに区切る。
- クラスを役割毎にカテゴリで分割する。
- Collection クラスはコメントで中の型を明示する e.g. `NSArray /* <NSString *> */ *hogeArray;`
- カテゴリで実装したメソッドには適切な prefix を付ける（「どのカテゴリのメソッドか」を明示化出来る）
- KVO を利用する。(`KVOController`)
- モジュール化し、CocoaPods で利用する
- .h ではむやみに `#import` せず `@class` で前方宣言する
- 循環参照は `libextobjc` で避ける
- `NSNotificationCenter` を避ける。
- appledoc, jazzy でドキュメンテーションする
- 「継承より合成」 Category + `objc_setAssociatedObject` で出来る。
  * `UIView` 関連を拡張したい時などは特に。(Reference の "iOS UI Component API Design" を参照)
  * Options クラスを提供するのも良いデザインになりやすい

番外編

- リソース
  - `R+String.m` で `NSLocalizedString()` をラップする
  - `R+Color.m` で `NSColor` をまとめる
  - `R+Number.m` で定数値をまとめる

UI 編

- `UIControl` はタップ認識領域を拡げる http://naoty.hatenablog.com/entry/2014/09/18/012246

Friend Methods
---

クラス間の連携で使うメソッドを protected っぽくしたい。

Protocol を分けるか、Category を使えば良い。

Category を使う場合は、`SomeClass+FriendMethods.h` とか。

Discussions
---

- view の取り扱い？
  * `Storybaord - instantiateViewControllerWithIdentifier:` は使う？
  * `.xib` は使う？
  * `.xib` からのロードはどうする？
  * `.xib` からロードしたとき、`IBAction` は？
- Runtime Attributes の取り扱い
  * 良いけど、コード外の動作が増えてしまう。

References
---

- http://modocache.svbtle.com/ios-ui-component-api-design
