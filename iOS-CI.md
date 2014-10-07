CI
===

iOS の CI やろうとして嵌ったのをメモ

xctools
---

なんか動かないし、`xcodebuild` で出来るので、

```
xcodebuild -workspace InPlaceEdit.xcworkspace -scheme InPlaceEditTests -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
```

指定出来るリストは

```
OS:7.1, name:iPad 2
OS:8.0, name:iPad 2
OS:7.1, name:iPad Air
OS:8.0, name:iPad Air
OS:7.1, name:iPad Retina
OS:8.0, name:iPad Retina
OS:7.1, name:iPhone 4s
OS:8.0, name:iPhone 4s
OS:7.1, name:iPhone 5
OS:8.0, name:iPhone 5
OS:7.1, name:iPhone 5s
OS:8.0, name:iPhone 5s
OS:8.0, name:iPhone 6 Plus
OS:8.0, name:iPhone 6
OS:8.0, name:Resizable iPad
OS:8.0, name:Resizable iPhone
```

以下は古い情報。（iOS 7.1 と Xcode 5.1）

```
iPhone Retina (3.5-inch)
iPhone Retina (4-inch)
iPhone Retina (4-inch 64-bit)
iPad
iPad Retina
iPad Retina (64-bit)
```

Scheme
---

左上の `プロジェクト名` を押して、`Manage Scheme` を押す。

![](https://dl.dropboxusercontent.com/u/7817937/_github/__Film/__train_activate_travis/1.png)

`プロジェクト名` の Shared を ON にする。

![](http://i.gyazo.com/e8982094ab941bdcdde1153553a74895.png)

以下のようになっているのを確認。

![](http://i.gyazo.com/84ae618bcabe79d811cee39ebdfe00f4.png)

.travis.yml
---

```
language: objective-c
notifications:
  email: false

before_install: gem install cocoapods

script:
  - pod install
  - xcodebuild -workspace InPlaceEdit.xcworkspace -scheme InPlaceEditTests -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
```

References
---

- Debugging XCTool’s NSInternalInconsistencyException: Failed while trying to gather build settings for your scheme : http://code.dblock.org/debugging-xctools-nsinternalinconsistencyexception-failed-while-trying-to-gather-build-settings-for-your-scheme
- Xcode5環境で、xcodebuildを使ってユニットテスト実行する方法 - Qiita : http://qiita.com/k_kinukawa/items/6c5ef09ad86ac9c75a8c
- http://kaspermunck.github.io/2013/04/running-kiwi-specs-with-travis-ci/
