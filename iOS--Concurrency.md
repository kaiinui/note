UIKit
---

メインでしか触れない。

AsyncDisplayKit は UIKit を擬似的にバックグラウンドスレッドで扱うことが出来る。

CoreData
---

- A Guide to Core Data Concurrency : http://robots.thoughtbot.com/core-data
- Touchwonders/CoreDataMultiThreading : https://github.com/touchwonders/CoreDataMultiThreading/
- Fast and non-blocking Core Data back-end programming | Touchwonders : http://www.touchwonders.com/fast-and-non-blocking-core-data-back-end-programming/

NSNotification
---

スレッドを共有するので、基本 Main Thread で投げるべき。
