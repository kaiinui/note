Design Doc
---

Google は Design Doc と呼ばれる設計文書を、実際に設計・実装にとりかかる前に書く。

内容は、

* Why?
* How?
* Who?
* セキュリティとか性能の懸念とか
* テスト方法等

など。

> プロジェクト立ち上げ時の 1～2週間をかけて書く。ある程度ポイントが書け たら、もうコーディングへ。

(http://blog.livedoor.jp/heitatta/archives/54439839.html)

「コードをガッツリ書ける人が」「コードを書く時間を捨てて」「1-2w というそこそこ長い時間を使って」書く…
わりと人的コストをかけて書いていることが伺える。

メリット
---

こういった文書を書くことには、以下のような良さがあると思った。

1. 問題が浮き彫りになる（Good Problem to have.）
2. チームで問題 / 課題を共有出来る
3. チームでプロダクトの方向性を共有出来る
3. 各々が好き勝手に実作業に取り掛かり、結果齟齬が出る可能性を予防出来る
  * 「問題の早期発見」
4. 文書に落とし込むことにより設計 / 実装を **具体的** にイメージすることが出来る
5. フォーマットが統一されていることにより、見落としがちなポイントを設計時点で考慮出来る
6. 具体的なフォーマットに沿った文書に落とし込むので、考慮出来ていないポイントが自明になる
  * あやふやな仕様を残さず実作業に入ることが出来る。

Design Doc は一人で書くプロダクトにも有効だが、複数人で開発するときにより効果を発揮する。具体的には、

1. 設計を誰かが一元的に済ませて…ではないので、チーム全員の意図が入った設計になりやすい
2. チーム全員が設計にコミット出来る（理想的には）
3. 仕様 / 思想 / 方法がチームで共有出来る。しやすい。

といった便益があるように思う。全体的に、「チームで共有する」「文書」を書くことと、あやふやになりがちなものを「書いて」残すことがこれらの便益をもたらすのだと感じた。

具体的な Design Doc
---

このへんで Googler の書いた Design Doc を参照することが出来る。

- Design Documents http://www.chromium.org/developers/design-documents
- Dagger 2.0 https://docs.google.com/document/d/1fwg-NsMKYtYxeEWe82rISIHjNrtdqonfiHgp8-PQ7m8/edit#heading=h.2t5cq7yndhts

あるいはこちらのブログで実例へのリンクが挙げられている。

- Google の Design Doc について http://d.hatena.ne.jp/cou929_la/20091116/1258373100

References
---

- Google のソフトウェア・エンジニアリング http://blog.livedoor.jp/heitatta/archives/54439839.html
- Google の Design Doc について http://d.hatena.ne.jp/cou929_la/20091116/1258373100
- Googleのdesign docを眺めてみる http://kenmaz.hatenadiary.jp/entry/20090712/1247401684
- Design Documents http://www.chromium.org/developers/design-documents
- Dagger 2.0 https://docs.google.com/document/d/1fwg-NsMKYtYxeEWe82rISIHjNrtdqonfiHgp8-PQ7m8/edit#heading=h.2t5cq7yndhts
