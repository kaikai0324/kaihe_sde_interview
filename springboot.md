# 1.AOP:

AOP采用了两种实现方式：静态织入(AspectJ 实现)和动态代理(Spring AOP实现).

JDK动态代理：Spring AOP的首选方法。每当目标对象实现一个接口时，就会使用JDK动态代理。目标对象必须实现接口。
CGLIB代理：如果目标对象没有实现接口，则可以使用CGLIB代理,通过修改字节码生成子类。

切面对调用方法进行拦截，并在拦截前后进行安全检查、日志、事务等处理，就相当于完成了所有业务功能。



# 2. 基础知识：

1.Dao层：持久层，主要与数据库交互
DAO层首先会创建Dao接口，接着就可以在配置文件中定义该接口的实现类；接着就可以在模块中调用Dao的接口进行数据业务的处理，而不用关注此接口的具体实现类是哪一个类，Dao层的数据源和数据库连接的参数都是在配置文件中进行配置的。
DAO层叫数据访问层，全称为data access object，属于一种比较底层，比较基础的操作，具体到对于某个表的增删改查，也就是说某个DAO一定是和数据库的某一张表一一对应的，其中封装了增删改查基本操作，建议DAO只做原子操作，增删改查。
Dao层中，hibernate用@Repository, MyBatis用@Mapper

2.Entity层：实体层，数据库在项目中的类
主要用于定义与数据库对象应的属性，提供get/set方法,tostring方法,有参无参构造函数。

3.Service层：业务层 控制业务
业务模块的逻辑应用设计，和DAO层一样都是先设计接口，再创建要实现的类，然后在配置文件中进行配置其实现的关联。接下来就可以在service层调用接口进行业务逻辑应用的处理。
Service层叫服务层，被称为服务，粗略的理解就是对一个或多个DAO进行的再次封装，封装成一个服务，所以这里也就不会是一个原子操作了，需要事物控制。
好处：封装Service层的业务逻辑有利于业务逻辑的独立性和重复利用性。

4.Controller层：控制层 控制业务逻辑
具体的业务模块流程的控制，controller层主要调用Service层里面的接口控制具体的业务流程，控制的配置也要在配置文件中进行。
Controler负责请求转发，接受页面过来的参数，传给Service处理，接到返回值，再传给页面。
Controller和Service的区别是：Controller负责具体的业务模块流程的控制；Service层负责业务模块的逻辑应用设计
总结：具体的一个项目中有：controller层调用了Service层的方法，Service层调用Dao层的方法，其中调用的参数是使用Entity层进行传递的。

5、View层
此层与控制层结合比较紧密，需要二者结合起来协同工发。View层主要负责前台jsp页面的表示

底层注解：
SpringBoot程序启动入口一个是SpringApplication.run,一个是@SpringBootApplication注解，这个注解是由三部分组成：
1. @ComponentScan注解，主要用于组件扫描和自动装配。
2. @SpringBootConfiguration注解，这个注解主要是继承@Configuration注解，主要用于加载配置文件。
3. @EnableAutoConfiguration注解，这个注释启用了Spring Boot的自动配置功能，可以自动为您配置很多东西。

@ComponentScan 指定扫描路径
@Configuration //配置类，配置文件，在@Configuration下注册非源码的bean
（proxyBeanMethods = true, 保证多次调用代理对象，值相等）可用于组件依赖
@Bean  //产生一个bean对象，交给spring管理，bean对象被放在ioc容器中。
//给容器中添加组件。以方法名作组件id。返回类型即组件类型。返回的值为组件在容器中保存的对象。
//配置类里面使用@Bean标注在方法上给容器注册组件

@Component  //它是一个组件，可用于注册源码的bean，下面@Controller@Service@Repository同
@Controller //它是一个控制器
@Service  //它是一个业务逻辑组件
@Repository //它是一个数据库层组件

@Conditional // 满足conditional指定的条件，则进行组件注入





spring security:web权限控制，分认证和授权
userDetailsService接口,使用数据库里数据设置web认证的用户名密码





1、domain知识（如Project.java）
@Id // 主键
@GeneratedValue(strategy = GenerationType.IDENTITY) // 自增长策略

@Column(nullable = false) // 映射为字段，值不能为空

FetchType.LAZY：懒加载，加载一个实体时，定义懒加载的属性不会马上从数据库中加载。
FetchType.EAGER：急加载，加载一个实体时，定义急加载的属性会立即从数据库中加载。
比方User类有两个属性，name跟address，就像百度知道，登录后用户名是需要显示出来的，此属性用到的几率极大，要马上到数据库查，用急加载；
而用户地址大多数情况下不需要显示出来，只有在查看用户资料是才需要显示，需要用了才查数据库，用懒加载就好了。所以，并不是一登录就把用户
的所有资料都加载到对象中，于是有了这两种加载模式。

@OneToMany(mappedBy = "author",cascade=CascadeType.ALL,fetch=FetchType.LAZY)
    //级联保存、更新、删除、刷新;延迟加载。当删除用户，会级联删除该用户的所有文章
    //拥有mappedBy注解的实体类为关系被维护端
     //mappedBy="author"中的author是Article中的author属性
@ManyToOne(cascade={CascadeType.MERGE,CascadeType.REFRESH},optional=false)//可选属性optional=false,表示author不能为空。删除文章，不影响用户
    @JoinColumn(name="author_id")//设置在article表中的关联字段(外键)




2、exceptions知识
@ControllerAdvice + @ExceptionHandler 全局处理 Controller 层异常
@ResponseStatus(HttpStatus.BAD_REQUEST) 此注解一定会抛出异常，如无则正常运行至完毕，配合@ExceptionHandler注解



3、repository知识



4、services知识
ProjectTaskService.java:
try {1⃣️} catch(exception e){2⃣️}
1⃣️语句出现错误异常时，执行2⃣️




5、web(如ProjectController)
@RequestMapping：将HTTP请求映射到MVC和REST控制器的处理方法上，即映射url
@PostMapping(value = "/user/login") == @RequestMapping(value = "/user/login",method = RequestMethod.POST)
RESTful API:
@GetMapping，处理get请求
@PostMapping，处理post请求
@PutMapping，处理put请求
@DeleteMapping，处理delete请求
@PatchMapping，用于资源的部分内容的更新

@PathVaribale 获取url中的数据
@RequestParam 获取请求参数的值
使用@RequestParam时，URL是这样的：http://host:port/path?参数名=参数值
使用@PathVariable时，URL是这样的：http://host:port/path/参数值





spring aop实现原理：动态代理

JDK动态代理(Dynamic Proxy)：
基于标准JDK的动态代理功能
只针对实现了接口的业务对象

CGLIB：
通过动态地对目标对象进行子类化来实现AOP代理，
需要指定@EnableAspectJAutoProxy(proxyTargetClass = true)来强制使用
当业务对象没有实现任何接口的时候默认会选择CGLIB