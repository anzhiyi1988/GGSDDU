cotroller控制器其中三种开发模式（大于三种，只学习了基础的三种）。

第一种

实现org.springframework.web.servlet.mvc.Controller;

 

第二种

继承org.springframework.web.servlet.mvc.multiaction.MultiActionController;

 

第三种

在bean的配置中使用代理，把自己写的类，让MultiActionController去代理。



 

```xml
    <bean name="userDelegate" class="edu.anzhy.delegateac.UserDelegate" />
    <bean name="/user2/**"
        class="org.springframework.web.servlet.mvc.multiaction.MultiActionController">
        <property name="delegate" ref="userDelegate" />
        <property name="methodNameResolver" ref="parameterMethodNameResolver" />
    </bean>
```

注解式控制器的开发模式





通过标签方式注入

@Autowired  以类型查找  spring提供

@Resource 以名字查找 j2ee提供

 

Spring 通过一个 BeanPostProcessor 对 @Autowired 进行解析，所以要让 @Autowired 起作用必须事先在 Spring 容器中声明 AutowiredAnnotationBeanPostProcessor Bean。   

<!-- 该 BeanPostProcessor 将自动对标注 @Autowired 的 Bean 进行注入 -->

<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/> 

标签@Autowired的域变量去掉相应的get 和set方法

在applicatonContext.xml中 把原来 引用的<porpery >标签也去掉