- [Cache-Control を設定する](#cache-control)
- [Memcache を効果的に使う](#memcache)
- [AppStats を設定しておく](#appstats)
- [Request, Response 毎に Body をログしておく](#request-log)
- [Google Cloud Storage を使う](#google-cloud-storage)
- [重い処理には Task Queue を使う](#task-queue)
- [独自ドメイン / SSL の設定の仕方](#ssl-domain)

Cache-Control
---

`Cache-Control` ヘッダを設定することで、結果の View 自体を丸ごと Edge キャッシュしてもらうことが出来る。

`Cache-Control: public, max-age:60`

ただし、**Evict 出来ない**, **最終更新時間によらず、クライアントは必ずキャッシュを参照してしまう** ので `max-age` は小さめに抑えた方が良い。

これは Google Cloud Storage も同様のことが可能。

参考: http://qiita.com/sinmetal/items/37c105a098174fb6bf77

Memcache
---

Memcache は重要。なんせ READ/WRITE ともに無料。

Spring の `@Cacheable` とかでやるか、Datastore の Read/Write をハイジャックするとか。
`@Cacheable` を `@RequestMapping` のメソッドに付けることでそもそもの

AppStats
---

AppStats を設定しておくことで、RPC にかかった時間やコストを見える化 / ログすることが出来る。

```xml
<filter>
    <filter-name>appstats</filter-name>
    <filter-class>com.google.appengine.tools.appstats.AppstatsFilter</filter-class>
    <init-param>
        <param-name>logMessage</param-name>
        <param-value>Appstats available: /appstats/details?time={ID}</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>appstats</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

[Blog @vierjp : 17.Appstats(Java)でRPCのコストと処理時間を調べてみよう](http://blog.vier.jp/2013/02/appstatsjavarpc.html)

Request Log
---

標準では Request と Response の Body がログされないので、Servlet Filter とかで毎回ログをフィルタリングしておく。

また、Cron で毎時とか Logs を Fetch して BigQuery とかに飛ばしておくと良い。

Google Cloud Storage
---

[GoogleAppEngine - GAE/jでGCS Default Bucketを使う - Qiita](http://qiita.com/sinmetal/items/f2f7e0fe444b7e000a61)

Google Cloud Storage<br>Java Client Library Overview - Java — Google Cloud Platform : https://cloud.google.com/appengine/docs/java/googlecloudstorageclient/

Task Queue
---

超気楽に Task をキューイングすることが出来る。`TaskManager` クラスとか作って次みたいな感じで呼べるようにしておく。

```java
taskManager.addHogeTask("some", "parameter");

// QueueFactory.getDefaultQueue.add(withUrl("/_task/hoge").addParam("someParam", "some").addParam("hogeParam", "parameter"));
```

SSL Domain
---

04.Google App Engineで独自ドメイン＆SSL(2/2)｜CA Beat エンジニアのブログ : http://ameblo.jp/cabeat-e/entry-11395919479.html

References
---

- Blog @vierjp : 17.Appstats(Java)でRPCのコストと処理時間を調べてみよう : http://blog.vier.jp/2013/02/appstatsjavarpc.html
- Google App Engine / Python 上での開発で最初から知ってればよかった、ってことをいくつか - Masatomo Nakano Blog : http://blog.madoro.org/mn/90
- GoogleAppEngine - GAE/jでGCS Default Bucketを使う - Qiita : http://qiita.com/sinmetal/items/f2f7e0fe444b7e000a61
- appengine ja night 16 BT Frontend cache control : http://www.slideshare.net/bluerabbit777jp/appengine-ja-night-16-bt-frontend-cache-control
- GoogleAppEngine - GCP エッジキャッシュ - Qiita : http://qiita.com/sinmetal/items/37c105a098174fb6bf77
