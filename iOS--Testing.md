Testing
---

- Unit: `Specta`, `Expecta`, `OCMockito`
- E2E: `KIF`, `Appium`

#### `NSUserDefaults` をリセット

```objc
NSString *appDomain = [[NSBundle mainBundle] bundleIdentifier];
[[NSUserDefaults standardUserDefaults] removePersistentDomainForName:appDomain]
```

#### `UICollectionView` の特定のセルをタップする

```objc
// - _:cellForIndexPath:

// For Testing
cell.accessibilityLabel = [NSString stringWithFormat:@"kif_cell_%d", indexPath.row];
```

```objc
// xxTests.m

[tester tapViewWithAccessibilityLabel:@"kif_cell_1"]; // Taps second cell
```
