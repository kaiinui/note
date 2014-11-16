Objectify
---

いい感じにデータアクセスをしてくれる

#### クエリ

```java
SomeClass entity = ObjectifyService.ofy().load().type(SomeClass.class).filter("id", someId).first().now();
```

#### 保存

```java
ObjectifyService.ofy().save().entity(entity).now().
```

簡単。

```java
ObjectifyService.register(SomeClass.class);
```

などと事前にやる必要アリ。

Spring 用の BeanFactory もある。

- marceloverdijk/objectify-appengine-spring : https://github.com/marceloverdijk/objectify-appengine-spring

GCS
---

```java
private final GcsService gcsService =
    GcsServiceFactory.createGcsService(RetryParams.getDefaultInstance());
GcsOutputChannel outputChannel =
    gcsService.createOrReplace(fileName, GcsFileOptions.getDefaultInstance());
ObjectOutputStream oout = new ObjectOutputStream(Channels.newOutputStream(outputChannel));
```

これで OutputStream が取れる。Guava などで

```java
ByteStreams.copy(inputStream, oout);
oout.close();
```

等と書き込んでやる。

`multipart/form-data` からファイルをとるのは、Commons Fileupload などを用いる。

#### ↑ で書き込むとファイルがぶっこわれる

java - Google Cloud Storage createOrReplace file is broken (different size, ...) - Stack Overflow : http://stackoverflow.com/questions/18214346/google-cloud-storage-createorreplace-file-is-broken-different-size

先頭に 20 bytes ほど余計なデータが入るのと、全体的に良く分からないバイトが入って壊れる。

```java
GcsOutputChannel outputChannel = gcsService.createOrReplace(new GcsFilename(kBucketName, filename), options);
InputStream in = item.openStream();

try {
    copy(in, Channels.newOutputStream(outputChannel));
} finally {
    in.close();
    outputChannel.close();
}
```

などとやればおk。つまり、`ObjectOutputStream` を挟まなければおk。

References
---

- objectify-appengine - The simplest convenient interface to the Google App Engine datastore - Google Project Hosting : https://code.google.com/p/objectify-appengine/
- Google App Engine for Java Questions - Google App Engine — Google Cloud Platform : https://cloud.google.com/appengine/kb/java
- Google App Engine ～ クエリとその制限 | R-Labs : http://blog.r-learning.co.jp/archives/448
