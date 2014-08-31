よく使う感じのクラス責務とか
===

良い名前を使うことで、クラスの責務の過多を防ぐことが出来る。（Policy クラスに API アクセス機能を持たせる奴は居ない）
様々なクラスの引き出しを持つことで、パターンに遭遇したときに容易にクラス責務を分けることが出来る。

また、責務を細かい粒度で分けてハッキリとさせることで、テストやテストダブルが容易になる。

一体何をテストすれば！？みたいにならない。各クラスが己の責務を粛々と果たすことを保証すればよい。
クラスの連携は結合テストの範囲。

Aggregator
---

- 複数クラスを跨いだ操作を担当
- Rails だとよく Model に書かれたりする。（この責務過多は最悪のパターンだと思う。）
- たとえば、「Order を作ったら、 afterCreate で 商品マスタの数を減らす」
- Transaction も絡むし、絶対にクラス横断の連携は他のクラスに切る。
- 横断操作、ヤバくなりやすい。

Repository
---

- "Persistence" を担当。（つまりデータベースに突っ込んだりするところ）
- ActiveModel でいえば、`#save`, `#find`, `#where` とか

Service
---

- ビジネスロジックを含まず、粛々と依頼通りに何かしらの物事をこなすクラス
- **Service はビジネスロジックを含まない。ビジネスロジックを担当するクラスが依頼を考え、Service は依頼を受けて粛々と実装するだけ。切り分けを間違ってはいけない。**
- けっこう逃げの名前にもなりがち

Builder
---

- なにかしらの Object を生成する責務を持つ。
- 例： `JsonBuilder`

Adapter
---

- 何かしらのややこしい操作を隠蔽して、シンプルな感じにして提供する責務。
- つまりラッパー。
- （Adapter を使うクラスは、ややこしい操作をクラス内に含めずに済み、責務の肥大化を防ぐ。）

Policy
---

- 何らかのルールを記述する（例：「バケツ」はその容量を超えて中身を入れられない）
- クラスにルールを持たせると、責務過多になりやすい
- たとえば、ビジネスロジックは当てはまりやすい
- 例：「プレミアムでない場合はこれが出来ない」は完全に Policy クラスの責務。

Specification
---

- 「仕様」クラス
- 完全に理解していないが、Eric Evans の DDD で触れられていた。

Entity
---

- 値を保持することのみが責務。（厳密に言えば "Entity" と "Value Object" は違うけども…）
- データを保持すること意外何も責務が無い
- ActiveModel は「データの保持」以外にも責務を持っている。

Presenter
---

- "Entity" の値を装飾することが責務。
- 例: `firstName` と `lastName` を持つ Entity を用いて、Presenter が `fullName` を返す。
- いわゆる View でこれをやりがちだが、 Presenter に分けた方が絶対に良い。
- Rails ではやっぱり Model に書かれがち。

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
