ReactiveCocoa
===

`RACSignal *` をイイ感じに弄るのが基本。

```objc
[signal subscribeNext:^(id someSignal){
  // Do something with someSignal
}];
```

Sources of `RACSignal`
---

- `RACObserve(object, keypath)` - KVO の代わりに使える
- `RAC(self.textEdit.text)`
- `AFNetworking+RAC` の `- rac_GET:params:` - レスポンスをシリアライズした結果が value として流される。
- `+ createSignal` で `RACSignal` を作ることが出来る。
- あとは、デザインパターンがある https://github.com/tLewisII/RACExample/blob/master/Pods/ReactiveCocoa/ReactiveCocoaFramework/ReactiveCocoa/RACSignal.h

`RACObserve(object, keypath)` は `object` の `keypath` の value を Observe して、変更されたらその value を signal として流す。

e.g., 

```objc
[[RACObserve(dog, name) subscribeNext:^(id x) {
  NSLog(@"%@", x);
}];

dog.name = @"Pochi";
dog.name = @"Tama";

// => Pochi 
// => Tama
```

操作
---

`- filter` で大体 OK.
基本的にリスト操作芸.

References
---

- http://qiita.com/ikesyo/items/ff0fdc179baa92a144ee - まとめリンク
- Getting Started with ReactiveCocoa | Teehan+Lax : http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/
- RACExample/RACSignal.h at master · tLewisII/RACExample : https://github.com/tLewisII/RACExample/blob/master/Pods/ReactiveCocoa/ReactiveCocoaFramework/ReactiveCocoa/RACSignal.h
