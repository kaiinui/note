Scraping BestPractices
===

Crawler と Scraper を分離する
---

Crawler (集める係) と Scraper (解析する係) を同じ系で管理してしまうと、実際的には行儀が悪い。

普通にやれば、Crawler がダウンロードしたものを、同一の系で Scraper が即座に解析するフローとする。
しかし、これは以下の点で最悪のフローである。

* 野生の HTML はまともなものばかりではないので、実際的には Scraper は簡単に失敗する。（e.g. サイトのデザインが変わった、このページだけ特別、キャンペーンで特別なページが出来た etc..）
* その際に、動かし方が悪ければ最悪 Crawler が引っ張られて死ぬ。
* タスクを永続化して管理していないので、Scrape タスクが失敗してしまえば、再開も出来ない。（タスク管理の状態が失われる）

以上より、Crawler と Scraper は分離したほうがよい。具体的には、

* Crawler は常時駆動し、ただ URL を集めて HTML とアセットをダウンロードする (S3 等に保存することが多い)
  * その際、SQS などを用いて、キューイングする。
* Scraper は SQS に貯められたキュー駆動で動き、S3 から HTML をダウンロードし解析。結果を DB などに永続化する

というフローになる。

Crawler はより愚直に、如何に BAN されないかだけ考えて、URL と HTML を集めていれば良い。

Scrapy であれば、Item Pipeline に様々な処理を書く所を、S3 にアップロードして SQS にキューイングするだけの処理とすればよい。

Scraper は何の言語 / フレームワークで書いても良い。

IP を分散する (Proxy を用いてダウンロードする)
---

IP は分散すればするだけ BAN されにくくなる。
また、IP を簡単に入れ替える構成を作っておけば、一旦 BAN されてしまっても容易に復旧することが出来る。

このために Crawler の IP を入れ替えても良いが、より簡便には、Proxy を通してダウンロードするようにしておけば、より役割分担が明確になる。
すなわち、Proxy が「IP をいい感じにして BAN されないようにダウンロードする」責務を持てば良い。

この as a Service として [Crawlera](http://crawlera.com/) がある。

PhantomJS を使う
---

JS を使って HTML を描画するサービスが増えてきているので、単純な HTML パースでは対応出来ないことも多い。

Selenium WebDriver などを用いて、実際のブラウザを動かしても良いが、実際的には PhantomJS がお手軽で速くて良い。

Scraper は構造定義ファイルを元に HTML を解析する
---

`nokogiri` などでパースして解析するベタ書きでは、複数のサイトに跨がるスケールは難しい。

実際的には、サイト / ページの構造を定義したファイルを用意しておく。こうすれば、動いている Scraper のソースに触らず、様々なサイトに対応することが出来る。

#### サイトの構造

* Base URL
* 取得したいページのパターンの配列

#### ページの構造

* URL のパターン（正規表現で表されることが多い）
* 取得要素の配列
  * XPath / CSS Path (XPath のほうが表現が強力)
  * 名称 (`price` や `name` など)

失敗した Scrape を解析する
---

パターンが変わっていれば対応しなければならないので、失敗した Scrape は常に追いかけられるようにしておく。

References
---

- Scrapy | An open source web scraping framework for Python : http://scrapy.org/
- Crawlera : http://crawlera.com/
- PhantomJS | PhantomJS : http://phantomjs.org/
- PythonとかScrapyとか使ってクローリングやスクレイピングするノウハウを公開してみる！ - orangain flavor : http://orangain.hatenablog.com/entry/scrapy
- ScrapyとPhantomJSを用いたスクレイピングDSL : http://www.slideshare.net/MasayukiIsobe/web-scraping-20140622isobe
