# project-introduction-springmvc-rest-boot-soap
* **introduction about springMVC, SpringBoot, SpringRestful,SpringSoap , postman and SoapUi tools.** 

#### Spring MVC Flow

* In Spring Web MVC, DispatcherServlet class works as the front controller. It is responsible to manage the flow of the spring mvc application.
* The @Controller annotation is used to mark the class as the controller in Spring 3.
* The @RequestMapping annotation is used to map the request url. It is applied on the method.

#### Diagram-1

![image](https://user-images.githubusercontent.com/40323661/152082609-c7536444-8332-4aee-bbe2-dcfaa84cf06c.png)

#### Diagram-2 

![SpringMVCTuto-1](https://java2blog.com/wp-content/uploads/2017/07/SpringMVCTuto-1.png "SpringMVCTuto-1")


#### Spring MVC Execution Flow

* **Step 1:** First request will be received by DispatcherServlet.

* **Step 2:**  DispatcherServlet will take the help of HandlerMapping and get to know the Controller class name associated with the given request.

* **Step 3:** So request transfer to the Controller, and then controller will process the request by executing appropriate methods and returns ModelAndView object (contains Model data and View name) back to the DispatcherServlet.

* **Step 4:** Now DispatcherServlet send the model object to the ViewResolver to get the actual view page.

* **Step 5:** Finally DispatcherServlet will pass the Model object to the View page to display the result.


#### Spring MVC Framework and REST

* Spring’s annotation-based MVC framework simplifies the process of creating RESTful web services. 
* The key difference between a traditional Spring MVC controller and the RESTful web service controller is the way the HTTP response body is created. While the traditional MVC controller relies on the View technology, the RESTful web service controller simply returns the object and the object data is written directly to the HTTP response as JSON/XML. 


#### Diagram-3

![PPnvoNS](http://i.imgur.com/PPnvoNS.png "PPnvoNS")


### Spring MVC REST Workflow

The following steps describe a typical Spring MVC REST workflow:

* The client sends a request to a web service in URI form.
* The request is intercepted by the DispatcherServlet which looks for Handler Mappings and its type.
  * The Handler Mappings section defined in the application context file tells DispatcherServlet which strategy to use to find controllers based on the incoming request.
  * Spring MVC supports three different types of mapping request URIs to controllers: annotation, name conventions, and explicit mappings.
* Requests are processed by the Controller and the response is returned to the DispatcherServlet which then dispatches to the view. 

* In Diagram-3, notice that in the traditional workflow the ModelAndView object is forwarded from the controller to the client. Spring lets you return data directly from the controller, without looking for a view, using the @ResponseBody annotation on a method. Beginning with Version 4.0, this process is simplified even further with the introduction of the @RestController annotation. Each approach is explained below.

#### Using the @ResponseBody Annotation

* When you use the @ResponseBody annotation on a method, Spring converts the return value and writes it to the HTTP response automatically. Each method in the Controller class must be annotated with @ResponseBody.

#### Diagram-4

![3.x-diagram](https://resources.cloud.genuitec.com/wp-content/uploads/2015/09/3.x-diagram.png "3.x-diagram")

* Behind the Scenes

  * Spring has a list of HttpMessageConverters registered in the background. 
  * The responsibility of the HTTPMessageConverter is to convert the request body to a specific class and back to the response body again, depending on a predefined mime type. 
  * Every time an issued request hits @ResponseBody, Spring loops through all registered HTTPMessageConverters seeking the first that fits the given mime type and class, and then uses it for the actual           conversion.
    
#### Code Examples

Notice-1 

```Java

@Controller
@RequestMapping("/employee-module")
public class EmployeeController 
{
    
    EmployeeManager manager = new EmployeeManager();
 
    @RequestMapping(value = "/getAllEmployees", method = RequestMethod.GET)
    public String getAllEmployees(Model model)
    {
        model.addAttribute("employees", manager.getAllEmployees());
        return "employeesListDisplay";
    }
}

```
 
Notice-2

```Java
@Controller

@RequestMapping("employees")

public class EmployeeController {

    Employee employee = new Employee();

    @RequestMapping(value = "/{name}", method = RequestMethod.GET, produces = "application/json")

    public @ResponseBody Employee getEmployeeInJSON(@PathVariable String name) {

       employee.setName(name);

       employee.setEmail("employee1@genuitec.com");

    return employee; 

    }
```
    
#### Using the @RestController Annotation

* Spring 4.0 introduced @RestController, a specialized version of the controller which is a convenience annotation that does nothing more than add the @Controller and @ResponseBody annotations. By annotating the controller class with @RestController annotation, you no longer need to add @ResponseBody to all the request mapping methods.  
 
Diagram-5

![4.x-diagram](https://resources.cloud.genuitec.com/wp-content/uploads/2015/09/4.x-diagram.png  "4.x-diagram")


* To use @RestController in our example, all we need to do is modify the @Controller to @RestController and remove the @ResponseBody from each method.

Notice-3

```Java
@RestController

@RequestMapping("employees")

public class EmployeeController {

    Employee employee = new Employee();

    @RequestMapping(value = "/{name}", method = RequestMethod.GET, produces = "application/json")

    public Employee getEmployeeInJSON(@PathVariable String name) {

       employee.setName(name);

       employee.setEmail("employee1@genuitec.com");

       return employee;

    }
```
* Note that we no longer need to add the @ResponseBody to the request mapping methods. After making the changes, running the application on the server again results in the same output as before.

#### Conclusion

* As you can see, using @RestController is quite simple and is the preferred method for creating MVC RESTful web services starting from Spring v4.0. 


 #### Differences between @RequestParam and @PathVariable in Spring MVC

```Console
Both annotations @RequestParam and @PathVariable in Spring MVC are used for fetching the values of request parameters.
These annotations have similar purpose but some differences in use. 
The key difference between @RequestParam and @PathVariable is that @RequestParam used for accessing
the values of the query parameters where as @PathVariable used for accessing the values from the URI template.
```

## This is the different annotations from Spring and JAX-RS

**SPRING ANNOTATION**|**JAX-RS ANNOTATION**
--------------------|------------------
@RequestMapping(path = “/troopers” | @Path(“/troopers”)
@PostMapping	| @POST
@PutMapping	| @PUT
@GetMapping	| @GET
@DeleteMapping	| @DELETE
@ResponseBody	| N/A
@RequestBody	| N/A
@PathVariable(“id”)	| @PathParam(“id”)
@RequestParam(“xyz”)	| @QueryParam(‘xyz”)
@RequestParam(value=”xyz”)	| @FormParam(“xyz”)
@RequestMapping(produces = {“application/json”}) | 	@Produces(“application/json”)
@RequestMapping(consumes = {“application/json”}) |	@Consumes(“application/json”)

#### Diff between PathParam and QueryParam ?

* Path params are part of the url where as query parameters are added after the ? mark symbol and separated from other query parameters by & symbol.
 
```Java
PathParam example
GET http://base-url/students/{roll-number}  OR

@RequestMapping(path = “/base-url/students/{roll-number}”  == @Path(“/base-url/students/{roll-number}”)

QueryParam example
GET http://base-url/students?grade=10    OR

@RequestMapping(path = “/base-url/students/{roll-number}”  == @Path(“/base-url/students/{roll-number}”)
```

* **When to use @PathParam vs @QueryParam**
* This is not a standard, you can use anyone for designing restful api.

* However, the commonly used convention is :

  * Any required or mandatory attributes should be added as path param
  * Any optional attributes should be added as query param
  * params used for filtering data are usually used as query param
 
 
 
## Spring Data – One API To Rule Them All ?

#### Spring Data is a high level SpringSource project whose purpose is to unify and ease the access to different kinds of persistence stores, both relational database systems and NoSQL data stores.

![spring_data_overview_small](https://res.infoq.com/articles/spring-data-intro/en/resources/spring_data_overview_small.jpg "spring_data_overview_small")

* With every kind of persistence store, your repositories (a.k.a. DAOs, or Data Access Objects) typically offer CRUD (Create-Read-Update-Delete ) operations on single domain objects, finder methods, sorting and pagination. Spring Data provides generic interfaces for these aspects (CrudRepository, PagingAndSortingRepository) as well as persistence store specific implementations.


#### Object/Datastore Mapping

**JPA**

```java

@Entity 
@Table(name="TUSR")
public class User {

  @Id
  private String id;

  @Column(name="fn")
  private String name;

  private Date lastLogin;

...
} 

```


**MongoDB**

```Java
@Document(collection="usr")
public class User {

  @Id
  private String id;

  @Field("fn")
  private String name;

  private Date lastLogin;

 ...
} 
```


## Difference Between Hibernate vs JPA

* Hibernate is a framework that is known as the Hibernate ORM framework. Hibernate which is known as Hibernate ORM is a framework that was designed by Red Hat and its initial release happened on 23 May 2007 is an object-relational mapping tool for the Java language. It is written in Java and it supports a cross-platform JVM. Its licensing is done under GNU Lesser General Public. JPA is known as Java persistence API. JPA which is actually known as Java Persistence Application Programming Interface OR Java application programming interface is used to manage the relational data. JPA is basically is a specification. It deals with the object or relational metadata. The language of the JPA is JPQL (Java Persistence Query Language).

#### Hibernate

* Hibernate’s primary features are to map the Java classes to database tables. Some key feature of Hibernate is given below:

  * It’s an implementation of JPA guidelines.
  * It helps to map Java classes to database tables and Java data types to SQL data types.
  * Hibernate is the provider of JPA.
  
#### JPA

* The initial release of JPA happened on 11 May 2006. Some Key features of JPA are given below:

  * JPA is not an implementation it is only a specification.
  * It is a set of rules and guidelines for setting interfaces for the implementation of object-relational mapping.
  * It requires a small number of classes and interfaces.
  * It supports easier cleaner and standardized object-relational mapping.
  * It supports polymorphism and inheritance.
  * In this dynamic and named queries can be added.
  * In one line if we want to define the Hibernate and JPA then we can say that Hibernate is the implementation of all the JPA guidelines.  

#### MongoDB installation and UserCreation:


* Download MongoDB using below Link : https://www.mongodb.com/download-center/community
* Download robo3t for MongoDB user interface : https://robomongo.org/download

* **Steps:**  
  * once installed above below location and run below two .exe files
  * C:\Program Files\MongoDB\Server\4.2\bin   and run
    mongod.exe then mongo.exe
  * Using mongo.exe window you should create user
  
```Java

use MongoDBPOC123

db.createUser(
  {
    user: "veer",
    pwd: "veer123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)  
```

**OR you can you another type of user:**

```Java
use MongoDBPOC

db.createUser(
  {
    user: "veer",
    pwd: "veer123",
    roles: [ { role: "userAdmin", db: "MongoDBPOC" } ]
  }
)

```

* Once you created user then open robo3t and open manage connection
  
#### Please refer below images :

![mongo_user_creation](https://github.com/privatevkakarla/project-introduction-springmvc-rest-boot-soap/blob/master/images/mongo_user_creation.JPG "mongo_user_creation")




#### Open robo3t and Manage User and create User:

![robo3t](https://github.com/privatevkakarla/project-introduction-springmvc-rest-boot-soap/blob/master/images/robo3t.JPG "robo3t")

![user](https://github.com/privatevkakarla/project-introduction-springmvc-rest-boot-soap/blob/master/images/user.JPG "user")

#### Add Authentication User

![Authentication](https://github.com/privatevkakarla/project-introduction-springmvc-rest-boot-soap/blob/master/images/Authentication.JPG "Authentication")

#### test created user with db connection

![checkdb_test](https://github.com/privatevkakarla/project-introduction-springmvc-rest-boot-soap/blob/master/images/checkdb_test.JPG "checkdb_test")





