Documentation
===

Documentation は大事な行為だと思う。何故か？
言葉でのコミュニケーションは確かに効率が良いし、伝わっているという気分になるけども、それはスケールしないから。
対面のコミュニケーションに頼っていると、大体以下のような問題に見舞われる。

1. 喋った人同士にしか情報残らない（ので、ある事柄について、知らない人が出てくる）
2. 案外、人に喋って事柄を伝えるのは時間が掛かる（効率良いはずなのに）
3. 喋った人にしか情報が渡らないので、複数人に情報を伝えるには逆に効率が悪い。
4. 情報が残らない。（結局、後から文書化したりする手間がかかる）

といいつつチャットのコミュニケーションでも情報がまとまらないので、同じようなことも起こる。

以上、如何に 1. 文字・あるいは動画 / 音声で 2. まとまった情報を残すか、ということが大事。

そのためには良いツールセットが必要である。

開発系
---

### ツールセット

まずはともかく Document Generator

- Objective-C [appledoc](https://github.com/tomaz/appledoc) - XCode docset を出力
- Objective-C & Swift (XCode 6 beta 2 が必要) [jazzy](https://github.com/realm/jazzy) - こっちのほうがカッコいい
- Android javadoc(標準) - 使いにくい
- Android [Doxygen](http://www.stack.nl/~dimitri/doxygen/) - マシ [使ってる Doc](http://jd.bukkit.org/rb/doxygen/)
- Go [godoc(標準)](http://godoc.org/) - godoc.org で GitHub 上のドキュメントを良い感じに documentation してくれる。
- Ruby [yard](http://yardoc.org/) - 普通に使いやすい。

そして、test. よく書かれたテストはそれ自体がドキュメントである。RSpec 形式のテスト読みやすいと思う。
また、結合テストは後述の「結合のための情報」のドキュメントにもなるため、大変重要であると考えている。

- Objective-C [Specta & Expecta](https://github.com/specta/specta)
- Android [lambda-behave](https://github.com/RichardWarburton/lambda-behave) あるいは [AssertJ](https://github.com/square/assertj-android)
- Go [Ginkgo](http://onsi.github.io/ginkgo/)
- Ruby [RSpec](http://rspec.info/)

API 限定で、ドキュメントのフォーマットとして, JSON Schema と Swagger Docs が使える

- [Swagger Docs](https://helloreverb.com/developers/swagger) - 試せる API Doc [サンプル(Petstore)](http://petstore.swagger.wordnik.com/)
  * https://github.com/yvasiyarov/swagger - Swagger を Go で
- [JSON Schema](http://json-schema.org/) - JSON でメタ情報を記述するフォーマット

絵を描くツール

- Balsamiq - https://balsamiq.com/
- cacoo - https://cacoo.com/lang/ja/

写真を共有する

- [gyazo](https://gyazo.com/ja) - SS, Screencast を共有
- vine - アプリの動作とかを手っ取り早く共有

### 開発においてなにを重点的にドキュメントにすべきか？

1. 結合のための情報 (document generator)
  * Web API ならば
    * エンドポイント
    * 認証方式とサンプルコード
    * データ形式
    * リクエストのデータ形式
    * レスポンスのデータ形式
    * 実際に使う流れのサンプルコード
  * モジュールであれば
    * 公開インターフェースの説明と、どう使うか
    * 標準的なユースケースに対して、そのためのインターフェースとサンプルコード
    * 「出来ないこと」と、「弱点」があれば明示的に記述しておく
      * e.g., 「このようなケースでエラーが稀に起こると思いますが、稀なケースなので対策してない」
  * プロトコル
    * どのような実装を用意すべきか
    * リファレンス実装
    * どのようなエラーが予想され、実装でどのように対策すべきか
2. 仕様
  * 大きな仕様は、大枠を描く。図を使えたら図を描く。
  * たとえば、画面遷移であれば図を描くべきである。そのためのツールは上に記載。
  * 細かい仕様は埋もれがちなので、チェックリスト的に網羅的に記述する。
    * チェックリスト的に使えるのが重要
3. クラス構成と、「何故こうなのか」 (document generator, drawing)
  * 「仕様」と被る部分は有るが、こちらは実装寄りの話。「仕様」はインターフェースである。
  * どんなクラス構成になっており、それぞれのクラスはどのような責務を担っているか。（また、どのような位置づけか）
  * また、実装において考えたことをだだ流しにする。
    * （e.g., このクラスは拡張を考えれば xx にすべきだけども、そのような拡張は予定していないため、こうなっている）
    * 「出来ないこと」と、「弱点」があれば明示的に記述しておく
      * e.g., 「このようなケースでエラーが稀に起こると思いますが、稀なケースなので対策してない」
  * ところでクラス設計の話
    * クラスはドメインによって明確にカテゴライズされるべきである。
    * ライブラリ化出来るクラス群はライブラリに出す。疎結合化を促進する。
    * クラスの命名便利なやつ: https://github.com/kaiinui/note/blob/master/Design--CommonClasses.md
4. どのようにテストすべきか (DesignDoc に含まれる)
  * どのような深度・周期でテストをすべきか
    * 例えば、「このモジュールはロジックが沢山絡むので、ユニットテストをカッチリやる」「このモジュールはジェスチャが沢山絡み合っているため、UI テストをカッチリやる」
    * 「このようなテストがしたい」のような願望も書いておくと忘れにくい
    * 「嵌りやすいところ」を明文化しておくことが肝心。
  * どのようなデータ / ケースを使えばテスト出来るか。
5. リファクタリング案
  * 将来に向けて変更を加えるため、どのようなリファクタリングをすべきかを記述する。
    * e.g., 「このクラスは本来であればこうすべきだが、今はこうなっている」
  * コードを書いた人がリファクタリング案を書く。
  * 場所は Issue が良い
6. DesignDoc
  
また、Documentation がどれほど完了しているかを表す indicator を付けるべきである。 (e.g., ![](http://progressed.io/bar/20))

（`http://progressed.io/bar/0` などで便利に作れる。）
  
### Project 構成

GitHub Wiki は埋もれるため、あまり使わない方が良いと考えている。
だから、通常の commmit で書く。
メタ情報も履歴が残ってしかるべきだし、コメントも GitHub 上で残せる。

#### 表に出るシステムの場合（モジュールではない場合）

- README.md - プロジェクトの概要(Why? How? Who?), Screen Shot (or Movie clip), 試せるものがあればリンク, 関連リンク, 各種 Doc へのリンク, 参考リンク
- Doc--Architecture.md - 「3. クラス構成」を記述する。また、他の Doc に対するリンクを全て含める。
- Doc--Spec.md - Major スペック（大枠）を記述する。
- Doc--MiniSpec.md - Minor スペックをチェックリスト形式で記述する。
- Doc--DesignDoc.md - Design Doc を記述する (Test はここに含まれている)
- SomeClass.h - Inline document (javadoc style) のドキュメントを書く。ドキュメントジェネレータに食わせる。

#### モジュールの場合

上記に加え、

- README.md - + 公開インターフェースの説明とサンプルコード, 
- Doc--Interface.md - 「1. 結合のための情報」を記述する。また、他の Doc へのリンクを全て含める

課題
---

- 色々やってみて最適解を探すしかない
- 面倒くささある
- 情報の鮮度を保つのが大変（GitHub の最終更新時刻で古いのが分かるのは良い。）
  * 時間的に古くなったら Notification してもいいかも
  * 情報的に古いのはどうしようもない
  * テスト / コードの inline document からドキュメント生成というのが、かなり現実的だと思う。
- そんなに量なくて良くて、キッチリドキュメントするというのが大事
  * 一番大事なのは、「どうしてこうなっているのか」と「今後の課題」をコメントでもいいから残しておくこと。
- 頭の中を dump する。
- ツールで出来る範囲が増えるとよい
  * Slack の会話をログしてイイ感じに紐づける
  * Documentation されてない Class が増えたら怒る
  * API はテストとか Schema から生成出来ると良い
  * Inline comments をイイ感じに可視化する。（`TODO:` とかをイイ感じに一覧にしたい。）
  
References
---

### DesignDoc

- [note/Design--Designdoc.md at master · kaiinui/note](https://github.com/kaiinui/note/blob/master/Design--Designdoc.md)
- [Google の Design Doc について - フリーフォーム フリークアウト](http://d.hatena.ne.jp/cou929_la/20091116/1258373100)

### Module / Interface Documents

- [Realm](http://realm.io/docs/cocoa/0.84.0/) - Realm のドキュメント。
- [NSOrderedSet Class Reference](https://developer.apple.com/library/mac/documentation/Foundation/Reference/NSOrderedSet_Class/Reference/Reference.html) - Apple のドキュメント。良い。
- [Protocol Buffers — Google Developers](https://developers.google.com/protocol-buffers/) - Protocol Buffers のドキュメント. well-documented.
- [cacoo API](https://cacoo.com/lang/ja/api)
