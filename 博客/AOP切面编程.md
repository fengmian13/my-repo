# AOP切面编程

#### 一些知识

切面：处理共同逻辑的模块

```
@Aspect	用在类上，表示这个类是一个切面
```

目标：被切面作用的业务模块

切入点：用于指定那些切面作用于哪些目标组件上，一般用表达式实现。

通知：切面和切入点的执行循序；分为：前置通知，后置通知，最终通知，环绕通知，异常通知

```
前置通知（Before Advice）：
注解：@Before
知识点：在目标方法执行之前执行的通知。常用于记录日志、权限检查等操作。

后置通知（After Returning Advice）：
注解：@AfterReturning
知识点：在目标方法成功执行并返回结果之后执行的通知。适用于记录方法返回值或者进行后续处理。

最终通知（After Advice）：
注解：@After
知识点：在目标方法执行之后执行的通知，无论方法是否抛出异常。用于释放资源等操作。

环绕通知（Around Advice）：
注解：@Around
知识点：环绕通知可以在目标方法执行前后进行自定义操作，甚至可以决定是否执行目标方法。它是功能最强的通知类型，可以用于事务管理、性能监控等。

异常通知（After Throwing Advice）：
注解：@AfterThrowing
知识点：在目标方法抛出异常后执行的通知。用于记录异常日志、处理异常等。
```

### 切面的实现

1、导包

```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
```

2、定义类加注解

`@Aspect`

3、通知后面的表达式

```
execution(*com.qst.springbootsecd.service..*.*(..))
解释如下:
符号含义
execution()
表达式的主体;
第一个”*“符号
表示返回值的类型任意;
com.sample.service.imp1AOP所切的服务的包名，即，我们的业务部分
包名后面的”..“表示当前包及子包
第二个”“表示类名，*即所有类。此处可以自定义，下文有举例
*(..)表示任何方法名，括号表示参数，两个点表示任何参数类型。
```

效果：

#### 前置通知

```java
@Before("execution(* com.example.service.UserService.*(..))")
public void beforeAdvice() {
    // 在目标方法执行前执行的逻辑
}
```

#### 后置通知

```java
@After("execution(* com.example.service.UserService.*(..))")
public void afterAdvice() {
    // 在目标方法执行后执行的逻辑
}
```

#### 返回通知

```typescript
@AfterReturning(pointcut = "execution(* com.example.service.UserService.*(..))", returning = "result")
public void afterReturningAdvice(Object result) {
    // 在目标方法返回结果后执行的逻辑，result 是目标方法的返回值
}
```

#### 异常通知

```java
@AfterThrowing(pointcut = "execution(* com.example.service.UserService.*(..))", throwing = "ex")
public void afterThrowingAdvice(Exception ex) {
    // 在目标方法抛出异常后执行的逻辑，ex 是目标方法抛出的异常
}
```

#### 环绕通知

```java
@Around("execution(* com.example.service.UserService.*(..))")
public Object aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
    // 在目标方法执行前后执行特定的逻辑，可以控制目标方法的执行
    // 执行目标方法
    Object result = joinPoint.proceed();
    // 在目标方法执行后执行的逻辑
    return result;
}
```

一些小tips：

1、在spring容器中JoinPoint可以被自动注入，我们可以用它来帮我们获取调用的类名、方法名

![image-20240805151649091](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20240805151649091.png)



### 切点的实现

**表达式**

 execution([可见性] 返回类型 [声明类型].方法名(参数) [异常类型])

其中：

execution：切入点表达式关键字；

[可见性]：可选，指定方法的可见性，如 public、private、protected 或 *；

返回类型：指定方法的返回类型，如 void、int、String 等；

[声明类型]：可选，指定方法所属的类、接口、注解等声明类型；

方法名：指定方法的名称，支持通配符 *；

参数：指定方法的参数类型列表，用逗号分隔，支持通配符 *；

[异常类型]：可选，指定方法可能抛出的异常类型。

例:

execution(public * com.example.service.UserService.addUser(…))：指定 com.example.service.UserService 类中的 addUser 方法；

execution(* com.example.service.*.*(…))：指定 com.example.service 包下的所有方法；

execution(* com.example.service…*.*(…))：指定 com.example.service 包及其子包下的所有方法；

execution(* com.example.service.UserService.*(String))：指定 com.example.service.UserService 类中所有参数类型为 String 的方法。

此外，切入点表达式还支持 &&（逻辑与）、||（逻辑或）和 !（逻辑非）等运算符，以及 @annotation、@within、@args 等注解限定符。

**注解方式**

#### 内置配置

```java
@Aspect
@Component
public class LogAspect {
    @Around("@annotation(com.xxx.LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

#### 注解配置（Annotation）  

1、先创建一个注解类，将注解类的包名放入Pointcut里，当调用注解时就会运行该切面

```
@Before("@annotation(com.easyjob.annotation.GlobalInterceptor)")
```

2、直接 @Pointcut("@annotation(com.xxx.LogExecutionTime)")

```java
@Aspect
@Component
public class LogAspect {
    @Pointcut("@annotation(com.xxx.LogExecutionTime)")
    private void pointcut() {
        // 切点表达式定义方法，方法修饰符可以是private或public
    }
    @Around("pointcut()")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

#### 公共配置

```java
public class CommonPointcut {
    @Pointcut("@annotation(com.xxx.LogExecutionTime)")
    public void pointcut() {
        // 注意定义切点的方法的访问权限为public
    }
}
@Aspect
@Component
public class LogAspect {
    @Around("com.xxx.CommonPointcut.pointcut()")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```



[文章一（SpringAOP（面向切面编程））](https://blog.csdn.net/qq_52748334/article/details/136860140)