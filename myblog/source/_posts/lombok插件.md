---
layout: post
title: lombok插件
date: 2019-03-17 18:36:45
comments: true
tags: 
    - 插件
    - lombok
---
Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.

<!--more-->

[官网](https://projectlombok.org/)
[源码](https://github.com/rzwitserloot/lombok.git)
[文档](http://jnb.ociweb.com/jnb/jnbJan2010.html)

 ## 思考
* 项目中一旦使用了lombok插件，后来的源码阅读必须安装插件才行，按照一般原则，jar包中应该尽量减少对第三方jar包的依赖。
* 降低了代码可读性
* 大厂应该不会使用，要凑代码量的呀

## 常用注解

### @Data
> Equivalent to {@code @Getter @Setter @RequiredArgsConstructor @ToString @EqualsAndHashCode}.

### @Getter @Setter
> 类或者属性
> 生产getter遵循布尔属性的预定
> 默认生成一个无参构造

```java
@Getter
@Setter
class Xin {
    private String name;
    private Date birthday;
    private boolean like;
}
// 对应的class文件
class Xin {
    public String getName() {
        return this.name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Date getBirhtday() {
        return this.birthday;
    }
    public boolean isLike() {
        return this.like;
    }
    // to-do
    public Xin {}

}
// 用在属性上
@Getter
private String name;
// 编译后
public String getName() { return this.name; }
```

### @NonNull
- 用在属性
- 用作属性的非空检查
- 放在setter方法上，将生成一个空检查，为空抛出NullPointerException
- 默认生成一个无参构造
```java
class Xin {
    @NonNull
    private String name;

    @NonNull
    @Getter
    @Setter
    private String address;
}
// 对应的class文件
class Xin {
    @NonNull
    private String name;

    @NonNull
    public String getAddress() {
        return this.address;
    }

    public void setAddress(@NonNull String address) {
        if (address == null) {
            throw new NullPointerException("address");
        } else {
            this.address = address;
        }
    }

    public Xin {}
}
```
### @ToString
- 使用在类
- 默认生成名称-值的形式输出
- exclude\callSuper 参数
- 默认生成一个无参构造
```java
@ToString
class Xin {
    private String name;
    private String address;
}
// 对应的class文件
class Xin {
    private String name;
    private String address;

    public Xin {}
    
    public String toString () {
        return "Xin(name=)" + this.name + ",address=" + this.address + ")";
    }
```
### @EqualsAndHashCode
- 使用在类
- Generates implementations for the {@code equals} and {@code hashCode} methods inherited by all objects, based on relevant fields.

### @AllArgsConstructor
- 用在类
- 全参数的构造方法
- 默认不提供无参构造

### @NoArgsConstructor
- 用在类
- 默认提供无参构造

### @RequiredArgsConstructor
- 用在类
- 用所有带有 @NonNull 注解的或者带有 final 修饰的成员变量生成对应的构造方法

### @Value
- 用在类
- Equivalent to {@code @Getter @FieldDefaults(makeFinal=true, level=AccessLevel.PRIVATE) @AllArgsConstructor @ToString @EqualsAndHashCode}.


### @Cleanup
- 用在属性前
- 用来保证分配的资源被释放
- 在本地变量上使用该注解，任何后续代码都将封装在try/finally中，确保当前作用于中的资源被释放。默认@Cleanup清理的方法为close，可以使用value指定不同的方法名称

```java
public void testCleanUp() {
    try {
        @Cleanup ByteArrayOutputStream baos = new ByteArrayOutputStream();
        baos.write(new byte[] {'Y','e','s'});
        System.out.println(baos.toString());
    } catch (IOException e) {
        e.printStackTrace();
    }
}
// 等价
public void testCleanUp() {
    try {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        try {
            baos.write(new byte[]{'Y', 'e', 's'});
            System.out.println(baos.toString());
        } finally {
            baos.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### @Synchronized
- 用在类或者实例方法
- 慎用
```java
private DateFormat format = new SimpleDateFormat("MM-dd-YYYY");
@Synchronized
public String synchronizedFormat(Date date) {
    return format.format(date);
}
// 等价
private final java.lang.Object $lock = new java.lang.Object[0];
private DateFormat format = new SimpleDateFormat("MM-dd-YYYY");
public String synchronizedFormat(Date date) {
    synchronized ($lock) {
        return format.format(date);
    }
}
```

### @SneakyThrows
- 用在方法
- 可以将方法中的代码用 try-catch 语句包裹起来，捕获异常并在 catch 中用 Lombok.sneakyThrow(e) 把异常抛出，可以使用 @SneakyThrows(Exception.class) 的形式指定抛出哪种异常
- 需要谨慎使用
```java
/** Throwing IllegalAccessException would normally generate an "Unhandled exception" error if IllegalAccessException, or some parent class, is not listed in a throws clause: */
// 编译error
public void test() {
    throw IllegalAccessException
}
// When annotated with @SneakyThrows, the error goes away
@SneakyThrows
public void test() {
    throw IllegalAccessException
}
```

### @Log
> [官网](https://projectlombok.org/features/log) <br/>
> You put the variant of @Log on your class (whichever one applies to the logging system you use); you then have a static final log field, initialized to the name of your class, which you can then use to write log statements.

- 用在类上
```xml
@CommonsLog
Creates private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);
@Flogger
Creates private static final com.google.common.flogger.FluentLogger log = com.google.common.flogger.FluentLogger.forEnclosingClass();
@JBossLog
Creates private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class);
@Log
Creates private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());
@Log4j
Creates private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class);
@Log4j2
Creates private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);
@Slf4j
Creates private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
@XSlf4j
Creates private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);
```



















