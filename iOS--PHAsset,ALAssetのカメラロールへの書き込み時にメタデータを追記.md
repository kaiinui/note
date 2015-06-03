メタデータを書き込むことが、`PHAssetChangeRequest` の提供している方法を使うことで可能です。

```objc
[[PHPhotoLibrary sharedPhotoLibrary] performChanges:^{
    PHAssetChangeRequest *request = [PHAssetChangeRequest creationRequestForAssetFromImage:someImage];
    request.creationDate = someDate;
    request.location = someLocation;
} completionHandler:^(BOOL success, NSError *error) {
    completion(nil, error);
}];
```

ALAssetsLibrary は `NSDictionary` でメタデータ（撮影日時、場所）を指定します。ついでに、Orientation も正しく指定しておく必要があります。

```
#import "NSMutableDictionary+ImageMetadata.h"

NSMutableDictionary *metadata = [NSMutableDictionary dictionary];
[metadata setDateOriginal:someDate];
[metadata setLocation:someLocation];
[metadata setImageOrientation:someUIImageOrientation];
```

```objc
[self.library writeImageDataToSavedPhotosAlbum:someImageData metadata:metadata.copy completionBlock:block];
```
