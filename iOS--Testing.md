Testing
---

- Unit: `Specta`, `Expecta`, `OCMockito`
- E2E: `KIF`, `Appium`

#### `NSUserDefaults` をリセット

```objc
        NSString *appDomain = [[NSBundle mainBundle] bundleIdentifier];
        [[NSUserDefaults standardUserDefaults] removePersistentDomainForName:appDomain]
```
