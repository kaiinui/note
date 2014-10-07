Over the Air 配信
---

ベータテスト等に。

http://joinswipe.com/producthunt/

↑ リンクをタップして、OK を押すだけでアプリをインストール出来る。
これは、Over the Air 配信と呼ばれる機能。

Over the Air 配信の方法
---

1. SSL がある環境を用意する（Dropbox, S3 など）
2. `.ipa`, `.plist`, `index.html` をそれぞれ用意し、配置する。
3. `index.html` では上記 `.plist` を用いたリンクを貼る `itms-services://?action=download-manifest&url=https://dl.dropboxusercontent.com/u/xxx/xxx.plist`
4. `.plist` は以下の内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>
		<dict>
			<key>assets</key>
			<array>
				<dict>
					<key>kind</key>
					<string>software-package</string>
					<key>url</key>
					<string>#{URL_TO_IPA_FILE}</string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string>#{BUNDLE_IDENTIFIER}</string>
				<key>kind</key>
				<string>software</string>
				<key>title</key>
				<string>#{SOFTWARE_NAME}</string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
```

http://qiita.com/leznupar999/items/de7a53054d043ba19ead

これで完了。

ユーザはこれをタップすると、アプリのインストールされ、アプリを初めてタップした際に「許可しますか」等と出る。

参考文献
---

- SSL契約いらず iOS7.1 AdHoc Dropboxからインストール - Qiita : http://qiita.com/leznupar999/items/de7a53054d043ba19ead
- iOS7.1以降端末へのOTA配信 - Qiita : http://qiita.com/nofrmm/items/62f0adf0268b87e075e6
- Digital publishing support, tablet publishing - Aquafadas : http://helpdesk.aquafadas.com/index.php?/Knowledgebase/Article/View/125/0/how-to-distribute-an-ad-hoc-or-in-house-app-over-the-air
