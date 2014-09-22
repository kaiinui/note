iOS での i18n
---

ベストプラクティスっぽいの。

1. `NSLocalizedString` をラップする
2. `.strings` は元ファイルをパースして生成する
3. 翻訳が出てるかテストする
4. Scheme を言語毎に作る
5. `.plist` の翻訳もテストする

### `NSLocalizedString` をラップする

```objc
// R.m

+ (NSString *)string_hello_world {
    return NSLocalizedString(@"HELLO_WORLD", @"");
}
```

pros: テストしやすい。キーがばらけない。リファクタリングしやすい。

### `.strings` は元ファイルをパースして生成する

キーがとんでもなく間違えやすいので、元ファイルを一括してキー対応させて生成する方が良い

### 翻訳が出てるかテストする

```objc
it(@"Label 'Hello World' should be translated.", ^{
    expect([R string_hello_world]).notTo.contain(@"HELLO_WORLD");
    expect([R string_hello_world]).notTo.contain(@"Hello World"); //if it is 'Hello World' in Base.strings
});
```

これで翻訳ちゃんと指定の場所に出てるかテスト出来る。
また、UI が崩れないようにテストするの、ラベルの長さを指定して、このようにしてもよい思う。

```objc
it(@"Label 'Hello World' should be not so long", ^{
    expect([R string_hello_world]).to.shorterThan(20);
});
```

### Scheme を言語毎に作る

以上はテスト毎に言語を設定すると手間が死ぬので自動化する。

Scheme を作り、`Arguments` を以下のように設定すればシミュレータがその言語で起動する

![](http://i.gyazo.com/f175f54ca1168b44e08a9c4923cb16f9.png)

これを言語分つくりやる。

### `xcodebuild` で自動化する

```
xcodebuild -workspace Post.xcworkspace -scheme PostTests-ja -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
xcodebuild -workspace Post.xcworkspace -scheme PostTests-en -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
...
```

実際的には for 文で回す。
