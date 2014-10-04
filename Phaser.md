Phaser
===

MIT ライセンスの HTML5 2D ゲームフレームワーク

MVP のコード

```js
var game = new Phaser.Game(800, 600, Phaser.AUTO, 'phaser-example', { preload: preload, create: create });

function preload() {
    game.load.image('mushroom', 'assets/sprites/mushroom2.png');
}

function create() {
    var test = game.add.sprite(200, 200, 'mushroom');
}
```

（実際は Haxe とか TypeScript で書く。）

かなりミニマルにかけていい感じ。コード量としては enchant.js とかとあんまり変わらない感ある。

![](http://cdn-ak.f.st-hatena.com/images/fotolife/s/syonx/20140705/20140705093433.png)

Flappy Bird とか簡単にかける

Compatibility
---

> Internet Explorer 9+, Firefox, Chrome, Safari and Opera. It also works on mobile web browsers including stock Android 2.x browser and above and iOS5 Mobile Safari and above

パフォーマンス
---

Pixi.js を描画エンジンとして用いていて、

- Hardware-Accelaration aware である
- DOM + CSS anim で動くモード
- WebGL / 2D で動くモード
- とにかくパフォーマンスが良い

Footprint
---

576 KB くらいある。128 KB gzipped.

enchant.js みたいなミニマルなやつも本体だけで 220 KB くらい. プラグインとか physics とか付けると 1MB 簡単に超えるので他と比較したら相当軽い。

576 KB というのも物理エンジン（3 種類）込みの話であって、抜くと 311 KB minified, 67 KB gzipped とかになるとのこと.軽い。

標準でサポートされてる機能
---

- Preloaders
- Sprite
- **Physics**
- **Collision Detection**
- Animatin
- Particle
- Tilemap

などなど

足りなそうなの

- ネットワーク関連無いので、自前で書く必要ある。
- `Scene` が何故か無いので `State` をイイ感じに使ってちょめちょめする必要がある。
- クソ重い Phaser をロードするブートストラッパーが無いので、ロード画面とか良い感じにするには bootstrapper を書く必要がある
 
###Sprite

`Sprite` はけっこう充実していて、`.angle` とかある。これを弄るだけでスプライトが回転してくれるっぽい（回転の transition とか設定できるのかな？）
Flappy Bird の bird の角度とかも簡単に出来る。

もちろん Sprite Animation も対応している。

チュートリアルとか
---

- Flappy Bird をつくる http://blog.lessmilk.com/make-html5-games-with-phaser-1/

http://syonx.hatenablog.com/entry/2014/07/05/103418 にまとまっている

エディタ
---

- TypeScript, JavaScript ともに WebStorm が良い

色んな対応状況
---

WebGL 描画がハイパフォーマンスで、対応してないブラウザは自動的に Canvas 描画にフォールバックされる。

モダンブラウザは殆ど対応。モバイルは

- Safari -> 8 以上が WebGL 描画 (Photon Storm » Blog Archive » A first look at what iOS8 means for Phaser and Pixi.js (hint: bunnies, LOTS of them!) : http://www.photonstorm.com/html5/a-first-look-at-what-ios8-means-for-phaser-and-pixi-js-hint-bunnies-lots-of-them)
- Android は最新の Chrome がWebGL 描画。標準ブラウザは Canvas

参考リンク
---

- HTML5ゲームエンジン「Phaser」が初心者とモバイルにやさしくていい感じです http://syonx.hatenablog.com/entry/2014/07/05/103418
- Phaser まとめ http://syon-wiki.herokuapp.com/Phaser
