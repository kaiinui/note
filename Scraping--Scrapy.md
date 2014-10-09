Scrapy
===

Scrapy はイイ感じにモジュラな設計になっているスクレイピングフレームワーク。Python 製。ただし 2 系。

Scrapy は以下のような設計になっている。

![](http://cdn-ak.f.st-hatena.com/images/fotolife/m/mi_kattun/20140104/20140104221104.jpg)

Spider で対象となる URL をかき集め、Downloader で対象 URL をダウンロードし、それを Item Pipeline で処理し、結果を永続化する。
Scheduler は BAN 対策のためのスケジュール調整のストラテジーを担当する。

他のスクレイピングフレームワークとの比較
---

- anemone (Ruby): 必要最低限の機能（同一 URL 防止、リンク抽出、メインループ）しか提供しておらず、使えない。
- Apache Nutch (Java): スゴく色々揃ってるような気がする。が、ゆるい情報が少なくて辛い。Scrapy と比べるとコミュニティは弱い。

実際
---

以下のような Spider を書くことで、リンクを次々と辿っていく形の Spider を作ることが出来る。

```py
# coding: utf-8

from datetime import datetime

from scrapy.contrib.spiders import CrawlSpider, Rule
from scrapy.contrib.linkextractors.sgml import SgmlLinkExtractor
from scrapy.selector import Selector

from helloscrapy.items import NewsItem


class CNetSpider(CrawlSpider):
    name = 'cnet'
    allowed_domains = ['news.cnet.com']
    start_urls = [
        'http://news.cnet.com/8324-12_3-0.html',
    ]
    rules = [
        # 正規表現 'begin=201312' にマッチするリンクを辿る
        Rule(SgmlLinkExtractor(allow=(r'begin=201312', ), restrict_xpaths=('/html', ))),
        # 正規表現 '/[\d_-]+/[^/]+/$' にマッチするリンクをparse_newsメソッドでパースする
        Rule(SgmlLinkExtractor(allow=(r'/[\d_-]+/[^/]+/$', ), restrict_xpaths=('/html', )),
             callback='parse_news'),
    ]

    def parse_news(self, response):
        item = NewsItem()

        sel = Selector(response)
        item['title'] = sel.xpath('//h1/text()').extract()[0]
        item['body'] = u'\n'.join(
            u''.join(p.xpath('.//text()').extract()) for p in sel.css('#contentBody .postBody p'))
        item['time'] = datetime.strptime(
            sel.xpath('//time[@class="datestamp"]/text()').extract()[0].strip()[:-4],
            u'%B %d, %Y %I:%M %p')

        yield item
```

(出典: http://orangain.hatenablog.com/entry/scrapy)

References
---

- ScrapyとPhantomJSを用いたスクレイピングDSL : http://www.slideshare.net/MasayukiIsobe/web-scraping-20140622isobe
- PythonとかScrapyとか使ってクローリングやスクレイピングするノウハウを公開してみる！ - orangain flavor : http://orangain.hatenablog.com/entry/scrapy
- Scrapy 0.24 documentation — Scrapy 0.24.4 documentation : http://doc.scrapy.org/en/latest/index.html
