Device API
---

Facebook iOS SDK すごい。`- canOpenURL:` を駆使して色んな API を実現している（`YES` or `NO` しか返せないけど…）

例えば、Like API も `- canOpenURL:` を駆使していて、`fblikecache://` みたいなスキームでアクセストークンとか付けてリクエストしてる。それで、Like の状況とか取るみたい。
まじで App as an API
