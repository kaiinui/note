こんなんを入れることでライセンス表記を自動化出来る

```rb
post_install do | installer |
  require 'fileutils'
  FileUtils.cp_r('Pods/Pods-acknowledgements.plist', 'film/Settings.bundle/Acknowledgements.plist', :remove_destination => true)
end
```

これを入れなければ無理。(分かれた？)

- https://github.com/CocoaPods/cocoapods-install-metadata

なんか `0.34.0.rc2` では色々無理だった

References
---

- http://d.hatena.ne.jp/KishikawaKatsumi/20140211/1392111037
