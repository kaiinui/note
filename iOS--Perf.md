Perf
===

キホン
---

iPhone のリソースはかなり改善されてきた。
キホンは、Main Thread で重い計算 / ブロックする処理を行わないだけ！
`dispatch_async` で簡単にバックグラウンドスレッドにも持っていけるので、楽ちん。

画像
---

### 画像は GPU を使って縮小する

CoreImage を使うと、GPU を使って画像をリサイズすることが出来る。

- CoreImage - GPUを使って画像のリサイズ - Qiita : http://qiita.com/blurrednote/items/0d0c52a0d6c2bef1be72

RecyclerView 関連
---

### UIImage を CALayer を使って描画する

Control を扱わなくて良い部分（画像を表示するだけで良い）場合は、`CALayer` を使って描画したほうが速い。

- A-Liaison BLOG: CALayer を使って UIImage を描画する : http://akisute.com/2010/08/calayer-uiimage.html

### Background Thread で画像を縮小してからメインスレッドに持ってくる

`UIImageView - setImage:` で暗黙的に画像が縮小される。

```objc
// image: UIImage

dispatch_async(backgroundQueue, ^{
    UIImage *resizedImage = [image your_resizedImage:CGSizeMake(100, 100)];

    dispatch_async(dispatch_get_main_queue(), ^{
        [cell.imageView setImage:resizedImage];
    });
});
```

リサイズは GPU で！

### AsyncDisplayKit を使う

View の純計算部分を Background Thread で扱える。
既に計算が終わった View を Main Thread で突っ込むだけなので軽い。（Paper はコレ。）

Core Data 関連
---

### Background Thread で動かす

MagicalRecord を使えばイイ感じにつくれる。

`contextForCurrentThread` を使う。
