iOS Development
===

言語の基本仕様
---

- Object の Messaging 教に入信した人が作った言語っぽい。どんなメソッド呼び出しも明示的にメッセージングである。
- nil receiver でのメソッド呼び出しは無視される
- Key-value coding という仕組みがあって、静的言語なのに property が動的に弄れたりする
- Messaging を forward 出来たりする（Ruby の `method_missing` 相当）
- method を swizzling 出来る（メソッドの中身入れ替えられたりする。）
- ARC (Automatic Reference Counting) を用いてのメモリ管理。明示的に `free` する必要は無い。
   * Android は GC 搭載！富豪的！

Blocks の循環参照
---

Blocks, 簡単に循環参照を起こす。

`self` を Block の中で使ってたら既に循環参照。weakify して `weakSelf` とかして頑張る必要ある。

あるいは、 Block を使い終わった後すぐに nil で殺す。

[TOOD] もうちょい調べる

RAC
---

ReactiveCocoa, iOS で Rx 出来る。
出来るのは

- RACSignal で普通に Stream (AFNetworking とかも AF で promise チックなことが出来る)
- Value observation (KVO よりも Stream の処理が楽)
- Stream の map とか filter とか merge とかそういうの
    * Validation で云々とか楽々！

##### 参考

* ReactiveCocoa勉強会関西を開催しました #rac_kansai http://ninjinkun.hatenablog.com/entry/2014/08/03/204348

Data Store
---

CoreData, 生で使うの相当だるいのでこのあたりを使う。

- Realm
- MagicalRecord

###Realm

SQLite alternative な mobile-oriented database.

1 MB くらいの footprint で色々いい感じのデータベース使える。

1. ORM
2. Transaction
3. Thread Safe

ORM が大変良く出来ている。

```objc
// Model
Dog *mydog = [[Dog alloc] init];
mydog.name = @"Rex"; 

// Save
RLMRealm *realm = [RLMRealm defaultRealm];
[realm beginWriteTransaction];
[realm addObject:mydog];
[realm commitWriteTransaction];

// Query
RLMArray *r = [Dog objectsWhere:@"age > 8"];
```

KeyChain
---

安全にデータ保管するの KeyChain を使う。 `LUKeyChainAccess` が相当楽。

http://qiita.com/kaiinui/items/e9bf25daed9bba46d8c7

#### KeyChain Sharing

同じデベロッパーのアプリの間であれば KeyChain のデータを共有することが出来る。

http://qiita.com/itoz/items/cac060f940e67d97ab9d

こういうのはちゃんと使って認証を二度と入力させないようにしたい。

.pbxproj merge
---

.pbxproj がキチガイみたいにコンフリクトするのでこういうのを使う。

https://github.com/simonwagner/mergepbx

Test
---

テスト、`Specta` と `Expecta` 使うとイイ感じに RSpec 風に出来る。大変黒魔術満載でよい。

```objc
#import <Specta/Specta.h>
#define EXP_SHORTHAND
#import <Expecta/Expecta.h>

SpecBegin(AQModel)

describe(@"AQAquasyncModelRequirement", ^{\
    describe(@"-aq_resolveConflict:delta;", ^{
        NSDictionary *delta = @{
                                @"gid": @"aaaaaaaa-e29b-41d4-a716-446655dd0000",
                                @"localTimestamp": @2000000000,
                                @"deviceToken": [AQUtil getDeviceToken],
                                @"isDeleted": @NO
                                };
        it(@"should update from delta", ^{
            AQModel *model = [[AQModel alloc] init];
            model.gid = @"aaaaaaaa-e29b-41d4-a716-446655dd0000";
            [model save];
            [model aq_resolveConflict:delta];
            expect(model.localTimestamp).to.equal(2000000000);
        });
        
        it(@"updated record should not be dirty", ^{
            AQModel *model = [[AQModel alloc] init];
            model.gid = @"aaaaaaaa-e29b-41d4-a716-446655dd0000";
            [model save];
            [model undirty];
            [model aq_resolveConflict:delta];
            expect(model.isDirty).to.equal(false);
        });
    });
});

SpecEnd
```

Mock は `OCMockito`.
