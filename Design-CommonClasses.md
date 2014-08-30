よく使う感じのクラス責務とか
===

Aggregator
---

- 複数クラスを跨いだ操作を担当

#### References

- http://www.sitepoint.com/ddd-for-rails-developers-part-3-aggregates/

Service
---

- ビジネスロジックを含まず、粛々と依頼通りに何かしらの物事をこなすクラス
- けっこう逃げの名前にもなりがち

Entity
---

- 値のみ。（厳密に言えば "Entity" と "Value Object" は違うけども…）
- データを保持すること意外何も責務が無い
- ActiveModel は「データの保持」以外にも責務を持っている。

番外編
---

ActiveRecord の責務を明示的に分ければどうなるか

* Entity
* Validator
* Repository
* Observer (Callbacks)
* (DatabaseAdapter)
