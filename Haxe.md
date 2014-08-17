Haxe
===

Java みたいな構文で JavaScript が書ける。
型推論があるから Java よりも Scala とか Groovy とか Swift って感じ。

TypeScript は JavaScript に型を付けただけ、って感じ。Haxe はそもそも根本から構文が違う。

JavaScript, 型とか継承をたくさんするクラスシステムとかをあまり使うことが少ないように思うので、どれだけ効果があるんだろう。
現状、既存ライブラリとの兼ね合いが型のある恩恵よりもつらみ、みたいな感じのように思う。

が、HTML5 ゲーム等大規模になってくると恩恵あるのだろうな、というイメージ。

```haxe
class Test {
	static function main() {
		var people = [
			"Elizabeth" => "Programming",
			"Joel" => "Design"
		];
		for (name in people.keys()) {
			var job = people[name];
			trace('$name does $job for a living!');
		}
	}
}
```

エディタ
---

http://old.haxe.org/com/ide

いい感じの IDE は無い。Sublime ぽい。このへん Go と似た感じ。

せっかく型があって補完とかコンパイルエラーいい感じなのに IDE のエコシステムが弱いのは残念。
Sublime もプラグイン次第で色々出来るのだろうか？

https://github.com/HaxeIDE

intellij-haxe とかある. あと atom-autocomplete


ライブラリとの連携
---

Extern を使う。外部の JS ライブラリに明示的に型を付けて Haxe の型システムのエコシステムに取り込むことが出来る。

mizchi 氏による vue.js の extern とかある。

標準ライブラリ
---

色々な標準ライブラリがサポートされてる。

template とかある ([TODO] JS でこれ使うとコンパイル時に template のコードがひっつくのだろうか？フットプリント気になる)

http://old.haxe.org/doc/cross/template

