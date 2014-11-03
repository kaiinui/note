Private Pod
---

#### 1.Private Repo を GitHub で作る

適当な名前で作る。

#### 2.Repo を `pod repo add` する

```sh
pod repo add REPO_NAME REPO_URL
```

でレポジトリを CocoaPods client に登録出来る

#### 3.普通に podspec 作る

LICENSE ファイルと、homepage の指定が必須で、かつ homepage はどこからでもアクセス出来なければならないことに注意。
Google とか指定しとく。

#### 4.`pod repo push`

```sh
pod repo push REPO_NAME YOUR_POD.podspec
```

で push 出来る。
