## 源码学习笔记

### @Configuration作用
1. 表明当前是一个配置类,是bean的源
2. 将@Configuration配置的AppConfig类的beanDefinition属性赋值为full类型，保证AppConfig类型可以转换为cglib类型
3. 效@Configuration配置的AppConfig类由普通类型转换为cglib动态代理类型，最后生成cglib代码对象，通过代理对象的方法拦截器可以解决AppConfig类内部方法bean之间发生依赖调用的时候从容器当中获取，避免多例的出现
