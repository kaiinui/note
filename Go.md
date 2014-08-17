所感
---

- python みたいに C が書ける。以上。
- gofmt 勝手にやってくれるの良い。記法が勝手に統一される。
- lint がやたらうるさいのが良い。#lint に細かく書く
- `go get` いいけどどうやってバージョン管理するんだとか、色々なアレがある
- `net/http` が神。 言語レベルでこういうのがあると、エコシステムが構築されやすいだろうなぁ。

Lint
---

- 未使用 import
- 未使用 var
- 戻り値を暗黙的に捨てる

のが lint で怒られる。lint といいつつも、lint 通らないとコンパイル出来ないというアグレッシブさ。良いと思います。

要らない変数とか使ってない import とかがイイ感じにコードから消えて実にメンテナブルである。

戻り値を暗黙的に捨てる（左に何も置かない）のは禁止されているので、何かを捨てているのが明示的になるのが良い。（`_` に入れると捨てれる。）

Frameworks
---

- [Martini](http://martini.codegangsta.io/) - Sinatra in Go
- [revel](http://revel.github.io/) - Rails in Go

Libraries
---

###Template

- [Gold](https://github.com/yosssi/gold) - Jade/Slim like template. has official sublime text plugin.

Utilities
---

- [gin](https://github.com/codegangsta/gin) - hotreload

Test
---

[Ginkgo](http://onsi.github.io/ginkgo/) という RSpec 風のやつがある。

```go
var _ = Describe("Book", func() {
    var (
        longBook  Book
        shortBook Book
    )

    BeforeEach(func() {
        longBook = Book{
            Title:  "Les Miserables",
            Author: "Victor Hugo",
            Pages:  1488,
        }

        shortBook = Book{
            Title:  "Fox In Socks",
            Author: "Dr. Seuss",
            Pages:  24,
        }
    })

    Describe("Categorizing book length", func() {
        Context("With more than 300 pages", func() {
            It("should be a novel", func() {
                Expect(longBook.CategoryByLength()).To(Equal("NOVEL"))
            })
        })

        Context("With fewer than 300 pages", func() {
            It("should be a short story", func() {
                Expect(shortBook.CategoryByLength()).To(Equal("SHORT STORY"))
            })
        })
    })
})
```

