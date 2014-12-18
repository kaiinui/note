JSON-RPC 2.0
---

JSON で RPC しようという仕様がある。まあ RPC での利用はどうでもいい。
API 呼び出しを RPC とみたて、JSON-RPC over HTTP をするとき、**割と標準的な仕様** として利用出来る。

Spec
---

```
{
    "jsonrpc": "2.0",
    "id": "some_id_for_invocation",
    "method": "methodName",
    "params": {
        "foo": "bar"
    }
}
```

通信レイヤが HTTP であることを限定していないので、Header や HTTP Method は仕様で指定されない。

`POST` で、`Accept-Content-Type: "application/json"` で良い。

利点？
---

1. Header に特別な情報を含まない。
  - Header を弄れないクライアントでも使える。（JSONP とか！）
2. HTTP Method に特別な情報を持たせない。
  - 目に見える「ボディ」だけに情報を集められる。
  - HTTP Method を自由に指定出来ないクライアントでも使える。（JSONP）
3. レスポンスに、標準化した形で Error 情報を含められる。
4. JSON-RPC 2.0 Batch という仕様があり、複数メソッドの一挙呼び出しに対応している。通信の数を抑えられるので Mobile API に良い。。

REST みたいに流暢ではないが、堅実でよい！
REST は URL がカッコいいけど、暗黙な部分が多いし、弄るべきフィールド（Header, HTTP Method, Body, Params..）が多くてつらい。

あと、Batch のリクエストなど、REST の「URL だけで出来るだけ全てを表現する」思想ゆえに対応が辛い場合が多い。

究極、GET でも params に JSON を含めて通信すれば良いので、JSONP でも全く同じ API 設計を持ってこられる。

欠点
---

1. `multipart/form-data` が辛い。バイナリを含められない。
2. REST に比べ一般的でない。
3. `Authorization` Header をどこに含めるか困る。
4. 
