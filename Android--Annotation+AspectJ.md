AspectJ と Annotation
===

https://speakerdeck.com/kaiinui/black-magic-in-java

けっこうカンタンに出来る。

Gradle
---

```groovy
// /build.gradle
classpath 'com.uphyca.gradle:gradle-android-aspectj-plugin:0.9.+'

apply plugin: 'android-aspectj'
```

```groovy
// /app/build.gradle
compile 'org.aspectj:aspectjrt:1.8.1'
```

Annotation
---

```java
@Target(METHOD)
@Retention(CLASS)
public @interface PotatoTip {}
```

Aspect
---

メソッドを乗っ取って色々出来る。

```java

@Aspect
public class PotatoAspect {
    @Pointcut("execution(@com.kaiinui.potatoannotation.Potato * *(..))")
    public void method() {}

    @Around("method()")
    public Object executePotato(ProceedingJoinPoint joinPoint) throws Throwable {
        final Activity activity = (Activity) joinPoint.getThis();
        activity.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(activity, "HOGHOGE", Toast.LENGTH_SHORT).show();
            }
        });

        Object result = joinPoint.proceed(); // 分かりやすくするため二段に。 .proceed() で対象のメソッドを実行して返り値を貰える。
        return result;
    }
}
```

これだけで動く
---

#### サンプル

```java
// MainActivity.java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my);

        doSomething();
    }
    
    @Potato
    public void doSomething() {
    }
```

なにもないメソッドを呼んだら…

![](https://dl.dropboxusercontent.com/u/7817937/_github/Screenshot_2014-08-20-18-31-11.png)

黒魔術。

参考文献
---

- https://github.com/JakeWharton/hugo/tree/master/hugo-runtime
