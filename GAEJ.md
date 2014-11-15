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

References
---

- objectify-appengine - The simplest convenient interface to the Google App Engine datastore - Google Project Hosting : https://code.google.com/p/objectify-appengine/
- Google App Engine for Java Questions - Google App Engine — Google Cloud Platform : https://cloud.google.com/appengine/kb/java
