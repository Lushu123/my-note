### 切面的用法（Aspect）

#### 1、代码展示

​	

```java
package com.chen.blog.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpRequest;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Arrays;

/*
* 日志记录：
*   方法在执行之前输出：请求url，ip，方法所在类，方法名，方法参数
* */

@Aspect
@Component
public class LogAspect {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @Pointcut("execution(* com.chen.blog.web.*.*(..))")
    public void log(){
    }

    @Before("log()")
    public void doBefore(JoinPoint joinPoint){
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        String url = request.getRequestURL().toString();//得到访问的url
        String ip = request.getRemoteAddr();//得到访问者的ip
        String classMethod = joinPoint.getSignature() +"." +joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        RequestLog requestLog = new RequestLog(url, ip, classMethod, args);
        logger.info("Request：{}",requestLog);
    }

    @After("log()")
    public void doAfter(){
        logger.info("--------------doAfter--------------");
    }

    @AfterReturning(pointcut = "log()",returning = "result")
    public void doAfterReturning(Object result){
        System.out.println(result);
        logger.info("Request：{}",result);
    }

    private static class RequestLog{
        private String url;
        private String ip;
        private String classMethod;
        private Object[] args;

        @Override
        public String toString() {
            return "RequestLog{" +
                    "url='" + url + '\'' +
                    ", ip='" + ip + '\'' +
                    ", classMethod='" + classMethod + '\'' +
                    ", args=" + Arrays.toString(args) +
                    '}';
        }

        public RequestLog(String url, String ip, String classMethod, Object[] args) {
            this.url = url;
            this.ip = ip;
            this.classMethod = classMethod;
            this.args = args;
        }
    }
}

```

#### 2、分析

- 定义一个切面。即在一个类上加@Aspect注解

- 定义切入点用来确定哪些类需要代理

  ```java
  @Pointcut("execution(* com.chen.blog.web.*.*(..))")
      public void log(){
      }
  ```

  execution语法：

   1、execution(): 表达式主体。

   2、第一个星号：表示返回类型，*号表示所有的类型。

   3、包名：表示需要拦截的包名。如果包名后加两个点表示： 当前包和当前包的所有子包 

   4、第二个星号：表示类名，*号表示所有的类。

   5、*(..):最后这个星号表示方法名，星号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数。

- JoinPoint类： 封装了SpringAop中切面方法的信息,在切面方法中添加JoinPoint参数,就可以获取到封装了该方法信息的JoinPoint对象. 

  | 方法名                    | 功能                                                         |
  | ------------------------- | ------------------------------------------------------------ |
  | Signature getSignature(); | 获取封装了署名信息的对象,在该对象中可以获取到目标方法名,所属类的Class等信息（目标方法所在类） |
  | Object[] getArgs();       | 获取传入目标方法的参数对象                                   |
  | Object getTarget();       | 获取被代理的对象                                             |
  | Object getThis();         | Object getThis();                                            |

  

