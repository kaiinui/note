Prepermissions & Rescue permissions
---

iOS, 権限の認証が個別なのでうざいことになりやすい。ベストプラクティスをまとめたい。

1. 必要になったときに認証を要求する
2. 適切な文言を付ける
3. Per-permission する
4. Denied のときの Rescue をする
5. AniGIF て案内が提供されていれば尚良いと思う

<img src="https://dl.dropboxusercontent.com/u/7817937/_github/BwYPsj5IUAAc7dF-1.png" width="240px" /> <img src="https://dl.dropboxusercontent.com/u/7817937/_github/img_20140905_130921.jpg" width="240px" /> <img src="http://www.standardinc.jp/reflection/wp-content/uploads/2014/08/2014-08-27_content01.png" width="480px"> <img src="http://www.standardinc.jp/reflection/wp-content/uploads/2014/08/2014-08-27_content02.png" width="480px"> <img src="https://dl.dropboxusercontent.com/u/7817937/_github/slack_for_ios_upload%20%281%29.png" width="240px" /> <img src="https://dl.dropboxusercontent.com/u/7817937/_github/slack_for_ios_upload%20%282%29.png" width="240px" />

iOS8+ は
---

- iOS8で復活した設定画面へのURLスキーム - Qiita : http://qiita.com/Night___/items/3d689d657ee691d2cabe

iOS8 で「自分のアプリ」の設定への URL スキームが復活。コレを使うと「プライバシー」等とリンクがある設定ページに飛べる。
「プライバシー」から写真や通知等の Permission を弄れるので、iOS7 よりも Permission の復旧への導線を引きやすい。

### 参考

https://vine.co/v/OZBY7mPw7ej

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
