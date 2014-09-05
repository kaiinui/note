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

疑問
---

- `NSOperationQueue` は誰が持てばいいんだろう？または Singleton 的に使う？

References
---

- http://stackoverflow.com/questions/19569244/nsoperation-and-nsoperationqueue-working-thread-vs-main-thread
- http://nshipster.com/nsoperation/
- NSOperation Class Reference : https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOperation_class/Reference/Reference.html
- Core Data Programming Guide: Concurrency with Core Data : https://developer.apple.com/library/mac/documentation/cocoa/conceptual/coredata/articles/cdConcurrency.html
- Working with the NSOperationQueue Class - Tuts+ Code Tutorial : http://code.tutsplus.com/tutorials/working-with-the-nsoperationqueue-class--mobile-14993
- もう怖くないCocoaの並列処理（GCD & NSOperation/NSOperationQueue） - $ cat /var/log/shin : http://shin.hateblo.jp/entry/2014/05/05/234912
