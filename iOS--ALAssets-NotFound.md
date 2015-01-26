ALAsset, PHAsset の not found
---

`ALAsset`, `PHAsset` にはそれぞれ `URL`, `localIdentifier` によって一意に識別される Asset を探す API が存在する。
これらの API について、Asset が見つからなかったときの処理について記す。

### ALAsset

Assets Library では `ALAssetsLibrary - assetForURL:resultBlock:failureBlock:` を用いて URL によって識別される Asset を探すことが出来る。

このメソッドでは `ALAsset` が見つからなかったとき、`resultBlock` の `ALAsset` にて `nil` が渡される。従って、`resultBlock` の冒頭にて `asset` が `nil` かどうかを判定すれば、見つからなかった時の処理が実装出来る。

`failureBlock` は Photo Library へのアクセスの Permission が拒否された時のみ呼び出されることに注意。

### PHAsset

PhotoKit では `PHAsset + fetchAssetsWithLocalIdentifiers:options:` を用いて、`localIdnetifier` にて識別される Asset を取得することが出来る。

このメソッドでは `PHAsset` が見つからなかったとき、空の `PHFetchResult` が返される。このとき `result.count` が 0 になっているので、これを識別すれば見つからなかったときの処理を実装出来る。

