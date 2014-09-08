NSOperation
===

イイ感じに Operation を動かしてくれる Queue 実装。同時実行数の制御などが出来る。

基本
---

- FIFO である（Queue）
- `NSOperationQueue` に対して `NSOperation` を `- addOperation:` することで Operation を start させることが出来る。
- cancel をすることが出来る
- 優先度を付けることが出来る
- 実行精度を決めることが出来る（最悪 Operation が死んでもいいか）
- 各 Operation に dependency を設定して走らせることが出来る。
- completionBlock をとれる
- 各 Operation 毎に勝手にメモリの許す限りスレッドを分ける
- Async な Task にも使える

使い方
---

`NSOperation` を継承し、 `- main` を実装する。

その際、下の各 `keypath` を適切に管理する。

* `isReady`
* `isExecuting`
* `isFinished` - 終了したときに `YES` にする。
* `isCancelled` - キャンセルされたときに `YES` になっている。自前でキャンセルをする。 

Queue のスレッド
---

```
NSOperationQueue *myQueue = [[NSOperationQueue alloc] init];
```

に突っ込めば Background Thread で

```
NSOperationQueue *mainQueue = [NSOperationQueue mainQueue];
```

に突っ込めば Main Thread で動く。

`NSOperationQueue`
---

`maxConcurrentOperationCount` を弄ることで最大同時実行数を制御出来る。

`[NSOperationQueue mainQueue]` に雑に突っ込むと、 `maxConcurrentOperationCount = 1` であり、かつ main thread で動く Operation になる。

なので、自分で `[[NSOperationQueue alloc] init];` して持つ。持ち方は、Lifecycle に巻き込まれてはいけないので、`Manager` 的クラスを Singleton で持つのが良いと思う。

Asynchronous Operation
---

Synchronous Operation で雑にやる場合は、`- main` をオーバーライドするだけでよい。 `- main` の return で勝手に Operation を終わり、 dealloc まで済ませてくれる。

ただし、Asynchronous Operation とか、色々非同期になっていて `- main` の return で死んでほしくない場合は、次のようにやる。

#### `isFinished` をオーバーライドする

1. `@interface` で `isFinished` をオーバーライドする。
2. 処理を終了させたい所で、 `self.isFinished = YES` とする。

これだけで Asynchronous Operation 対応できる。

```objc
// KISomeOperation.h

@interface KISomeOperation : NSOperation

@property (nonatomic, assign) BOOL isFinished;

@end
```

```objc
// KISomeOperation.m

- (void)main {
    @weakify(self);
    [someThing doSomeThingAsynchronouslyWithBlock:^{
        @strongify(self);
        self.isFinished = YES;
    }];
}
```

注意点は、`@weakify` しないと循環参照して Operation が死なない。

テスト
---

[TODO] あとで書く

疑問
---

- `NSOperationQueue` は誰が持てばいいんだろう？または Singleton 的に使う？
  * -> Model と同じく、Lifecycle に巻き込まれないために、`OperationManager` とか `OperationService` 的なクラスを Singleton で持つ。役割はただ `NSOperationQueue` を retain するだけ。

References
---

- http://stackoverflow.com/questions/19569244/nsoperation-and-nsoperationqueue-working-thread-vs-main-thread
- http://nshipster.com/nsoperation/
- NSOperation Class Reference : https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOperation_class/Reference/Reference.html
- Core Data Programming Guide: Concurrency with Core Data : https://developer.apple.com/library/mac/documentation/cocoa/conceptual/coredata/articles/cdConcurrency.html
- Working with the NSOperationQueue Class - Tuts+ Code Tutorial : http://code.tutsplus.com/tutorials/working-with-the-nsoperationqueue-class--mobile-14993
- もう怖くないCocoaの並列処理（GCD & NSOperation/NSOperationQueue） - $ cat /var/log/shin : http://shin.hateblo.jp/entry/2014/05/05/234912
