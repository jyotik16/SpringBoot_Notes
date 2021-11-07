## Spring mvc Configuraton (ORM) XML
### Register a DispatcherServlet using XML-based Spring configuration

![Diagram](https://3.bp.blogspot.com/-pguKiexbH1I/W-_c7-7AU2I/AAAAAAAAEwo/VKWUVRq3Z7wK3iZTJqfW0BqXkqBc-LCbQCLcBGAs/s1600/springmvc-hibernate5-xml-class-diagram.png)

In Spring MVC, The DispatcherServlet needs to be declared and mapped for processing all requests either using web.xml configuration.
Let's use the below configuration in the web.xml file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
    <display-name>spring-mvc-crud-demo</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring-mvc-crud-demo-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app> 
```
***
##Spring and Hibernate Integration using XML-based Spring configuration

Hibernate configuration used in the example is based on hibernate XML based configuration. this is hibernate integration  with spring application via hibernate template.

Let's create *spring-mvc-crud-demo-servlet.xml* file and add the following configuration to it:
```xml 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd
    ">

	<tx:annotation-driven />
	<bean
		class="org.springframework.jdbc.datasource.DriverManagerDataSource"		name="ds">
		<property name="driverClassName"	value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/springjdbc" />
		<property name="username" value="root" />
		<property name="password" value="root"></property>
	</bean>
	<bean
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean"		name="factory">
		<!-- data source -->
		<property name="dataSource" ref="ds"></property>
		<!-- hibernate properties -->
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQL57Dialect</prop>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
			</props>
		</property>
		<!-- annotated classes -->
		<property name="annotatedClasses">
			<list>
				<value>
					com.spring.orm.entities.Student
				</value>
			</list>
		</property>
	</bean>
  <bean
		class=" org.springframework.orm.hibernate5.HibernateTemplate"
		name="hibernateTemplate">
		<property name="sessionFactory" ref="factory"></property>
	</bean>
	<bean class="com.spring.orm.dao.StudentDao" name="studentDao">
		<property name="hibernateTemplate" ref="hibernateTemplate"></property>
	</bean>
  <bean
		class="org.springframework.orm.hibernate5.HibernateTransactionManager"
		name="transactionManager">
		<property name="sessionFactory" ref="factory"></property>
	</bean>
  <!-- enable the configuration of transactional behavior based on annotations -->
	<tx:annotation-driven transaction-manager="transactionManager" />
</beans>`

```
### Configuring Spring MVC View Resolvers

**Add support for conversion, formatting and validation support:**

```java
<!-- Add support for conversion, formatting and validation support -->
    <mvc:annotation-driven/>
```

Add support for component scanning :

```java
 <!-- Add support for component scanning -->
    <context:component-scan base-package="net.javaguides.springmvc" />
```

Enable configuration of transactional behavior based on annotations:

```java
    <!-- Step 4: Enable configuration of transactional behavior based on annotations -->
    <tx:annotation-driven transaction-manager="myTransactionManager" />
```

Add support for reading web resources: css, images, js, etc:

```java
    <!-- Add support for reading web resources: css, images, js, etc ... -->
    <mvc:resources location="/resources/" mapping="/resources/**"></mvc:resources>
```

 Define Spring MVC view resolver:

```java
 <!-- Define Spring MVC view resolver -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
```