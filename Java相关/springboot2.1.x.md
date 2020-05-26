[toc]

# 目录

## springboot

#### 1.x 和 2.x区别

- 增加一些参数
  - logging.pattern.dateformat
  - logging.file.max-size

- JDK依赖版本变化
  - 2.x 至少需要jdk8
- 第三方包升级
  - spring5.x
  - tomcat8.5
  - Hibernate5.2
- 响应式编程支持：异步非阻塞，事件驱动
  - spring webflux：Flux、Mono
  - 内嵌式nettty服务器支持



#### 主要注解

- @SpringBootApplication：标识应用为springboot应用，开启springboot各项能力
  - @EnableAutoConfiguration：开启自动配置 
  - @ComponentScan：开启包扫描
- @ConfigurationProperties：加载额外的配置文件，可以之前匹配前缀
  - @EnableConfiguraitonProperties：一般配合使用
- @AutoConfigureBefore / @AutoConfigureAfter：控制配置类的顺序



#### 读取配置文件的几种方式

- @Value
- @ConfigurationProperties
- @PropertySource



#### 配置文件加载顺序

> 由包外向包内加载

- 优先加载带profile的文件
  - application-${profile}.properties （包外 -> 包内）
- 加载不带profile的文件
  - application.properties（包外 -> 包内）
- 加载@Configuration 上指定的配置文件
- 加载SpringApplicaiton.setDefaultProperties 指定的默认属性



#### 启动过程

- SpringApplication.run -> new SpringApplication().run
- 开启启动计时器StopWtch
- 获取所有spring启动运行监听器（SpringApplicationRunListener）
- 执行所有监听器的starting() 方法
- 准备应用运行参数 applicationArguments
- 创建Banner打印类
- 创建应用上下文！！！！**这步很重要**
  - 判断应用类型，servlet或者reactive，构建不同的应用上下文实例
    - AnnotationConfigServletWebServerApplicationContext
    - AnnotationConfigReactiveWebServerApplicationContext
- 准备应用上下文
  - set Environment 到上下文
  - 监听器准备上下文（SpringApplicationRunListner）
- 调用refreshContext方法！！！**这步更重要**
  - 调用 refresh 方法，AbstractApplicationContext
    - finishRefresh方法回调servlet或reactive重写的方法
      - 调用WebServer start方法，启动对应Web服务器（TomcatServer/NettyServer）
  - 注册容器shutdown时的钩子程序，Runtime.getRuntime().addShutdownHook
- 启动计时器停止计时，并打印日志
- 执行所有启动运行监听器的started方法
- 执行所有实现ApplicationRunner接口和CommandLineRunner接口的run方法
- 执行所有启动运行监听器的running方法



## springcloud

#### 如何实现刷新@RefreshScope

- 消息总线自动配置 -> 注册环境变更监听器
- 刷新范围自动配置生成ContextRefresher

- refresh 的 endpoint 收到请求
- 调用ContextRefresh 的refresh方法
- 取出改变的keys





[回到顶部](#目录)