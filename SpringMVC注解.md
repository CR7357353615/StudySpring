# SpringMVC注解
---
## @Controller
控制器Controller负责处理由DispatcherServlet分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model，然后再把该Model返回给对应的View进行展示。

@Controller用于标记在一个类上，使用它标记的类就是一个SpringMVC的Controller对象。
## @RequestMapping
RequestMapping是用来处理请求地址映射的注解，可用于类或方法上。
* 用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径的。
* 用于方法上，表示该方法的请求路径，在前面会加上类的路径（如果有配置）

#### RequestMapping的属性
##### 1.value,method
* value:指定请求的地址
* method:指定请求的method类型
```java
@RequestMapping(value = "/access/direct", method = RequestMethod.POST)
```
##### 2.consumes,produces
* consumes:指定处理请求的提交内容类型（Content-Type），例如application/json,text/html。
```java
@RequestMapping(value = "/test", method = RequestMethod.POST, consumes = "application/xml")
```
* produces:指定返回的内容类型，仅当request请求头中的（Accept）类型中包含该指定类型才返回。
```java
@RequestMapping(value = "/test", method = RequestMethod.POST, consumes = "application/xml")
```
##### 3.params,headers
* params:指定request中必须包含某些参数值时，才能让该方法处理。
```java
@RequestMapping(value = "/list",method=RequestMethod.GET,params="type=one")  
```
* headers:指定request中必须包含某些指定的header值，才能让该方法处理请求。
```java
@RequestMapping(value = "/add", method = RequestMethod.POST, headers = "Accept=application/json")
```

## @Resource和@Autowired
@Resource和@Autowired都是做bean的注入时使用，其实@Resource并不是Spring的注解，需要导入，但是Spring支持该注解的注入。
* 1.共同点
两者都可以写在字段和setter方法上，如果写在字段上，就不需要再写setter方法了。
* 2.不同点
  * @Autowired
  是Spring提供的注解，只按照byType注入，如果需要按名字注入，要搭配@Qualifier注解使用。
  ```java
  @Autowired
  @Qualifie("userService")
  ```
  * @Resource默认按照byName注入，找不到名字，按类型搜索注入。如果使用name属性指定名称，按照byName注入，使用type属性指定类型，则按照byType自动注入。
  ```java
  @Resource(name="userDao")
  ```

## @ModelAttribute和@SessionAttributes
@ModelAttribute用在方法上，在访问这个Controller时，都需要按顺序先执行注解了@ModelAttribute的方法，谨慎使用。可以用在：
* 1.方法上
  * 使用@ModelAttribute注解，方法无返回值：先执行@ModelAttribute注解的方法。
  * 使用@ModelAttribute注解，方法有返回值：先执行@ModelAttribute注解的方法，并且把返回值以key(返回值类型首字母小写)-value(返回值)存入model中。model是org.springframework.ui。如果不想用类型就用@ModelAttribute(value = "num")，就以num-返回值的形式存在model中。

* 2.方法的参数上：从model中取出对应属性的值。
* 3.方法上，并且该方法也使用了@RequestMapping：还是将返回值存在model中，返回。

@SessionAttributes在类上使用，可以使得模型中的数据存储一份到session中。有以下参数：
* 1.name:这是一个字符串数组，里面应写需要存储到session中数据的名称。
* 2.types:根据指定参数的类型，将模型中对应类型的参数存储到session中。
* 3.value：和names类似。
```java
@SessionAttributes (value={ "intValue" , "stringValue" }, types={User. class })
```

## @PathVariable
用于将请求URL中的模板变量映射到功能处理方法的参数上，即取出uri模板中的变量作为参数
```java
@RequestMapping(value="/user/{userId}/roles/{roleId}",method = RequestMethod.GET)  
public String getLogin(@PathVariable("userId") String userId, @PathVariable("roleId") String roleId)
```

## @requestParam
类似于request.getParameter("name")。有三个参数
* defaultValue：默认值
* required：是否必须要传入
* value：接收传入的参数类型

## @ResponseBody
应用在控制器的方法上，返回特定的数据，比如json，xml等。

## @RequestBody
在方法参数中使用，比如接收json，xml等数据。
```java
public User login(@RequestBody User user)
```

## @Component
通用的容器注解

## @Repository
用于Dao层

## @Service
用于service层
