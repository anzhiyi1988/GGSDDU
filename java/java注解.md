# java注解

注解类似标签，给个种类加注解，就是给类贴上一个一个的标签。

元注解也是标签，但是他比较特殊，他是给标签加标签。

注解在定义时需要指定一些行为，这些行为用元标签说明，还要指定一些属性。



## 元注解

元注解有五种：

### @Retention - 保留期

说明一个注解的存活时间。

取值如下：

- RetentionPolicy.SOURCE 注解只在源码阶段保留，在编译器进行编译时它将被丢弃忽视。
- RetentionPolicy.CLASS 注解只被保留到编译进行的时候，它并不会被加载到 JVM 中。
- RetentionPolicy.RUNTIME 注解可以保留到程序运行的时候，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们。



### @Documented

顾名思义，这个元注解肯定是和文档有关。它的作用是能够将注解中的元素包含到 Javadoc 中去。



### @Target

指明注解运用的地方。比如只能张贴到方法上、类上、方法参数上等等，取值如下：

- ElementType.ANNOTATION_TYPE 可以给一个注解进行注解
- ElementType.CONSTRUCTOR 可以给构造方法进行注解
- ElementType.FIELD 可以给属性进行注解
- ElementType.LOCAL_VARIABLE 可以给局部变量进行注解
- ElementType.METHOD 可以给方法进行注解
- ElementType.PACKAGE 可以给一个包进行注解
- ElementType.PARAMETER 可以给一个方法内的参数进行注解
- ElementType.TYPE 可以给一个类型进行注解，比如类、接口、枚举



### @Inherited

父类被贴了一个注解，子类是否也要有这个注解呢？如果父类中贴的注解在定义时由这个元注解，则子类会继承。



### @Repeatable

Persons 是一张总的标签，上面贴满了 Person 这种同类型但内容不一样的标签。把 Persons 给一个 SuperMan 贴上，相当于同时给他贴了程序员、产品经理、画家的标签。

```java
@interface Persons {
    Person[]  value();
}


@Repeatable(Persons.class)
@interface Person{
    String role default "";
}



@Person(role="artist")
@Person(role="coder")
@Person(role="PM")
public class SuperMan{
}
```



## 注解的属性

注解的属性也叫做成员变量。注解只有成员变量，没有方法。注解的成员变量在注解的定义中以“无形参的方法”形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
    int id();
    String msg();
}
```



如果注解中有一个属性，则可以直接赋值：

```java
@TestAnnotation(3)
public class Test {
}
```

如果 有多个，如下：

```java
@TestAnnotation(id=3,msg="hello annotation")
public class Test {
}
```



## 注解的提取

注解与反射

首先获取每个类中的所有注解，

```java
// Test 类中是否有TestAnnotation注解
boolean hasAnnotation = Test.class.isAnnotationPresent(TestAnnotation.class);

// 如果有，获取到这个注解。
if ( hasAnnotation ) {
    TestAnnotation testAnnotation = Test.class.getAnnotation(TestAnnotation.class);
    System.out.println("id:"+testAnnotation.id());
    System.out.println("msg:"+testAnnotation.msg());
}
```

同样的，可以获取方法变量等。都是通过反射完成的。





## 总结

通过编码，找到哪些有指定标签的类、方法、属性，然后对这些类、方法、属性做后续的操作。



## 注解的使用场景





## java预置的注解

### @Deprecated

### @Override

### @SuppressWarnings

### @SafeVarargs