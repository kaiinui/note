API デザイン (kaiinui/api_design-resources から移動)
====================

- [Web API 設計のベストプラクティス集 Web API Design](http://d.hatena.ne.jp/cou929_la/20130121/1358776754)
- [Web API Design](http://offers.apigee.com/web-api-design-ebook/)
- [Heroku HTTP API Design Guide](https://github.com/interagent/http-api-design) - Heroku でまとめられた、API のデザイン指針
- [《REST思想》と《リソース指向》と《Webページ》に関する（主にRailsの）話](http://qiita.com/tkawa/items/9bd50e80cfe354062dfb) - よい URL とは？ REST とリソース指向から読み解くURL
- [アジャイルAPI設計時代の到来！？APIデザインの極意を読みました。](http://kozake.hatenablog.com/entry/2014/08/03/232443) - ヤバい API 本
- [すべてがJSONになる](http://r7kamura.hatenablog.com/entry/2014/06/10/023433) - JSON Schema を利用して、Validation や Doc, API Client を自動的に用意する
- [NetflixのAPIプラットフォーム](http://wazanova.jp/items/1114) - フロントエンドチームが API をカスタマイズできるようにしている
- [モバイルAPIデザインのまとめ](http://wazanova.jp/items/1283) - Etsy での API デザイン。モバイルのために、如何に必要な API コール回数を減らすかを考える。
- [\[その１\] Netflix: APIの改善と継続的デリバリー](http://wazanova.jp/items/678) - Netflix は如何に 800 種のデバイスに利用される API を運用しているのか？
- [Ruby RoguesメンバとiOSエンジニアのAPI議論](http://wazanova.jp/items/1211) - 一貫性、ドキュメント、モバイル

Batch Request
---

[モバイルAPIデザインのまとめ](http://wazanova.jp/items/1283) でも触れられていた通り、如何にリクエストを減らすか。
Etsy では API エンドポイントをモバイルチームで改造出来るようにしている。

もう少し汎用的には、JSON-RPC Batch 2.0 のような方法は、解決策の一つになり得る。

Etsy のような（あるいは API エンドポイントを特定アプリに特化させる）アプローチでは API のエンドポイントとクライアントが密結合になりやすい（まあ、なっても別にいいけど）

なので、"A & B & C" を渡す API 、とエンドポイントを特化させるのではなく、リクエストする側が "A と B と C" をくれ！とやる作戦。

- [モバイルアプリとAPIのありかたを考える2014](https://speakerdeck.com/ar_tama/mobairuapuritoapifalsearikatawokao-eru2014) - JSON-RPC Batch 2.0 について言及
- [Facebook の Batch Request](https://developers.facebook.com/docs/graph-api/making-multiple-requests)
- [Google Cloud Storage の Batch Request](https://developers.google.com/storage/docs/json_api/v1/how-tos/batch)
- [AWS Product Advertising API の Batch Request](http://docs.aws.amazon.com/AWSECommerceService/latest/DG/BatchRequests.html)
- [Rails gem: batch_api](https://github.com/arsduo/batch_api) - Facebook の Batch Request みたいな API エンドポイントを切れる gem.

```json
# POST /batch
# Content-Type: application/json

{
  ops: [
    {method: "get",    url: "/patrons"},
    {method: "post",   url: "/orders/new",  params: {dish_id: 123}},
    {method: "get",    url: "/oh/no/error", headers: {break: "fast"}},
    {method: "delete", url: "/patrons/456"}
  ],
  sequential: true
}
```

```json
{
  results: [
    {status: 200, body: [{id: 1, name: "Jim-Bob"}, ...], headers: {}},
    {status: 201, body: {id: 4, dish_name: "Spicy Crab Legs"}, headers: {}},
    {status: 500, body: {error: {oh: "noes!"}}, headers: {Problem: "woops"}},
    {status: 200, body: null, headers: {}}}
  ]
}
```

JSON-RPC Batch 2.0 崩れのようなリクエスト。だけども実際的で良いと思う。中身は RESTful な routing.
実際的には単一のリクエストも route を切りつつ、`/batch` とかでバッチリクエストを JSON-RPC (風) に受けるのが良いと思った。

ただし、一挙にリクエストを受ける都合上レスポンスが遅くなりがちな問題がある。

Response, [Chunked Transfer Encoding](http://en.wikipedia.org/wiki/Chunked_transfer_encoding) のような受け方をする必要があるのかもしれない。

Pagination
---

[Using the Graph API](https://developers.facebook.com/docs/graph-api/using-graph-api/v2.1)

```json
{
  "data": [
     ... Endpoint data is here
  ],
  "paging": {
    "cursors": {
      "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
      "before": "NDMyNzQyODI3OTQw"
    },
    "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw"
    "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
  }
}
```

[Instagram API Endpoints](http://instagram.com/developer/endpoints/)

`The envelope`: API レスポンスのフォーマット。 Instagram の API レスポンスは全て envelope 形式。

```json
{
    "meta": {
        "code": 200
    },
    "data": {
        ...
    },
    "pagination": {
        "next_url": "...",
        "next_max_id": "13872296"
    }
}
```

##### Meta

```json
{
    "meta": {
        "error_type": "OAuthException",
        "code": 400,
        "error_message": "..."
    }
}
```

#### Data

普通の `body`

#### Pagination

```json
{
    ...
    "pagination": {
        "next_url": "https://api.instagram.com/v1/tags/puppy/media/recent?access_token=fb2e77d.47a0479900504cb3ab4a1f626d174d2d&max_id=13872296",
        "next_max_id": "13872296"
    }
}
```

Formats
---

### Request

当たり前だけど GET は URL params で。 `(e.g.) ?user_id=hogehoge&password=hogehoge`

POST の場合は Body に何でも詰め込めるので後述のデータ構造を全て利用出来る。

### Response

基本的には Dictionary 的なデータ構造で返す。（つまり JSON みたいなやつ）

- JSON
- MessagePack
- Binary plist
- JSONP (まあ。)

Binary plist については、これを使って iOS での高速化を果たした報告が有る（http://tech.vasily.jp/backend_ios_plist/）

あるいは XML のようなメタ情報をたくさん詰め込めるフォーマットを使っても良い。(が、大体 JSON みたいな使い方をされる)

- XML

前者 4 つはフォーマットの違いでしかないので、透過的に扱える限りは全て返せるようにしたほうが良い感じ。（XML はもう少し扱える範囲が大きいと思う）
どうせ Dictionary を食わせて生成するのだから、少しの努力で大体何でも返せる。

それぞれの端末で一番良い方法で受け取ってもらうのが一番良いと思う。

当たり前だけど gzip は必ずする。

Try-able API Document (Console)
---

instagram とか Facebook とかは「試せる」API ドキュメントを提供している。
これは、API を利用する側にとっては気軽に試すことが出来て大きいコトと思う。
（もちろん 1. API ドキュメントが整備されている 2. API クライアントライブラリ実装がある ことが一番。）

- https://apigee.com/console/instagram
- https://developers.facebook.com/tools/explorer/
- http://petstore.swagger.wordnik.com/

このような API コンソールを提供するツールとしては、

- [apigee](https://apigee.com/)
- [Swagger UI](https://github.com/wordnik/swagger-ui)

がある。

Tools
---

- [Apiary](http://apiary.io/) - API Blueprint というスキーマから、Validation や Doc などを生成
- [Swagger UI](https://github.com/wordnik/swagger-ui) - JSON から「試せる API Doc」を生成
- [prmd](https://github.com/interagent/prmd) - JSON Schema を色々出来るツール by Heroku
