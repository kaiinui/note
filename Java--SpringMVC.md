Spring
---

Spring 自体は DI フレームワーク.
Spring MVC という下位プロジェクトが WAF.

旧来は xml をそこそこ書く必要があったが、今は Spring Boot があるのでアノテーションでイイ感じに設定が出来るようになっている。

Spring Boot
---

```java
@Configuration
@ComponentScan(basePackages = "com.kaiinui.someapp")
@EnableAutoConfiguration
class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

これで `@Controller` とかをイイ感じに認識してくれるようになる。

Controller
---

Rails などと違い、Spring MVC では Controller にて path を指定する。

```java
@RestController
public class BookController {
    @RequestMapping("/books/{id}")
    public Book getBook(@PathVariable String id) {
        return new Book("Harry Potter and the Sorcerer's Stone", "J. K. Rowling");
    }
}
```

DI
---

Spring の目玉は DI. 次のように Bean を書いておけば、`@Autowired` で勝手に入ってくれる。

```java
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
    @Bean
    public DataStoreService dataStore() {
        return new DataStoreFactory.getDataStoreService();
    }
}
```

```java
@RestController
public class BookController {
    @Autowired
    DataStoreService dataStoreService;
    
    @RequestMapping("/books/{id}")
    public Book getBook(@PathVariable String id) {
        // ここで dataStoreService を使える。
    }
}
```

モックを外部から突っ込んだり出来てテストに便利。

テスト
---

`SpringJUnit4ClassRunner` を `@RunWith` で指定することでテストが出来るようになる。
あとは Spring Boot の Configuration クラスの指定に `@SpringApplicationConfiguration`.
Web MVC をテストしたい場合は `@WebAppConfiguration` を付ける。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = Application.class)
@WebAppConfiguration
class SomeTests {

}
```

これだけでは HTTP のテストは出来ない。`@IntegrationTest` を付けることで、サーバを起動してくれるようになる。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = Application.class)
@WebAppConfiguration
@IntegrationTest
class SomeIntegrationTest {

}
```
