よく使う感じのクラス責務とか
===

Aggregator
---

- 複数クラスを跨いだ操作を担当

Repository
---

- "Persistence" を担当。（つまりデータベースに突っ込んだりするところ）

Service
---

- ビジネスロジックを含まず、粛々と依頼通りに何かしらの物事をこなすクラス
- けっこう逃げの名前にもなりがち

Builder
---

- なにかしらの Object を生成する責務を持つ。
- 例： `JsonBuilder`

Entity
---

- 値のみ。（厳密に言えば "Entity" と "Value Object" は違うけども…）
- データを保持すること意外何も責務が無い
- ActiveModel は「データの保持」以外にも責務を持っている。

Presenter
---

- "Entity" の値を装飾することが責務。
- 例: `firstName` と `lastName` を持つ Entity を用いて、Presenter が `fullName` を返す。
- いわゆる View でこれをやりがちだが、 Presenter に分けた方が絶対に良い。

Observer
---

- "Entity" の値を見て、変更があれば即座に何かにそれを伝えることが責務
- Model の値を View に bind したりとか。

番外編
---

ActiveModel の責務を明示的に分ければどうなるか

* Entity
* Validator
* Repository
* Observer (Callbacks)
* (DatabaseAdapter)

"Model is to vague."

#### References

- http://www.sitepoint.com/ddd-for-rails-developers-part-3-aggregates/
