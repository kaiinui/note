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

利用方法
--

全うな利用方法は、これをタグとして、他の場所から色々なことをする。
ButterKnife などの DI が良い例。

アノテーションの扱い方
---

```java
for (Field field : this.getClass().getDeclaredFields()) {
    InjectView annotation = field.getAnnotation(InjectView.class);
    if (annotation != null){
        Log.i("ANNOTATION", "value:" + annotation.value());
    }
}
```

Java には `Class`, `Method`, `Field` っていう如何にもなクラスが用意されている。これらは、`instance.getClass()`, `class.getMethods()`, `class.getDeclaredFields()` で得ることが出来る。

これらは `field.getName()` とか色々することが出来、色々出来そうな感じが有る。

注意は、`class.getFields()` があるがよくわからないやつが結構返ってくる。`class.getDeclaredFields()` を使う。

こうすることで

```
08-22 17:50:45.409  24891-24891/com.kaiinui.sampleannotation I/MyActivity﹕ textView
08-22 17:50:45.409  24891-24891/com.kaiinui.sampleannotation I/MyActivity﹕ value:2131230721
```

といったログを得ることが出来る。

MVP の ButterKnife
---

```java
@InjectView(R.id.text_view) TextView textView;

// onCreate

for (Field field : this.getClass().getDeclaredFields()) {
    InjectView annotation = field.getAnnotation(InjectView.class);
    Log.i("MyActivity", field.getName());
    if (annotation == null) { return; }
    
    int id = annotation.value();
    View view = this.findViewById(id);
    try {
        field.set(this, view);
    } catch (IllegalAccessException e) { }
}

textView.setText("WHOA!");
```

こんなクソコードで最低限の ButterKnife を作れる。

注意点は

* `getAnnotation()` は Nullable
* `field.set(targetInstance, value)` で `targetInstance` の当該フィールドに `value` を突っ込める (例外は `IllegalAccessException`)
