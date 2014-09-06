JSONPatch
===

```json
[
  { "op": "replace", "path": "/baz", "value": "boo" },
  { "op": "add", "path": "/hello", "value": ["world"] },
  { "op": "remove", "path": "/foo"}
]
```

のような JSON. これを次のような JSON に適用すれば

```json
{
  "baz": "qux",
  "foo": "bar"
}
```

↓

```json
{
   "baz": "boo",
   "hello": ["world"]
}
```

こうなる。まあ、`op` 通りの操作を順当にやるだけ。 
`op` には色々語彙が用意されているが、基本的には `add`, `remove`, `replace` あたりがまともに使える語彙。

面白いこと？
---

次のように Client <=> API のデータのやりとりを捉えることで、JSONPatch を利用した一貫性のあるやりとりが可能となる。

- Client 上で持っているデータを JSON と見なす。（本当は SQLite とかのデータベースで持っている。）
  * Resource のネームスペースにそれぞれの ID を入れ子した JSON と見なすことが出来る。 (下記の例を参照)
- データを取得する場合
  1. Client はサーバに JSONPatch を問い合わせる。
  2. Client は得られた JSONPatch を自身のデータベースに適用する。
  3. 以上により、Client はサーバ上の resources と自身のデータベースの resources を同期することが出来る。
- データを push する場合
  1. Client は JSONPatch の表現で、Client 上で変更したデータの差分をバックエンドに渡す
  2. バックエンドは自身のデータベースに JSONPatch を適用する
  3. （他の clients にも変更を反映させたい場合、ここで JSONPatch をそのまま Eventsource 等で broadcast する）
  4. 以上により、Client は自身のデータ変更をバックエンドに反映させることが出来る。
  
#### 例: データベース上のデータを JSON とみなす  

Primary Key を UUIDv1 として書く。Incremental な Int key でも良い。

`books` と `authors` というテーブルにそれぞれ 3 つのデータが入っている。これを JSON で次のように表すことが出来る。

```json
{
  "books": {
    "4cc47114-35b7-11e4-bb0f-164230d1df67": {
      "title": "Harry Potter",
      "author_id": "38ce0e76-35b8-11e4-bb0f-164230d1df67"
    },
    "4cc47376-35b7-11e4-bb0f-164230d1df67": {
      "title": "Kokoro",
      "author_id": "38ce0fd4-35b8-11e4-bb0f-164230d1df67"
    },
    "4cc47632-35b7-11e4-bb0f-164230d1df67": {
      "title": "Crime and Punishment",
      "author_id": "38ce111e-35b8-11e4-bb0f-164230d1df67"
    }
  },
  "authors": {
    "38ce0e76-35b8-11e4-bb0f-164230d1df67": {
      "name": "J. K. Rowling"
    },
    "38ce0fd4-35b8-11e4-bb0f-164230d1df67": {
      "name": "Natsume Soseki"
    },
    "38ce111e-35b8-11e4-bb0f-164230d1df67": {
      "name": "Fyodor Dostoyevsky"
    }
  }
}
```

この場合、例えば "J. K. Rowling" は `/authors/38ce0e76-35b8-11e4-bb0f-164230d1df67` の `path` で表される。

課題？
---

- patch を作るには path が不可欠なので、各 resource に対して Client 側で ID を割り当てなければならない。このため、Incremental な primary key は使用不可能である。これは標準化されていない。
- クライアントで変更したデータを逐次送信するのでなければ、クライアントは自身の resources に "変更済み" マークを持たなければならない。これは標準化されていない。
- サーバは各々のクライアントに対して、それぞれどの段階での patch を持っているか知っていなければならない。あるいは、各々のクライアントは自身がどの patch まで適用されているか知っていなければならない。これは標準化されていない。
- ある範囲だけの patch が欲しい場合のリクエストは標準化されていない。これは JSON-RPC2.0 Batch で表現した方が表現の幅が広い。（なぜなら JSONPatch は resources の操作しか提供しない）

References
---

- jsonpatch.com : http://jsonpatch.com/
- Rocket: a hybrid approach to real-time cloud applications : http://rocket.github.io/
