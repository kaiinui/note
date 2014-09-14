Protocol Buffers
---

エディタ？
---

IntelliJ IDEA に Protocol Buffers Support がある。

とりあえず入門
---

```proto
package someproj;

message Book {
    required string title = 1;
    required int32 page_number = 2;
}
```

1 とか 2 とかは、プロパティの ID みたいなん。
プロパティを特定しているのは、`title` とかではなく `1`.
これによって、`title` をやっぱり `name` にしたり出来る。

各言語の実装
---

- Java, C++, Python: http://github.com/google/protobuf
- Golang: goprotobuf
- Objective-C: http://protobuf.axo.io/

`protoc` は `brew install protobuf` で入れられる模様。

`protoc --go_out=. *.proto` とかでコンパイルする。

Objective-C のコンパイルは

`protoc --plugin=/usr/local/bin/protoc-gen-objc *.proto --objc_out="./"`

### 注意

`protoc-gen-go` は `protoc` 無ければ何もないので注意必要

とりあえず実装
---

サーバ (Go)

```go
func main() {
	m := martini.Classic()
	m.Get("/books/:id", book)
	m.Run()
}

func book(w http.ResponseWriter, r *http.Request) {
	book := &Book{
		BookId: proto.String("1"),
		Title: proto.String("Harry Potter And Something!"),
		PageNumber: proto.Int32(322),
	}
	data, _ := proto.Marshal(book)

	_, _ = w.Write(data)
}
```

クライアント (iOS)

```objc
- (void)viewDidLoad
{
    [super viewDidLoad];
    
    NSData *bookData = [NSData dataWithContentsOfURL:[NSURL URLWithString:@"http://0.0.0.0:3000/books/somebook"]];
    
    Book *book = [Book parseFromData:bookData];

    NSLog(@"%@", book);
    
	// Do any additional setup after loading the view, typically from a nib.
}
```

`.proto`

```proto
package main;

message Book {
    required string book_id = 3;
    required string title = 1;
    required int32 page_number = 2;
}
```

データのアレコレを考えなくていいのは楽

データ構造変えたらどうなるか？
---

#### 項目の追加

`required string author_name = 4;` を足して、 "foo" を入れてみた。

![](http://i.gyazo.com/bd0c3f874f1b5ebe57471d950a444856.png)

シリアライズは問題なく出来る。

### プロパティ名の変更

`page_number` -> `page_count` にしてみた。

上手く行く。クライアント側では `page_number` として解釈されている。

やはり、プロパティ名に紐づいた id でプロパティの対応付けが管理されているようだ。

![](http://i.gyazo.com/e2a3997f0ed36ef09626cac34888a42a.png)

