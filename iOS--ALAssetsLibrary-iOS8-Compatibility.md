ALAssetsLibrary が iOS8 で変わったの
---

1. Camera Roll に iOS8 以上で撮った写真しかない
---

`ALAssetsGroupSavedPhotos`

2. `ALAssetsGroupPhotoStream` がない
---

```objc
 [library enumerateGroupsWithTypes:ALAssetsGroupPhotoStream usingBlock:^(ALAssetsGroup *group, BOOL *stop) {
        NSLog(@"WHOA");
        [group enumerateAssetsUsingBlock:^(ALAsset *result, NSUInteger index, BOOL *stop) {
            NSLog(@"%@", result);
            NSLog(@"%@", [result valueForProperty:ALAssetPropertyDate]);
        }];
        
    } failureBlock:^(NSError *error) {
        NSLog(@"ERROR");
    }];
```

```
2014-09-21 03:36:10.310 assetslibrary[324:18413] WHOA
2014-09-21 03:36:10.319 assetslibrary[324:18413] (null)
2014-09-21 03:36:10.320 assetslibrary[324:18413] (null)
2014-09-21 03:36:10.321 assetslibrary[324:18413] WHOA
```

3. `ALAssetsGroupLibrary` がない
---

```objc
[library enumerateGroupsWithTypes:ALAssetsGroupLibrary usingBlock:^(ALAssetsGroup *group, BOOL *stop) {
        NSLog(@"WHOA");
        [group enumerateAssetsUsingBlock:^(ALAsset *result, NSUInteger index, BOOL *stop) {
            NSLog(@"%@", result);
            NSLog(@"%@", [result valueForProperty:ALAssetPropertyDate]);
        }];
        
    } failureBlock:^(NSError *error) {
        NSLog(@"ERROR");
    }];
```

```
2014-09-21 03:36:10.310 assetslibrary[324:18413] WHOA
2014-09-21 03:36:10.319 assetslibrary[324:18413] (null)
2014-09-21 03:36:10.320 assetslibrary[324:18413] (null)
2014-09-21 03:36:10.321 assetslibrary[324:18413] WHOA
```

全体的に 8 以降に撮った写真しか、とれないっぽい
---

手持ちの iPhone 4S は 7 時代にアルバムを作っていないっぽいので、 `ALAssetsGroupAll` でやっても `ALAssetsGroupSavedPhotos` でしか enumerate 出来ず、やはり iOS8 以降の写真しかとれない。

4. `ALAssetsGroupSavedPhotos` は「最近追加した項目」になっている
---

description を呼ぶと

```
ALAssetsGroup - Name:Recently Added, Type:Saved Photos, Assets count:12
```

5. `ALAssetsGroup` の `- valueForKey:` で死ぬ
---

```objc
 NSLog(@"%@", [group valueForKey:ALAssetsGroupPropertyName]);
 NSLog(@"%@", [group valueForKey:ALAssetsGroupPropertyPersistentID]);
 NSLog(@"%@", [group valueForKey:ALAssetsGroupPropertyURL]);
 NSLog(@"%@", [group valueForKey:ALAssetsGroupPropertyType]);
```

全部死ぬ。 

```
Terminating app due to uncaught exception 'NSUnknownKeyException'
```

他色々事案
---

#### ALAssetsLibrary は写真をキャッシュする

表側でいくら頑張ろうと、 ALAssetsLibrary がフレームワーク側でキャッシュをしてくれるのでメモリが重い

#### `enumerateUsingBlock` が何故か 600KB ほど使う

理由は不明だが何もしなくても 600KB ほど使う。libdispatch が色々してるっぽい。
終わっても開放されない
