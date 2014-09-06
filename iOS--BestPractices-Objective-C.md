オレオレ Effective Objective-C
===

- instance variable ではなく property を使う
- メソッドを細かく分ける
- `#pragma mark` をたくさん付ける
- クラスを役割毎にカテゴリで分割する
- カテゴリで実装したメソッドには適切な prefix を付ける（「どのカテゴリのメソッドか」を明示化出来る）
- `objc_setAssociatedObject` で Composition を実現する
- KVO を利用する。(`KVOController`)
- RAC を利用する。(`ReactiveCocoa`)
- モジュール化し、CocoaPods で利用する
- .h ではむやみに `#import` せず `@class` で前方宣言する
- `NSNotificationCenter` を多用しない
- appledoc, jazzy でドキュメンテーションする

番外編

- リソース
  - `R+String.m` で `NSLocalizedString()` をラップする
  - `R+Color.m` で `NSColor` をまとめる
  - `R+Number.m` で定数値をまとめる
- `Aspects.m` を上手く使う
