CI
===

iOS の CI やろうとして嵌ったのをメモ

xctools
---

なんか動かないし、`xcodebuild` で出来るので、

```
xcodebuild -workspace InPlaceEdit.xcworkspace -scheme InPlaceEditTests -destination 'platform=iOS Simulator,name=iPhone Retina (4-inch 64-bit)' test
```

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

- Xcode5環境で、xcodebuildを使ってユニットテスト実行する方法 - Qiita : http://qiita.com/k_kinukawa/items/6c5ef09ad86ac9c75a8c
- http://kaspermunck.github.io/2013/04/running-kiwi-specs-with-travis-ci/
