Testing
===

- Unit: `Specta`, `Expecta`, `OCMockito`
- E2E: `KIF`, `Appium`

### `NSUserDefaults` をリセット

```objc
NSString *appDomain = [[NSBundle mainBundle] bundleIdentifier];
[[NSUserDefaults standardUserDefaults] removePersistentDomainForName:appDomain]
```

KIF
---

### Accessibility Label?

画面には出ないけれど、各 View に結びついている、アクセシビリティ用のラベル。
標準は画面に出てる文字列が設定されてる。
たとえば「Start!」のボタンの Accessibility Label は `@"Start!"` 

Storyboard からは下記の場所で弄れる。

![](https://dl.dropboxusercontent.com/u/7817937/_github/__Film/accesibility_label.png)

または、コード上でこれを変更することが出来る。

```objc
view.accessibilityLabel = @"some_accessibility_label";
```

### `Photo Library` をテストする

方法は二つある。

1. `ALAsstesLibrary`, `PHAssetsLibrary` を mock する
2. Safari でひたすら画像を保存する

1.はなかなかの苦行なので、とりあえず2.でお茶を濁しておくのが良い。

実はシミュレータ上においても、Safari で画像を開き、ロングタップで画像を保存することが出来る。
これは、もちろん `ALAssetsLibrary` などで読み取ることが出来る。

普通の iPhone で撮った写真を Dropbox でアップロードし、片っ端からダウンロードするのが良いと思う。

ただし、シミュレータの端末種別を変えると、ライブラリの内容も変わるので注意。

または、数が多ければ、zip を受け取って Photo Library に全てぶち込む専用のアプリを作って、シミュレータをセットアップするのが良い。

### `UICollectionView` の特定のセルをタップする

```objc
// - _:cellForIndexPath:

// For Testing
cell.accessibilityLabel = [NSString stringWithFormat:@"kif_cell_%d", indexPath.row];
```

```objc
// xxTests.m

[tester tapViewWithAccessibilityLabel:@"kif_cell_1"]; // Taps second cell
```

References
---

- Zillow: モバイルアプリの自動化テストフレームワーク - ワザノバ | wazanova : http://wazanova.jp/items/708
