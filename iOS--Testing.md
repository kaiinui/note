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
