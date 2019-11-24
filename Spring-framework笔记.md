## 源码学习笔记

### @Configuration作用

1. 表明当前是一个配置类,是bean的源
2. 将@Configuration配置的AppConfig类的beanDefinition属性赋值为full类型，保证AppConfig类型可以转换为cglib类型
3. 将@Configuration配置的AppConfig类由普通类型转换为cglib动态代理类型，最后生成cglib代理对象，通过代理对象的方法拦截器可以解决AppConfig类内部方法bean之间发生依赖调用的时候从容器当中获取，避免多例的出现

### 使用IOC的原因

1. 依赖对象多次创建
  * 通过设计模式解决：单例,工厂
2. 对象多次创建的问题
 * 外部传入依赖对象：依赖注入
 * 方法传入；构造参数；属性反射
3. 设计理念：统一管理对象（bean）的生命周期，自动维护对象的依赖关系

### Bean配置方式

1. @Component+@ComponentScan
2. @Bean+@Configuration
3. 通过xml配置bean
4. 实现FactoryBean方法
5. @Import 实现ImportSelector或ImportBeanDefinitionRegistrar  
6. @ImportResource(“spring.xml”)
7. @Conditional
  *  ImportSelector方式注册的bean，在获取时beanName为全类名，如下：
  ```
  public class MyImportSelector implements ImportSelector {
   @Override
   public String[] selectImports(AnnotationMetadata importingClassMetadata) {
    return new String[]{"com.wqp.study.bean.Dream"};
   }
  }
  获取方式：System.out.println(context.getBean("com.wqp.study.bean.Dream"));  
  ```
  *  ImportSelector或ImportBeanDefinitionRegistrar方式如下：
  ```
  public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

   @Override
   public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
    // 将dream注册到BeanDefinitionMap中去
    registry.registerBeanDefinition("dream", new RootBeanDefinition(Dream.class));
   }
  }
  获取方式：System.out.println(context.getBean("dream"));
  ```
  **引入使用：@Import(value = MyImportBeanDefinitionRegistrar.class)**  

  6. @Conditional


### BeanDefinition
1. BeanDefinition bean定义 承载bean的属性：scope className lazy-init等
2. BeanDefinitionRegistry bean定义的注册器: beanName: id/name, 调用registerBeanDefinition(String beanName, BeanDefinition beanDefinition)
3. beanDefinitionMap 存储注册的bean, k-v的形式存放bean, beanName为key, BeanDefinition为value

### 依赖注入的方式

1. @Autowired 底层是通过属性反射的方式注入
2. beanDefinition.setAutoworeMode(0/1/2/3)
  * 0:默认方式  
  * 1: byName方式，解析setter方法名  
  * 2: byType方式，解析setter方法参数类型  
  * 3：构造器贪婪模式：选择最多参数（bean）进行注入  
3. @Import，实现ImportSelector或ImportBeanDefinitionRegistrar ,这种方式也将对象交给spring容器进行管理



### xml方式注册bean到容器原理

1. 创建beanDefinitionFactory
2. 通过beanDefinitionFactory内部的注册器加载xml配置文件
3. 核心调用registry.regiterBeanDefinition(benaName, beanDefinition)
4. 通过注册器获取beanDefinition对象

java 注册方式注册bean到容器原理

1. 









