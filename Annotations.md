# Spring Core

## What is an API for a website?
## what is API in web development
>An API (Application Programming Interface) is a set of functions that allows applications to access data and interact with external software components, operating systems, or microservices.

## @Bean 
[visit bean ](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Bean.html)

## Life Cycle
![img](https://media.geeksforgeeks.org/wp-content/uploads/20200428011831/Bean-Life-Cycle-Process-flow3.png)

***

>This annotation is used at the method level. @Bean annotation works with @Configuration to create Spring beans. As mentioned earlier, @Configuration will have methods to instantiate and configure dependencies. Such methods will be annotated with @Bean. 
>
>Indicates that a method produces a bean to be managed by the Spring container.
***

## @Configuration 
>This annotation is used on classes which define beans. @Configuration is an analog for XML configuration file – it is configuration using Java class. Java class annotated with @Configuration is a configuration by itself and will have methods to instantiate and configure the dependencies.
>
***
## @Autowired
>This annotation is applied on fields, setter methods, and constructors. The @Autowired annotation injects object dependency implicitly.
>
>When you use @Autowired on fields and pass the values for the fields using the property name, Spring will automatically assign the fields with the passed values.
>
```java
public class Customer {
    @Autowired                               
    private Person person;                   
    private int type;
}
```
>When you use @Autowired on setter methods, Spring tries to perform the by Type autowiring on the method

```java
public class Customer {                                                                                        
    private Person person;
    @Autowired                                                                                                  
    public void setPerson (Person person) {
     this.person=person;
    }
}
```
>When you use @Autowired on a constructor, constructor injection happens at the time of object creation. It indicates the constructor to autowire when used as a bean.
***
## @Qualifier
>This annotation is used along with @Autowired annotation. When you need more control of the dependency injection process, @Qualifier can be used. @Qualifier can be specified on individual constructor arguments or method parameters. This annotation is used to avoid confusion which occurs when you create more than one bean of the same type and want to wire only one of them with a property.

```Java
@Component
public class BeanB1 implements BeanInterface {
  //
}
@Component
public class BeanB2 implements BeanInterface {
  //
}
Now if BeanA autowires this interface, Spring will not know which one of the two implementations to inject.
One solution to this problem is the use of the @Qualifier annotation.

@Component
public class BeanA {
  @Autowired
  @Qualifier("beanB2")
  private BeanInterface dependency;
  ...
} 
```
***

## @ComponentScan
>This annotation is used with **@Configuration** annotation to allow Spring to know the packages to scan for annotated components. @ComponentScan is also used to specify base packages using basePackageClasses or basePackage attributes to scan. If specific packages are not defined, scanning will occur from the package of the class that declares this annotation.
````
@ComponentScans({ @ComponentScan(basePackages = "com.xflow.engine.server")})
````
***
## @Lazy
>This annotation is used on component classes. By default all autowired dependencies are created and configured at startup. But if you want to initialize a bean lazily, you can use @Lazy annotation over the class. This means that the bean will be created and initialized only when it is first requested for. You can also use this annotation on @Configuration classes. This indicates that all @Bean methods within that @Configuration should be lazily initialized.
***
## @Value
>This annotation is used at the field, constructor parameter, and method parameter level. The @Value annotation indicates a default value expression for the field or parameter to initialize the property with. you can use @Value annotation to inject values from a property file into a bean’s attribute. It supports both #{...} and ${...} placeholders.

## What is the use of stereotype annotations in Spring?
These annotations are used to create Spring beans automatically in the application context. Example
@Component . @Controller
***

## @Component
>This annotation is used on classes to indicate a Spring component. The @Component annotation marks the Java class as a bean or say component so that the component-scanning mechanism of Spring can add into the application context.

***
## @Controller
> Marks annotated class as a bean for Spring MVC containing request handler.This annotation can be used to identify controllers for Spring MVC or Spring WebFlux.

>This annotation is used on Java classes that play the role of controller in your application. The @Controller annotation allows autodetection of component classes in the classpath and auto-registering bean definitions for them.
***

## @Controller Vs RestController
We typically use @Controller in combination with a @RequestMapping annotation for request handling methods.

````java
@RequestMapping("books")
public class SimpleBookController {

    @GetMapping("/{id}", produces = "application/json")
    public @ResponseBody Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}java
````
>We annotated the request handling method with @ResponseBody. This annotation enables automatic serialization of the return object into the HttpResponse.

## @RestController 
>is a specialized version of the controller. It includes the @Controller and @ResponseBody annotations, and as a result, simplifies the controller implementation:
```java
@RestController
@RequestMapping("books-rest")
public class SimpleBookRestController {
    
    @GetMapping("/{id}", produces = "application/json")
    public Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}
```
>The controller is annotated with the @RestController annotation; therefore, the @ResponseBody isn't required.
***
## @Service
>This annotation is used on a class. The **@Service** marks a Java class that performs some service, such as execute business logic, perform calculations and call external APIs. This annotation is a specialized form of the @Component annotation intended to be used in the service layer.
***
## @Repository
>This annotation is used on Java classes which directly access the database. The @Repository annotation works as marker for any class that fulfills the role of repository or Data Access Object.

>This annotation has a automatic translation feature. For example, when an exception occurs in the @Repository there is a handler for that exception and there is no need to add a try catch block.

***
## @SpringBootApplication
>This annotation is used on the application class while setting up a Spring Boot project. The class that is annotated with the @SpringBootApplication must be kept in the base package. The one thing that the @SpringBootApplication does is a component scan. But it will scan only its sub-packages. As an example, if you put the class annotated with @SpringBootApplication in com.example then @SpringBootApplication will scan all its sub-packages, such as com.example.a, com.example.b, and com.example.a.x.

>The @SpringBootApplication is a convenient annotation that adds all the following:

- @Configuration
- @EnableAutoConfiguration
- @ComponentScan
***
## @RequestMapping
>This annotation is used both at class and method level. The @RequestMapping annotation is used to map web requests onto specific handler classes and handler methods. When @RequestMapping is used on class level it creates a base URI for which the controller will be used. When this annotation is used on methods it will give you the URI on which the handler methods will be executed.
## @GetMapping
 **@RequestMapping(method = RequestMethod.GET)**
## @PostMapping
**@RequestMapping(method = RequestMethod.POST)**
## @PutMapping
**@RequestMapping(method = RequestMethod.PUT)**
## @PatchMapping
**@RequestMapping(method = RequestMethod.PATCH)**
## @DeleteMapping
**@RequestMapping(method = RequestMethod.DELETE)**

````
@Controller
@RequestMapping("/welcome")
public class WelcomeController{
  @RequestMapping(method = RequestMethod.GET)
  public String welcomeAll(){
    return "welcome all";
  }  
}
````
## @CrossOrigin
>This annotation is used both at class and method level to enable cross origin requests. In many cases the host that serves JavaScript will be different from the host that serves the data. In such a case Cross Origin Resource Sharing (CORS) enables cross-domain communication. To enable this communication you just need to add the @CrossOrigin annotation.

>By default the @CrossOrigin annotation allows all origin, all headers, the HTTP methods specified in the @RequestMapping annotation and maxAge of 30 min. You can customize the behavior by specifying the corresponding attribute values.
***
## @PathVariable
>This annotation is used to annotate request handler method arguments. The @RequestMapping annotation can be used to handle dynamic changes in the URI where certain URI value acts as a parameter. You can specify this parameter using a regular expression. 

***
## @RequestBody
>This annotation is used to annotate request handler method arguments. The @RequestBody annotation indicates that a method parameter should be bound to the value of the HTTP request body. The HttpMessageConveter is responsible for converting from the HTTP request message to object. 

 ## @ResponseBody 
 >annotation indicates that the result type should be written straight in the response body in whatever format you specify like JSON or XML. Spring converts the returned object into a response body by using the HttpMessageConveter.
````
@PatchMapping(value="holidays/{holidayId}")
public ResponseEntity<ApiResponse> updateHoliday(HttpServletRequest httpServletRequest,
		@RequestBody HolidayDto holiday,
        @PathVariable("holidayId") Integer holidayId) {
		
````
***

## @RequestParam
>In Spring MVC, the @RequestParam annotation is used to read the form data and bind it automatically to the parameter present in the provided method. So, it ignores the requirement of HttpServletRequest object to read the provided data.

>Including form data, it also maps the request parameter to query parameter and parts in multipart requests. If the method parameter type is Map and a request parameter name is specified, then the request parameter value is converted to a Map else the map parameter is populated with all request parameter names and values.

**URI-> xflow/assigned-tasks?12**
````
@GetMapping(value = "/assigned-tasks")
public ResponseEntity<ApiResponse> getTasksByUser(@RequestParam Optional<Integer> statusId)
````
***
## @RequestPart
 The @RequestPart annotation can be used instead of @RequestParam to get the content of a specific multipart and bind to the method argument annotated with @RequestPart. This annotation takes into consideration the “Content-Type” header in the multipart(request part).

````
@PostMapping(value = "/create", consumes = { MediaType.APPLICATION_OCTET_STREAM_VALUE,MediaType.MULTIPART_FORM_DATA_VALUE, MediaType.MULTIPART_MIXED_VALUE })
public ResponseEntity<ApiResponse> createWorkflow(HttpServletRequest req,
		@RequestPart(value = "task", required = true) String taskDtoStr,
		@RequestPart(value = "files", required = false) MultipartFile[] files) {`
        
````
***
## @ControllerAdvice
>This annotation is applied at the class level. As explained earlier, for each controller you can use **@ExceptionHandler** on a method that will be called when a given exception occurs. But this handles only those exception that occur within the controller in which it is defined. 

>To overcome this problem you can now use the @ControllerAdvice annotation. This annotation is used to define @ExceptionHandler, @InitBinder and @ModelAttribute methods that apply to all @RequestMapping methods. **Thus if you define the @ExceptionHandler annotation on a method in @ControllerAdvice class, it will be applied to all the controllers.**
