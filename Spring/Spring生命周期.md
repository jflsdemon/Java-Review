### Spring中Bean的生命周期
- 生命周期的顺序
1. 实例化
2. 利用IOC设置属性
---
3. 若实现了BeanNameAware，执行setBeanName方法
4. 若实现了BeanFactoryAware，执行setBeanFactory方法
5. 若实现了ApplicationContextAware，执行setApplicationContext方法
---
6. 若实现了BeanPostProcessor，执行预初始化方法postProcessBeforeInitialization
7. 执行InitializingBean的afterPropertiesSet方法
8. 若设置了init-method，执行
9. 执行BeanPostProcessor，执行后初始化方法postProcessAfterInitialization
10. 执行InstantiationAwareBeanPostProcessor的postProcessAfterInitialization（可忽略）
    - 此时beam实例已经可以使用，执行业务逻辑
---
11. 执行DisposibleBean的destory方法
12. 执行destory-method方法
---

#### 但我在app-maven-spring 的edu.demon.beanlife中测试时，发现6，9，10三步没有执行。不知道哪里出了问题，遗留。

- 完整的流程如下
![](http://images.cnitblog.com/i/580631/201405/181453414212066.png)
![](http://images.cnitblog.com/i/580631/201405/181454040628981.png)

> https://www.cnblogs.com/zrtqsk/p/3735273.html
---


- 各种接口方法分类
1. Bean自身方法
    - Bean自身调用
    - 配置文件中的init-method
    - destroy-method
2. Bean级生命周期接口方法
    - BeanNameAware
    - BeanFactoryAware
    - InitializingBean
    - DisposableBean
3. 容器级生命周期接口方法
    - InstantiationAwareBeanPostProcessor
    - BeanPostProcessor 
        - 一般称这两个的实现类为“后处理器”。
4. 工厂后处理器接口方法
    - AspectJWeavingEnabler
    - ConfigurationClassPostProcessor,
    - CustomAutowireConfigurer
        - 等等非常有用的工厂后处理器接口的方法。工厂后处理器也是容器级的。在应用上下文装配配置文件之后立即调用。