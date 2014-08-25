ID 生成器
===

MySQL では auto-increment で increment な ID を用いる。これはアルゴリズムが単純かつ順序を保存し、かつ少ない領域で保存出来る利点がある。

しかし、単一の生成器でしか ID を生成出来ないという欠点がある。ゆえに、分散システムとの相性が悪い。
また、timestamp 情報を含められないので、`created_at` など別カラムとして生成日を保管する必要がある。

SnowFlake
---

Twitter では対応として、SnowFlake という分散 ID 生成器を用いている。

* 63 bit で表される (long int)
* timestamp を含む（ゆえに、順序が保存される）
* 1024 台までの生成器で安全に ID を生成出来る。

bit layout は以下のようにデザインされている。

* timestamp: 41bit
* machine id: 10bit
* sequence: 12bit

ただし、timestamp は unixtime ではなく特定の日の unixtime を引いた値となっている（これにより bit 数を節約）

sequence では increment に値を付加し、同一 timestamp での ID 衝突を防ぐ。

Device-side ID generating では？
---

デバイス側で ID を生成したい際どうするか？
まず machine id は 10bit では全然足りないし、 sequence も 12bit も必要ない。

しかし、22 bit を全力で振り切っても、たった 400 万程度しかない。
128 bit 無いと timestamp + device id + sequence でのユニーク化は難しい気がする…

が、実際はデバイス内でのユニーク性だけあればよく、マスター側でのユニークは device id + item id で担保すればよく、device id を id に含めなくて良いのかもしれない。

参考
---

- http://qiita.com/daisy1754/items/98a6e6b17d8161eab081
- http://developer.smartnews.be/blog/2013/07/31/shakeflake-is-a-tool-for-generating-unique-id-numbers/
