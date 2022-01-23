## Exception  Handler 

**@NotEmpty**, **@Email**

````json
public class EmployeeVO extends ResourceSupport implements Serializable
{
    private static final long serialVersionUID = 1L;
     
    public EmployeeVO(Integer id, String firstName, String lastName, String email) {
        super();
        this.employeeId = id;
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }
 
    public EmployeeVO() {
    }
 
    private Integer employeeId;
 
    @NotEmpty(message = "first name must not be empty")
    private String firstName;
 
    @NotEmpty(message = "last name must not be empty")
    private String lastName;
 
    @NotEmpty(message = "email must not be empty")
    @Email(message = "email should be a valid email")
    private String email;
     
    //Removed setter/getter for readability
}
````

I have created `RecordNotFoundException` class for all such scenarios where a resource is requested by it’s ID, and resource is not found in the system.RecordNotFoundException.java

### Custom Exception Class

````java
package com.howtodoinjava.demo.exception; 
import` org.springframework.http.HttpStatus;`
import` `org.springframework.web.bind.annotation.ResponseStatus;
@ResponseStatus (HttpStatus.NOT_FOUND)
public  class RecordNotFoundException extends RuntimeException 
{public RecordNotFoundException(String exception) 
{super (exception);
}
}
````

````java
import java.util.List;
import javax.xml.bind.annotation.XmlRootElement;
 
@XmlRootElement(name = "error")
public class ErrorResponse 
{
    public ErrorResponse(String message, List<String> details) {
        super();
        this.message = message;
        this.details = details;
    }
 
    //General error message about nature of error
    private String message;
 
    //Specific errors in API request processing
    private List<String> details;
 
    //Getter and setters
}
````
> ## Custom ExceptionHandler

> Now add one class extending **ResponseEntityExceptionHandler** and annotate it with **@ControllerAdvice** annotation.
> `ResponseEntityExceptionHandler` is a convenient base class for to provide centralized exception handling across all `@RequestMapping` methods through `@ExceptionHandler` methods. `@ControllerAdvice` is more for enabling auto-scanning and configuration at application startup.

````java
CustomExceptionHandler.java
package com.howtodoinjava.demo.exception;
 
import java.util.ArrayList;
import java.util.List;
 
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.ObjectError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;
 
@SuppressWarnings({"unchecked","rawtypes"})
@ControllerAdvice
public class CustomExceptionHandler extends ResponseEntityExceptionHandler 
{
    @ExceptionHandler(Exception.class)
    public final ResponseEntity<Object> handleAllExceptions(Exception ex, WebRequest request) {
        List<String> details = new ArrayList<>();
        details.add(ex.getLocalizedMessage());
        ErrorResponse error = new ErrorResponse("Server Error", details);
        return new ResponseEntity(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
 
    @ExceptionHandler(RecordNotFoundException.class)
    public final ResponseEntity<Object> handleUserNotFoundException(RecordNotFoundException ex, WebRequest request) {
        List<String> details = new ArrayList<>();
        details.add(ex.getLocalizedMessage());
        ErrorResponse error = new ErrorResponse("Record Not Found", details);
        return new ResponseEntity(error, HttpStatus.NOT_FOUND);
    }
 
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex, HttpHeaders headers, HttpStatus status, WebRequest request) {
        List<String> details = new ArrayList<>();
        for(ObjectError error : ex.getBindingResult().getAllErrors()) {
            details.add(error.getDefaultMessage());
        }
        ErrorResponse error = new ErrorResponse("Validation Failed", details);
        return new ResponseEntity(error, HttpStatus.BAD_REQUEST);
    }
}
````

### HTTP POST/employees/invalid

```JSON
{
    "email": "ibill@gmail.com"
}
```

````JSON
HTTP Status : 400 
{
    "message": "Validation Failed",
    "details": [
        "last name must not be empty",
        "first name must not be empty"
    ]
}
````

### HTTP POST/employees/23

````json
HTTP Status : 404 
{
    "message": "Record Not Found",
    "details": [
        "Invalid employee id : 23"
    ]
}
````

