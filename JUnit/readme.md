https://www.w3cschool.cn/junit/fegu1hv3.html

https://blog.csdn.net/weixx3/article/details/93767257

https://junit.org/junit5/docs/current/user-guide/



# 简介

测试工具，经常与[Mocktio](https://blog.csdn.net/weixx3/article/details/93890555)一起使用



# 注解

使用时的注解有如下内容：



@Test - 这个注解说明依附在 JUnit 的 public void 方法可以作为一个测试案例。

@Before - 有些测试在运行前需要创造几个相似的对象，在 public void 方法加该注释是因为该方法需要在 test 方法前运行。

@After - 如果你将外部资源在 Before 方法中分配，那么你需要在测试运行后释放他们。在 public void 方法加该注释是因为该方法需要在 test 方法后运行。

@BeforeClass - 在 public void 方法加该注释是因为该方法需要在类中所有方法前运行。

@AfterClass - 它将会使方法在所有测试结束后执行，这个可以用来进行清理活动。

@Ignore - 这个注释是用来忽略有关不需要执行的测试的，Junit 在类级别上使用 @Ignore 来忽略所有的测试用例。

@Test(timeout=1000) - 测试用例执行时长大于指定的毫秒数，那么将自动被标记为失败。

@Test(except=RuntimeException.class) - 测试用例抛出RuntimeException则pass。

@RunWith(Parameterized.class) - 注入参数