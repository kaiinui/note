API Batch Request
===

[モバイルAPIデザインのまとめ](http://wazanova.jp/items/1283) でも触れられていた通り、如何にリクエストを減らすか。
Etsy では API エンドポイントをモバイルチームで改造出来るようにしている。

もう少し汎用的には、JSON-RPC Batch 2.0 のような方法は、解決策の一つになり得る。

Etsy のような（あるいは API エンドポイントを特定アプリに特化させる）アプローチでは API のエンドポイントとクライアントが密結合になりやすい（まあ、なっても別にいいけど）

なので、"A & B & C" を渡す API 、とエンドポイントを特化させるのではなく、リクエストする側が "A と B と C" をくれ！とやる作戦。

```
# POST /batch
# Content-Type: application/json
```
```json
{
  "ops": [
    {"method": "get",    "url": "/patrons"},
    {"method": "post",   "url": "/orders/new",  "params": {"dish_id": 123}},
    {"method": "get",    "url": "/oh/no/error", "headers": {"break": "fast"}},
    {"method": "delete", "url": "/patrons/456"}
  ],
  "sequential": true
}
```

```json
{
  "results": [
    {"status": 200, "body": [{"id": 1, "name": "Jim-Bob"}, ...], "headers": {}},
    {"status": 201, "body": {"id": 4, "dish_name": "Spicy Crab Legs"}, "headers": {}},
    {"status": 500, "body": {"error": {"oh": "noes!"}}, "headers": {"Problem": "woops"}},
    {"status": 200, "body": null, "headers": {}}}
  ]
}
```

JSON-RPC Batch 2.0 崩れのようなリクエスト。だけども実際的で良いと思う。中身は RESTful な routing.
実際的には単一のリクエストも route を切りつつ、`/batch` とかでバッチリクエストを JSON-RPC (風) に受けるのが良いと思った。

ただし、一挙にリクエストを受ける都合上レスポンスが遅くなりがちな問題がある。

Response, [Chunked Transfer Encoding](http://en.wikipedia.org/wiki/Chunked_transfer_encoding) のような受け方をする必要があるのかもしれない。

References
---

- [モバイルアプリとAPIのありかたを考える2014](https://speakerdeck.com/ar_tama/mobairuapuritoapifalsearikatawokao-eru2014) - JSON-RPC Batch 2.0 について言及
- [Facebook の Batch Request](https://developers.facebook.com/docs/graph-api/making-multiple-requests)
- [Google Cloud Storage の Batch Request](https://developers.google.com/storage/docs/json_api/v1/how-tos/batch)
- [AWS Product Advertising API の Batch Request](http://docs.aws.amazon.com/AWSECommerceService/latest/DG/BatchRequests.html)
- [Rails gem: batch_api](https://github.com/arsduo/batch_api) - Facebook の Batch Request みたいな API エンドポイントを切れる gem.
