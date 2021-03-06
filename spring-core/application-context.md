# 所谓的容器

目前我个人认为他只是对DefaultListableBeanFactory的封装而已，他的实现也都比较简单，他也是一个BeanFactory的实现

## ClassPathXmlApplicationContext

看`AbstractXmlApplicationContext`(ClassPathXmlApplicationContext的父类)的这个方法的实现，只是使用了XmlBeanDefinitionReader 来对xml中描述的bean做一个加载，加载的逻辑可以看看 [bean-factory](/spring-core/applicationcontext.md "bean-factory"),登记到BeanDefinitionRegistry之后由BeanFactory操作。所以ApplicationContext只是BeanFactory的一个"代理对象而已"


```
	/**
	 * Loads the bean definitions via an XmlBeanDefinitionReader.
	 * @see org.springframework.beans.factory.xml.XmlBeanDefinitionReader
	 * @see #initBeanDefinitionReader
	 * @see #loadBeanDefinitions
	 */
	@Override
	protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory) throws BeansException, IOException {
		// Create a new XmlBeanDefinitionReader for the given BeanFactory.
		XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);

		// Configure the bean definition reader with this context's
		// resource loading environment.
		beanDefinitionReader.setEnvironment(this.getEnvironment());
		beanDefinitionReader.setResourceLoader(this);
		beanDefinitionReader.setEntityResolver(new ResourceEntityResolver(this));

		// Allow a subclass to provide custom initialization of the reader,
		// then proceed with actually loading the bean definitions.
		initBeanDefinitionReader(beanDefinitionReader);
		loadBeanDefinitions(beanDefinitionReader);
	}
```

重点是在`AbstractRefreshableApplicationContext`中持有一个BeanFactory对象


```
AbstractRefreshableApplicationContext中的抽象方法，这个类初始化好BeanFactory之后就调用这个方法加载bean，
loadBeanDefinitions(DefaultListableBeanFactory beanFactory)

<p>The only method to be implemented by subclasses is {@link #loadBeanDefinitions},
which gets invoked on each refresh. A concrete implementation is supposed to load
bean definitions into the given
{@link org.springframework.beans.factory.support.DefaultListableBeanFactory},
typically delegating to one or more specific bean definition readers.
```



