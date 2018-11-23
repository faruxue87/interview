# SpringFramework

# 1 spring-jcl (Spring Commons Logging)

# 2 spring-core 
## 2.1 util
## 2.2 core
### 2.2.1 env
* interface PropertyResolver 顶级接口，解析属性值
    * interface Environment 继承PropertyResolver，新增了获取profiles功能
    * interface ConfigurablePropertyResolver 继承PropertyResolver，新增配置入口，如占位符前缀后缀配置，分隔符配置，类型转换配置等
        * interface ConfigurableEnvironment 继承上述接口，新增设置profile功能，获取属性源迭代器功能
            * class StandardEnvironment：普通环境 StandardServletEnvironment：web环境 StandardReactiveWebEnvironment：reactive环境


* abstract class PropertySource 抽象类，属性源。属性：属性源名称和属性源(例如一个properties，一个map)。功能：从属性源中获取属性值
    * abstract class EnumerablePropertySource 抽象类，可列举的属性源。继承PropertySource，新增获取所有属性名功能
        * class MapPropertySource 类属性源，属性键值存放在Map<String,Object>当中


* interface PropertySources 属性源迭代器，用于获取属性源
    * class MutablePropertySources 不可变属性源迭代器，继承自PropertySources，使用CopyOnWriteArrayList存放属性源
### 2.2.2 io
* interface ResourceLoader 顶级接口 根据路径获取Resource(这是一个抽象的资源概念，不一定真实存在)
    * interface InputStreamSource 顶级接口 获取资源对应的输入流
        * interface Resource 继承InputStreamSource 新增了获取url uri，是否是文件，文件相关信息获取的功能 
            * interface WritableResource 继承Resource 新增了获取输出流，channel等功能
### 2.2.3 codec 基于MimeType的编解码
* interface Encoder 编码
* interface Decoder 解码
### 2.2.4 convert

# 3 spring-beans
* interface BeanFactory 顶级接口，提供获取bean，beanProvider功能
    * interface HierarchicalBeanFactory 继承BeanFactory，提供获取父容器功能
        * interface ConfigurableBeanFactory 继承HierarchicalBeanFactory，设置父容器，类加载器，类型转换器，BeanPostProcessor，别名，获取bean依赖
    * interface ListableBeanFactory 继承BeanFactory，获取bean数量，扫描bean name等
    * interface AutowiredCapableBeanFactory 继承BeanFactory，创建，销毁，注入，初始化(afterPropertiesSet,@PostConstruct) 前初始化(BeanPostProcessor.postProcessBeforeInitialization)，后初始化(BeanPostProcessor.postProcessAfterInitialization)
        * interface ConfigurableListableBeanFactory继承上述接口 实例化singletonBean，冻结bean配置，获取指定bean定义
        * class DefaultListableBeanFactory
        
* interface FactoryBean 在Autowired阶段会查找注入属性对应的FactoryBean，调用getObject方法进行依赖注入


* interface AliasRegistry 顶级接口，获取，设置，删除bean别名
    * interface BeanDefinitionRegistry 继承AliasRegistry，获取，设置，删除BeanDefinition


* interface BeanDefinition 获取设置bean定义相关属性，例如bean名称，scope作用域，依赖，延迟加载，beanFactory名称，是否primary，MutablePropertySources，初始化方法，销毁方法等
    * interface AnnotatedBeanDefinition 继承BeanDefinition，新增获取注解元数据功能
        * class DefaultListableBeanFactory(GenericApplicationContext类中实例化)，继承上述BeanFactory，BeanDefinitionRegistry接口

* interface BeanFactoryPostProcessor (invokeBeanFactoryPostProcessors阶段执行,可以对BeanFactory进行修改操作，顺序PriorityOrdered，Ordered，NonOrdered)
    * interface BeanDefinitionRegistryPostProcessor 继承BeanFactoryPostProcessor，可对BeanDefinitionRegistry进行修改操作，优先于其他BeanFactoryPostProcessor执行

* interface BeanPostProcessor bean初始化前后执行
    * interface InstantiationAwareBeanPostProcessor 继承BeanPostProcessor bean实例化前后执行
        * interface SmartInstantiationAwareBeanPostProcessor 继承InstantiationAwareBeanPostProcessor
    * interface DestructionAwareBeanPostProcessor 继承BeanPostProcessor bean销毁前后执行

* interface Aware
    * interface BeanClassLoaderAware
    * interface BeanNameAware
    * interface BeanFactoryAware
    
    
# 4 spring-expression


# 5 spring-aop
* interface Advice 切面
    * interface Interceptor 拦截器
        * interface MethodInterceptor 方法拦截器，提供invoke方法
        * interface ConstructorInterceptor 构造函数拦截器，提供construct方法
* interface JoinPoint 连接点，通知下一个拦截器处理proceed()，getThis()
    * interface Invocation 继承JoinPoint，获取方法参数
        * interface ConstructInvocation 获取构造函数
        * interface MethodInvocation 获取方法
* interface Advisor 切面holder
    * interface PointcutAdvisor 连接点Holder
    * interface IntroductionAdvisor 提供切面自身的接口描述，提供ClassFilter判定是否对目标对象进行代理
       

* interface AopProxy getProxy() 获取代理对象
    * class CglibAopProxy
        * class ObjenesisCglibAopProxy
    * class JdkDynamicAopProxy
    
    
* interface AopInfrastructureBean 标记接口，SpringAop基础设施
* class ProxyConfig 创建代理相关的基类，确保代理创建类拥有相同的属性值
    * class ProxyProcessorSupport 设置类加载器，检测检测代理接口的算法
        * class AbstractAutoProxyCreator 这是一个BeanPostProcessor，用于创建代理，指定在interceptor在bean方法前被调用
            * class AbstractAdvisorAutoProxyCreator 根据advisor创建代理
            * class AbstractNameAutoProxyCreator 根据beanName创建代理

# 6 spring-context
* interface ApplicationEventPublisher 顶级接口，发布PayLoadApplicationEvent/ApplicationEvent
* interface MessageSource 顶级接口获取国际化字符串
    * interface ApplicationContext 继承上述接口以及ListableBeanFactory，HierarchicalBeanFactory，EnvironmentCapable
        * interface ConfigurableApplicationContext 添加BeanFactoryPostProcessor，添加ApplicationListener，设置Environment，获取BeanFactory
            * abstract class AbstractApplicationContext 持有parentApplicationContext，List<BeanFactoryPostProcessor>，Environment，List<ApplicationListener>
            
* interface LifeCycle
* interface Phase
    * interface SmartLifeCycle
    
* interface Validator 提供supports和validate方法。
    * interface SmartValidator 提供提供指引的validate方法
        * class SpringValidatorAdapter
        
* interface Error 提供获取boundRootObjectName方法，检测是否存在error，error数量，设置error，添加error等
    interface BindingResult 获取targetBean
    
# 7 spring-tx
@EnableTransactionManagement指定TransactionManagementConfigurationSelector选择ProxyTransactionManagementConfiguration或AspectJTransactionManagementConfiguration进行配置
* interface SavepointManager 创建，释放，回滚至savepoint 
    * interface TransactionStatus 事务状态，是否是一个新的事务，设置rollbackOnly，是否事务已完成，是否有回滚点
    
* interface PlatformTransactionManager 获取事务状态，提交，回滚

* interface TransactionDefinition 方法对应@Transactional注解当中属性值
    * interface TransactionAttribute 对指定异常是否回滚，指定TransactionManager
        * class DefaultTransactionAttribute
        
* interface TransactionAttributeSource 获取TransactionAttribute
    * class AnnotationTransactionAttributeSource
    

# 8 spring-web
# 9 spring-webmvc


---

# spring Web容器启动方式
* 内嵌容器
    * applicationContext的refresh阶段启动tomcat容器。applicationContext当中是否存在servletContext判断是否启动新的web容器。
* war发布
    * SpringServletContainerInitializer(spring-web模块) 继承ServletContainerInitializer被tomcat容器调用
    * 转调WebApplicationInitializer的实现类
        * spring-web提供了 AbstractContextLoaderInitializer 抽象类供继承，注册ContextLoaderListener，通过ContextLoader带起容器
        * spring-boot提供了 SpringBootServletInitializer 抽象类供继承，但不再通过ContextLoader带起容器
    * ApplicationContextInitializer ServletContextApplicationContextInitializer 将ServletContainerInitializer获取的servletContext放入applicationContext当中

---


# applicationContext初始化流程
* postProcessBeanFactory 注册ServletContext容器内定义相关bean，扫描basePackages
* invokeBeanFactoryPostProcessors 通过以下过程准备bean的定义，beanDefinitionNames，beanDefinitionMap
    * ConfigurationClassPostProcessor.postProcessBeanFactory
    * ConfigurationClassParser.doProcessConfigurationClass
* registerBeanPostProcessors 依据上一步准备好的beanDefinition，找出beanPostProcessor，实例化，并注册
* onRefresh 启动web容器
* registerListeners 注册ApplicationListener
* finishBeanFactoryInitialization 实例化单例对象，主要一些beanPostProcessor发挥作用
    * AutowiredAnnotationBeanPostProcessor处理Autowired
    * EnableTransactionManagement注解(选择proxy/aspectJ)->TransactionManagementConfigurationSelector根据proxy/aspectJ选择代理方式，proxy：ProxyTransactionManagementConfiguration
    * InfrastructureAdvisorAutoProxyCreator(AbstractAutoProxyCreator349)postProcessBeforeInstantiation实例化前检测是否直接创建代理bean(advisedBeans)，或者postProcessAfterInitialization初始化后wrap目标source，生成代理
    * InfrastructureAdvisorAutoProxyCreator(AbstractAutoProxyCreator349)获取拦截器
    * SpringTransactionAnnotationParser解析@Transaction注解
    * InfrastructureAdvisorAutoProxyCreator(AbstractAutoProxyCreator352)根据拦截器创建代理
        * advisor:BeanFactoryTransactionAttributeSourceAdvisor
        * pointCut:TransactionAttributeSourcePointcut
        * advice:TransactionInterceptor
* finishRefresh 清Resource缓存，publishEvent