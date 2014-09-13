CI
===

iOS の CI やろうとして嵌ったのをメモ

xctools
---

なんか動かないし、`xcodebuild` で出来るので、

```
xcodebuild -workspace InPlaceEdit.xcworkspace -scheme InPlaceEditTests -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
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
