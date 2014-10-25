TestDouble
==

OCMock
---

基本の流れ

```objc
SomeObject *objectMock = [OCMockObject niceMockForClass:[SomeObject class]];

// (1)
[[objectMock expect] someMethod:[OCMArg any] withSomeArg:[OCMArg any]];

// (2)
doSomeThingUsingMock();

// (3)
[objectMock verify];
```

注意点は

1. 最初に `expect` を行う。
2. mock オブジェクトを用いた処理を行う。（ここで、`expect` したメソッドが呼ばれる）
3. 最後に `verify` を行う。

非同期テストの場合は、

`[objectMock verityWithDelay:1.0]` などとする。

OHHTTPStub
---

HTTP request を Stub 出来る。

```objc
[OHHTTPStubs stubRequestsPassingTest:^BOOL(NSURLRequest *request) {
    return YES; // 全てのリクエストを Stub する
} withStubResponse:^OHHTTPStubsResponse *(NSURLRequest *request) {
    // 200 で `test_response.json` の中身を返す
    return [OHHTTPStubsResponse responseWithFileAtPath:@"test_response.json" statusCode:200 headers:nil];
}];
```

失敗させたいときは、Status コードを 400, 500 番台にすれば良い。

```objc
[OHHTTPStubs stubRequestsPassingTest:^BOOL(NSURLRequest *request) {
    return YES; // 全てのリクエストを Stub する
} withStubResponse:^OHHTTPStubsResponse *(NSURLRequest *request) {
    // 200 で `test_response.json` の中身を返す
    return [OHHTTPStubsResponse responseWithFileAtPath:@"500_response.json" statusCode:500 headers:nil];
}];
```

References
---

- Mocking Network Requests With OHHTTPStubs. - Adoption Curve Dot Net : http://adoptioncurve.net/archives/2012/10/mocking-network-requests-with-ohhttpstubs/
- Front Page · OCMock : http://ocmock.org/
- AliSoftware/OHHTTPStubs : https://github.com/AliSoftware/OHHTTPStubs
