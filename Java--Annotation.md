Annotation
===

```java
public @interface InjectView {}
```

などとすることで Annotation を作ることが出来る。

```java
public @interface InjectView {
    int id();
}
```

とすると、 `@InjectView(id=3)` などと使うことが出来る。

`id=` とか付けたくない場合は `value()` とすれば

```
public @interface InjectView {
    int value()
}
```

`@InjectView(R.id.text_view)` などと使える。

@Retention と @Target
---

`@Retention` はアノテーション宣言(@interface)に付けるアノテーション。
どの段階まで情報を保持すべきかを宣言する。（`RUNTIME` であれば実行時まで保持するが、`SOURCE` だとビルド時に失われる。`@NonNull` みたいなのはこれ）

`@Target` はどんなものにアノテーションを付けられるかを宣言出来る。
`FIELD`, `METHOD`, `CLASS` などがある。 `ANNOTATION` というのもあってイイ感じに楽しいことが出来る。

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface InjectView {
    int value();
}
```

こんな感じ。

これで呼び出し側からは `@InjectView(R.id.text_view) TextView textView;` という風に出来る。

