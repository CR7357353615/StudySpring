# Spring-aop拦截器实现
---
## 1.定义一个注解
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Documented
public @interface BindingResultErrors {
}
```

## 2.定义拦截器
```java
@Aspect
@Component
@Order(1) // 执行优先级，值越小，有限度越高
public class BindingResultAspect {

    private static Logger logger = LoggerFactory.getLogger(BindingResultAspect.class);

    @Before("@annotation(annotation的全类名)")
    public void doAfterThrowingAdvice(JoinPoint joinPoint) {
        Object[] params = joinPoint.getArgs();
        for (Object o : params) {
            if (o instanceof BindingResult) {
                BindingResult bindingResult = (BindingResult) o;
                if (bindingResult.hasErrors()) {
                    for (ObjectError err : bindingResult.getAllErrors()) {
                        logger.error(err.toString());
                    }

                    if (bindingResult.getFieldErrorCount() > 0) {
                        FieldError fieldError = bindingResult.getFieldError();
                        throw new IllegalArgumentException(fieldError.getField() + fieldError.getDefaultMessage());
                    }
                }
            }
        }
    }
}
```

这样，使用了@BindingResultErrors的方法都会执行该方法
