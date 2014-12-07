Android Development
===

iOSとの比較つき!Androidでこんなアプリ,こんな機能を作りたかったらこれを見ろ！作りたいアプリに対応するクラス、ライブラリのまとめ！ - Qiita : http://qiita.com/appwatcher/items/7d270de99d63bb9f2be4

Activity 簡単に死ぬ
---

Activity, 画面に紐づいてて画面に居る間は死なないイメージを普通の脳みそなら持つと思うけど、回転させるだけで一旦死んでまた作られる。
また、バックグラウンドに居る間にメモリ足りなくなったりすると勝手に殺される。

Activity とか Fragment の変数で状態管理するの気をつけないと簡単にフローが崩れる。

ちゃんと `onCreate(Bundle savedInstanceState)` と `onSaveInstanceState(Bundle outState)` をやっておけば変数は保たれる。

色々書いたりするの面倒くさいので、アノテーションでよろしくやってくれる [icepick](https://github.com/frankiesardo/icepick) を使うと良い。

書いた => http://qiita.com/kaiinui/items/8a6a7dddb9310f645da3

Background スレッドから UI を触ると死ぬ
---

結構な数の処理が Background スレッドで実行されることが前提とされている。（UI をロックしないように）
Callback 的なものはほとんど Background で走る。

ここから UI を触ると死ぬ。`Toast.makeText()` するだけで死ぬ。

UI を触るときは `runOnUiThread(Runnable)` を使う必要がある。相当たくさんの場面で出てくる。

GridView, HeaderView をサポートしていない
---

GridView なぜか HeaderView をサポートしていない。

じゃあ GridView の上に View を置いてやればいいじゃん。は間違い。

なぜなら GridView は `match_parent` で使うことを前提としていて、動的な高さを持つようには出来ていない。
GridView を `height:fill_content` でやろうとすると、中身を全部構築し、高さを知ってから描画、となってしまい画像の描画とかだと大変重くなる。

Google の公式 training コードでは、`< n` までの `getView()` で Header の view を返し、`>= n` なら要素の View を返す。

やれば分かるけど相当コードが汚くなる。 `position - numberOfColumn` とかたくさん出てくる。もちろん `numberOfColumn` 忘れると要素がズレる。

https://github.com/maurycyw/HeaderGridView こういうのを使うとよい。

`getViewById()` のキャストがキモい
---

`getViewById()`, `TextView` とかにキャストしまくっててキモい感じは有る。

https://github.com/JakeWharton/butterknife 使うとコード短くなるしその辺隠れて良い。

`@OnClick()` とか相当良い。

テスト
---

UI テストは Robotium がまともらしい。 espresso はメンテされてないとのこと…

https://code.google.com/p/robotium/
