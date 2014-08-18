CocoaPods で自分のライブラリを管理
---

`#{NAME}.podspec` ファイルをルートに置けばOK

```rb
Pod::Spec.new do |s|
  s.name         = "KIIncam"
  s.version      = "0.1.0"
  s.summary      = "Inline camera. No modal to take a picture."
  s.description  = "Inspired by Facebook messenger's inline camera UI. People might not need to open a modal to take a picture."
  s.homepage     = "https://github.com/kaiinui/incam"
  s.license      = "MIT"
  s.author       = { "kaiinui" => "lied.der.optik@gmail.com" }
  s.source       = { :git => "https://github.com/kaiinui/incam.git", :tag => "v0.1.0" }
  s.source_files  = "incam/Classes/**/*.{h,m}"
  s.requires_arc = true
  s.platform = "ios", '7.0'
end
```

中身はこんな感じ。

dependency は `s.dependency "AFNetworking", "~> 2.3"` みたいなのをどんどん足す。

利用
---

`pod 'MyLibrary', :path => '../MyLibrary'`

とかとすることでローカルの Pod を利用出来る。

pod 'MyLibrary', :git => 'https://github.com/stevenpsmith/YahooWeatherService.git', :tag => 'v0.0.3'

みたいにすれば git にあげてる Pod を利用出来る

参考
---

- http://qiita.com/somtd@github/items/9386e2185adfff4c54bc
- http://chariotsolutions.com/blog/post/using-cocoapods-to-manage-private-libraries/
