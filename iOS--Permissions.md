Prepermissions & Rescue permissions
---

iOS, 権限の認証が個別なのでうざいことになりやすい。ベストプラクティスをまとめたい。

1. 必要になったときに認証を要求する
2. 適切な文言を付ける
3. Per-permission する
4. Denied のときの Rescue をする
5. AniGIF て案内が提供されていれば尚良いと思う

![](https://dl.dropboxusercontent.com/u/7817937/_github/BwYPsj5IUAAc7dF-1.png)

![](https://dl.dropboxusercontent.com/u/7817937/_github/img_20140905_130921.jpg)

トリビア
===

文言を変える
---

`<appname>-Info.plist` の `NSPhotoLibraryUsageDescription` を弄ることで、「カメラロールへのアクセスを許可しますか」の定型文の下に、自分の書いた文章を出せる。

何故アクセスが必要かの説明があれば、安心して許可しやすい。

<img src="https://raw.githubusercontent.com/AquaSupport/AQSPermissionsLib/master/SS_2.png" width="240px" />

こんなかんじ

Permission を何回もテストしたい
--

Bundle Identifier をイイ感じに変えまくれば別アプリとして認識してくれる。

Bundle Identifier を好きに変えてテスト出来ない通知だけはうまくいかないので、これを参照。

http://kenmaz.hatenadiary.jp/entry/20110826/1314376887

References
---

- https://developer.apple.com/library/ios/documentation/general/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html
- http://kenmaz.hatenadiary.jp/entry/20110826/1314376887
- デバイスのアクセス許可を求めるUI | Reflection | UIデザイン会社Standard Incのブログ : http://www.standardinc.jp/reflection/article/ui-to-seek-the-permission-of-the-device/
