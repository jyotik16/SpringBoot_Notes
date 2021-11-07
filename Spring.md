## How to configure the Spring Dispatcher Servlet using all Java Code (no xml)?

Answer:
>Good question!
>For Spring MVC, we cover all Java config (no xml) later in the course, complete with videos/explanation and everything.
>However, if you just need the code, here are the steps

>1. Delete the files: web.xml file and spring-mvc-demo-servlet.xml files

>2. Create a new Java package: com.luv2code.springdemo.config

>3. Add the following Java files in your package

````File: DemoAppConfig.java

package com.luv2code.springdemo.config;
 
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
 
@Configuration
@EnableWebMvc
@ComponentScan(basePackages="com.luv2code.springdemo")
public class DemoAppConfig {
 
	// define a bean for ViewResolver
 
	@Bean
	public ViewResolver viewResolver() {
		
		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
		
		viewResolver.setPrefix("/WEB-INF/view/");
		viewResolver.setSuffix(".jsp");
		
		return viewResolver;
	}
	
}
````
````
File: MySpringMvcDispatcherServletInitializer.java

package com.luv2code.springdemo.config;
 
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;
 
public class MySpringMvcDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer { 
	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	} 
	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[] { DemoAppConfig.class };
	} 
	@Override
	protected String[] getServletMappings() {
		return new String[] { "/" };
	} 
}
````
4. Test your app
  Your app should work as desired.
---
I also uploaded a full project implementation with code here

[Visit Code](https://drive.google.com/open?id=1_5__2SggzgFHt7Rs2YYsv5JHRVX5Orq3)
---
For a discussion on how this code works, you can skip ahead to the following video
Video 403 - Spring MVC All Java Config

#### DI Or IOC

**Inversion of control** is a design paradigm with the goal of giving more control to the targeted components of your application, the ones that are actually doing the work.

**Dependency injection** is a pattern used to create instances of objects that other objects rely on without knowing at compile time which class will be used to provide that functionality. 

Inversion of control relies on dependency injection because a mechanism is needed in order to activate the components providing the specific functionality. 

**benefits**  :: Dependency Injection design pattern allows us to remove the hard-coded dependencies and make our application loosely coupled, extendable and maintainable. We can implement the dependency injection pattern to move the dependency resolution from compile-time to runtime.

Some of the **benefits** of using Dependency Injection are Separation of Concerns, Boilerplate Code reduction, Configurable components, and easy unit testing.

### Bean Life Cycle

Spring Beans are initialized by Spring Container and all the dependencies are also injected. When the context is destroyed, it also destroys all the initialized beans.  Spring framework provides support for post-initialization and pre-destroy methods in spring beans.

We can do this in two ways – by implementing `InitializingBean` and `DisposableBean` interfaces or using **init-method** and **destroy-method** attribute in spring bean configurations. Also By using Annotations **@PostConstruct** and **@PreDestroy** For more details, please read [Spring Bean Life Cycle Methods](https://www.journaldev.com/2637/spring-bean-life-cycle).

Spring framework also support `@PostConstruct` and `@PreDestroy` annotations for defining post-init and pre-destroy methods. These annotations are part of `javax.annotation` package. However for these annotations to work, we need to configure our spring application to look for annotations. We can do this either by defining bean of type `org.springframework.context.annotation.CommonAnnotationBeanPostProcessor` or by `context:annotation-config` element in spring bean configuration file.

```java
package com.journaldev.spring.service;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import com.journaldev.spring.bean.Employee;
public class EmployeeService implements InitializingBean, DisposableBean{

	private Employee employee;
	public Employee getEmployee() {
		return employee;
	}
	public void setEmployee(Employee employee) {
		this.employee = employee;
	}	
	public EmployeeService(){
		System.out.println("EmployeeService no-args constructor called");
	}
	@Override
	public void destroy() throws Exception {
		System.out.println("EmployeeService Closing resources");
	}
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("EmployeeService initializing to dummy value");
		if(employee.getName() == null){
			employee.setName("Pankaj");
		}
	}
}
OR
	//pre-destroy method
	public void destroy() throws Exception {
		System.out.println("MyEmployeeService Closing resources");
	}
	//post-init method
	public void init() throws Exception {
		System.out.println("MyEmployeeService initializing to dummy value");
		if(employee.getName() == null){
			employee.setName("Pankaj");
		}
	}
}
```

### init-method OR destroy-method

```xml
<bean name="myEmployeeService" class="com.journaldev.spring.service.MyEmployeeService"
		init-method="init" destroy-method="destroy">
	<property name="employee" ref="employee"></property>
</bean>
```

### @Post-Construct Or @PreDestroy

```java
package com.journaldev.spring.service;
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
public class MyService {
	@PostConstruct
	public void init(){
		System.out.println("MyService init method called");
	}	
	public MyService(){
		System.out.println("MyService no-args constructor called");
	}	
	@PreDestroy
	public void destory(){
		System.out.println("MyService destroy method called");	}
}
```



The `org.springframework.beans` and `org.springframework.context` packages provide the basis for the Spring Framework’s IoC container. 

The `BeanFactory` interface provides an advanced configuration mechanism capable of managing objects of any nature. A `BeanFactory` is like a factory class that contains a collection of beans. The `BeanFactory `holds Bean Definitions of multiple beans within itself and then instantiates the bean whenever asked for by clients. `BeanFactory` is able to create associations between collaborating objects as they are instantiated. This removes the burden of configuration from bean itself.

The `ApplicationContext` interface builds on top of the `BeanFactory` (it is a sub-interface) and adds other functionality such as easier integration with **Spring’s AOP features**, **message resource handling** (for use in internationalization), event propagation, and application-layer specific contexts such as the `WebApplicationContext` for use in web applications.

### How to create ApplicationContext in a Java Program?

There are following ways to create spring context in a standalone java program.

1. **AnnotationConfigApplicationContext**: If we are using Spring in standalone Java applications and using annotations for Configuration, then we can use this to initialize the container and get the bean objects.

2. **ClassPathXmlApplicationContext**: If we have a spring bean configuration XML file in a standalone application, then we can use this class to load the file and get the container object.

3. **FileSystemXmlApplicationContext**: This is similar to ClassPathXmlApplicationContext except that the XML configuration file can be loaded from anywhere in the file system.

   ````xml
   ApplicationContext context = new ClassPathXmlApplicationContext(“bean.xml”);
   ````

**Annotation Based Configuration**: We can also use @Component, @Service, @Repository and @Controller annotations with classes to configure them to be as spring bean. For these, we would need to provide base package location to scan for these classes. For example:

```xml < context:component-scan  base-package = "com.journaldev.spring" />```

**Aspect Oriented Programming** (AOP) compliments OOPs in the sense that it also provides modularity. But the key unit of modularity is aspect than class.

AOP breaks the program logic into distinct parts (called concerns). It is used to increase modularity by **cross-cutting concerns**.

A **cross-cutting concern** is a concern that can affect the whole application and should be centralized in one location in code as possible, such as transaction management, authentication, logging, security etc.

------

#### Why use AOP?

It provides the pluggable way to dynamically add the additional concern before, after or around the actual logic.

**Join point** is any point in your program such as method execution, exception handling, field access etc. Spring supports only method execution join point.


**Advice** represents an action taken by an aspect at a particular join point.

**Pointcut** :It is an expression language of AOP that matches join points.

**Target Object :** It is the object i.e. being advised by one or more aspects. It is also known as proxied object in spring because Spring AOP is implemented using runtime proxies.

**Weaving :** It is the process of linking aspect with other application types or objects to create an advised object. Weaving can be done at compile time, load time or runtime. Spring AOP performs weaving at runtime.

**Aspect:** It is a class that contains advices, joinpoints etc.

### What is Spring IoC Container?

**Inversion of Control** (IoC) is the mechanism to achieve loose-coupling between Object dependencies. To achieve loose coupling and dynamic binding of the objects at runtime, the objects define their dependencies that are being injected by other assembler objects. `Spring IoC container is the program` that injects dependencies into an object and makes it ready for our use.

Spring Framework IoC container classes are part of `org.springframework.beans` and `org.springframework.context` packages and provides us different ways to decouple the object dependencies.

## Spring Advantages
**-Spring Framework can be employed on all architectural layers used in the development of web applications;**
**-Uses the very lightweight POJO model when writing classes;**
**-Allows you to freely link modules and easily test them;**
**-Supports declarative programming;**
**-Eliminates the need to independently create factory and singleton classes;**
**-Supports various configuration methods;**
**-Provides middleware-level service.**
** -Despite the many advantages of Spring Framework, the lengthy preparation process involved in configuring it has contributed to the creation of Spring Boot.**

## Spring Boot Advantages Over Spring

### **Ease of Dependency Management**

To speed up the dependency management process, Spring Boot implicitly packages the required third-party dependencies for each type of Spring-based application and provides them through so-called *starter* packages (spring-boot-starter-web, spring-boot-starter-data-jpa, etc.).

*Starter* packages are collections of handy dependency descriptors that you can include in your application. They allow you to get a universal solution for all Spring-related technologies, removing the necessity to search for code examples and load the required dependency descriptors from them.

For instance, if you want to start using Spring Data JPA to access your database, just include the *spring-boot-starter-data-jpa* dependency in your project and you’ll be done (no need to look for compatible Hibernate database drivers and libraries).

### **Automatic Сonfiguration**

One of the advantages of Spring Boot, that is worth mentioning is automatic configuration of the application. After choosing a suitable starter package, Spring Boot will try to automatically configure your Spring application based on the* jar *dependencies you added. For example, if you add *Spring-boot-starter-web*, Spring Boot will automatically configure registered beans such as *DispatcherServlet*, *ResourceHandlers*, and *MessageSource*.

If you are using *spring-boot-starter-jdbc*, Spring Boot automatically registers the *DataSource*, *EntityManagerFactory, *and *TransactionManager* beans and reads the database connection information from the *application.properties* file.

- **Fast and easy development of Spring-based applications;**
- **No need for the deployment of war files;**
- **The ability to create standalone applications;**
- **Helping to directly embed Tomcat, Jetty, or Undertow into an application;**
- **No need for XML configuration;**
- **Reduced amounts of source code;**
- **Additional out-of-the-box functionality;**
- **Easy start;**  **-Simple setup and management;**
- **Large community and many training programs to facilitate the familiarization period.**


### Native Support for Application Server – Servlet Container
Every Spring Boot application includes an embedded web server. Developers no longer have to worry about setting up a servlet container and deploying an application to it. The application can now run itself as an executable jar file using the built-in server. If you need to use a separate HTTP server, simply exclude the default dependencies. Spring Boot provides separate starter packages for different HTTP servers.