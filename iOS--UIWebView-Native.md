UIWebView アレコレ
===

- `UIWebView` の `UIScrollView` は `webView.scrollView` で参照することが出来る
- `UIWebView` のスクロールは標準で減速しやすいようになっている（WebView 特有のもっさり感の原因の一つ）
  * `.scrollView.decelerationRate` を `UIScrollViewDecelerationRateNormal` にすることでネイティブの速度に
- `UIWebView` のページ領域外のグレーは `.backgroundColor` を弄ることで消せる
- `-webkit-tap-highlight-color` を alpha 0 の色にすることで、タップ時のハイライトを消すことが出来る。
- FastClick を使えばタップ時のディレイを無くすことが出来る

以上をやれば違和感は結構消える。
あとは、ロードの遅さとかを上手く誤摩化す。

References
---

- (52) How do you make UIWebView behave like a native app? - Quora : http://www.quora.com/How-do-you-make-UIWebView-behave-like-a-native-app
- ios - Remove gradient background from UIWebView? - Stack Overflow : http://stackoverflow.com/questions/3009063/remove-gradient-background-from-uiwebview
