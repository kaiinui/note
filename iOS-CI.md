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

`InPlaceEdits` みたいな Scheme を作る必要有る。（しなくとも、`InPlaceEdit` でテスト出来る？）

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
