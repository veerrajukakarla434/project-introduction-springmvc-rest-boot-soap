# project-introduction-springmvc-rest-boot-soap
* **introduction about springMVC, SpringBoot, SpringRestful,SpringSoap , postman and SoapUi tools.** 

#### Spring MVC Flow

* In Spring Web MVC, DispatcherServlet class works as the front controller. It is responsible to manage the flow of the spring mvc application.
* The @Controller annotation is used to mark the class as the controller in Spring 3.
* The @RequestMapping annotation is used to map the request url. It is applied on the method.

#### Diagram-1

![Spring_Flow](https://codenuclear.com/wp-content/uploads/2017/08/Spring_Flow.jpg "Spring_Flow")

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
 





