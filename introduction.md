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
