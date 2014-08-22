Aspect Oriented
---

https://github.com/steipete/Aspects

```objc
// NSObject+Aspects

+ (id<AspectToken>)aspect_hookSelector:(SEL)selector
                      withOptions:(AspectOptions)options
                       usingBlock:(id)block
                            error:(NSError **)error;

- (id<AspectToken>)aspect_hookSelector:(SEL)selector
                      withOptions:(AspectOptions)options
                       usingBlock:(id)block
                            error:(NSError **)error;
```

最高のやつがある。

`withOptions` に `AspectPositionBefore` とか付けると `block` が対象メソッドの前に呼ばれる。 `AspectPositionInstead` もある。

`+aspect_hookSelector:withOptions:usingBlock:error;` はクラスメソッドを hook するように見えるけど、対象クラスのインスタンス全てのインスタンスメソッドを hook する。

メソッドが呼ばれる前に Log をつけるようにする
---

```objc
[object aspect_hookSelector:@selector(method:) withOptions:AspectPositionBefore usingBlock: ^(id<AspectInfo> aspectInfo) {
    NSLog(@"%@ %@", aspectInfo.instance, aspectInfo.arguments);
}, error: NULL];
```

とすることでメソッドが呼ばれる前に, インスタンスと引数配列の description が Log されるようになる.

ちなみにメソッドの名前も欲しい場合は、`sel_getName(selector)` とすることで名前が得られる（`method:withHoge:` みたいな形式）

Block には `id<AspectInfo>` が必ず最初の引数として入る。

```objc
@protocol AspectInfo <NSObject>

/// The instance that is currently hooked.
- (id)instance;

/// The original invocation of the hooked method.
- (NSInvocation *)originalInvocation;

/// All method arguments, boxed. This is lazily evaluated.
- (NSArray *)arguments;

@end
```

`originalInvocation` は `NSInvocation` であり、扱いがけっこうめどんくさい。

メソッド呼び出し前後を計測したり返り値をログしたりしたい
---

`aspectInfo.originalInvocation` をゴニョゴニョする。

#### `NSInvocation` の扱い

1. `[invocation invoke];` でメソッドを実行
2. `[invocation getResult:&buffer]` で結果を `buffer` にぶち込む

実際はかなり扱いがめんどくさい。まず、`(void)` を返すメソッドの `getResult:` をやると落ちる。んで、 `buffer` も返ってくる大きさを見越した大きさを確保して宣言する必要ある。
なので、 `[[invocation methodSignature] methodReturnType];` でまず型を知って…とかやる必要ある。

めんどくさいので [kstenerud/Objective-Gems](https://github.com/kstenerud/Objective-Gems) を使う。

これは、 `NSInvocation` に `-invokeAndReturn` とか生やしてくれて最高。

単に `id result = [invocation invokeAndReturn];` と出来る。

##### ただし

二重 free が発生するので, `NSInvocation+Simple.m` をこんな感じに改造する必要ある。

```objc
NSUInteger length = [[self methodSignature] methodReturnLength];
NSMutableData * dat = [NSMutableData dataWithLength:length];
void * buffer = [dat mutableBytes];
[self getReturnValue:&buffer];
return (__bridge id)(buffer);
```

`NSMutableData` を使うのは、 http://stackoverflow.com/questions/9707233/free-and-malloc-with-nsvalue-and-nsinvocation を参照。


かくして
---

```objc
+ (void)inspect:(SEL)selector of:(NSObject *)object {
#ifdef DEBUG
    id insteadBlock = [self insteadBlock:selector];
    [object aspect_hookSelector:selector withOptions:AspectPositionInstead usingBlock:insteadBlock error:NULL];
#endif
}
```

```objc
+ (id)insteadBlock:(SEL)selector {
    return ^(id<AspectInfo> aspectInfo) {
        [self logBeforeInvoke:aspectInfo ofSelector:selector];
        NSDate *start = [NSDate date];
        id result = [self invoke:aspectInfo.originalInvocation];
        NSTimeInterval milliInterval = [self intervalInMilliSecond:start];
        [self logAfterInvoke:aspectInfo ofSelector:selector withResult:result withInterval:milliInterval];
    };
}
```

とすることが出来た。
