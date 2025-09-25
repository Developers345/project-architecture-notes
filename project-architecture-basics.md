# Project Architecture

Before going to the Spring project architecture, first we need to know about **JavaEE Project Architecture** and **Struts Project Architecture**.

# Which are called as presentation tier component?

The component which is the part of the application that deals with client responsibility is managing and handling the request, these classes and the components are called **presentation tier component**.

# Why Servlet is called as Controller?

A **Servlet** acts as a controller in web applications because it manages and coordinates the flow between the client requests and the server-side components. Below are the reasons:

1. **Request Handling**  
   - When an end user sends a request, the **Servlet Container** receives it.  
   - However, the container itself does not act as a controller because it does not manage or send a response back to the client directly.  

2. **Dispatching Requests**  
   - The Servlet Container dispatches the request to the **Servlet component**.  
   - The Servlet then performs the required operation based on the request.  

3. **Dynamic Operations**  
   - A Servlet does not always perform the same task.  
   - Depending on the **request URL**, the Servlet decides which operation to execute.  

4. **Decision Maker**  
   - The Servlet examines the request, determines what needs to be done, and forwards or responds accordingly.  
   - Because of this **decision-making capability**, the Servlet is referred to as the **Controller** in the MVC (Model–View–Controller) architecture.  

---

 # Why JSP is called as View Component?

In the **MVC (Model–View–Controller) architecture**, **JSP (JavaServer Pages)** is referred to as the **View Component**. This is because its primary responsibility is to present data to the client in a user-friendly format, without handling business logic or request processing.  

---

## Reasons JSP is a View Component

1. **Presentation Layer**  
   - JSP focuses on displaying information to the end user (HTML, CSS, JavaScript, or data).  
   - It does not handle business rules or database interactions.  

2. **Separation of Concerns**  
   - Business logic is handled by the Model.  
   - Decision-making and flow control are managed by the Controller (Servlet).  
   - JSP is only responsible for **rendering the response** (output).  

3. **Dynamic Content Rendering**  
   - JSP allows embedding of Java code (via tags, expressions, JSTL, EL) to display dynamic data.  
   - Example: Displaying user details, product lists, or reports.  

4. **No Action / Logic Handling**  
   - JSP does not manage request routing, validation, or decision-making.  
   - Its sole responsibility is to **present the result to the client**.  


# Why We Should Not Write Business Logic in Servlet?

When developing Java-based web applications, it is considered a **bad practice** to write business logic directly inside a Servlet. The main purpose of a Servlet is to act as a **controller** that receives client requests, processes them with the help of other components (like business and data layers), and then forwards the response to the appropriate view (JSP, HTML, etc.).

---

## Key Reasons

### 1. Tight Coupling with Presentation Tier
- If we write business logic directly inside the Servlet, the logic becomes **tightly coupled with the presentation tier**.
- This makes the business logic **non-reusable**, since it cannot be separated and reused by other components outside the Servlet.

---

### 2. Lack of Reusability
- Business logic is supposed to be **reusable** across different parts of the application (for example, in desktop apps, web apps, or APIs).
- By embedding it in a Servlet, the logic is locked into the Servlet lifecycle and cannot be reused elsewhere.

---

### 3. Servlet Object Creation Constraints
- We do not create Servlet objects manually because Servlet objects are managed by the **Servlet Container** (like Tomcat, Jetty, etc.).
- If we try to create a Servlet object ourselves, we would need to pass **`HttpServletRequest`** and **`HttpServletResponse`** objects, which are **technology-specific components**.
- Since these objects are created and managed only by the container, the logic inside cannot be accessed independently.

---

### 4. Violates Separation of Concerns
- According to **MVC architecture**, Servlets should act as **Controllers**, JSPs as **Views**, and business logic should reside in **Service/Model classes**.
- Mixing responsibilities makes the code hard to maintain and test.

---

## Best Practice
- Keep the Servlet limited to **request handling and response redirection**.
- Write business logic inside **separate service classes (POJOs)** or use frameworks like **Spring** or **EJB** for better separation of concerns and reusability.


# What is the Problem We Face in Spring and Struts Frameworks if We Write Business Logic in Controller Class?

When building enterprise web applications using frameworks like **Spring MVC** or **Struts**, it is strongly discouraged to write **business logic** inside the controller classes. Controllers should only manage the **flow of requests and responses**, while business logic should be handled by **service or model classes**.

---

## Key Points

### 1. Presentation Tier Responsibility
- In any web application, the **presentation tier** is built using **Servlets** and **JSPs** because they are responsible for handling **end-user interactions**.
- Even in frameworks like **Spring MVC** or **Struts**, a presentation tier is still required to process client requests.

---

### 2. Division of Responsibilities
- In an application, generally:
  - One component manages the **view** (JSPs).
  - Another component manages the **decision-making process** (Controller/Action class).

---

### 3. Framework-Specific Roles
- **Spring MVC**:
  - **JSP Page** → Acts as the **View Component**.
  - **Controller Class** → Acts as the **Controller** (decision maker).
- **Struts**:
  - **JSP Page** → Acts as the **View Component**.
  - **Action Class** → Acts as the **Controller** (decision maker).

---

### 4. Technology Dependency
- The **presentation tier components** change depending on the framework/technology:
  - In Spring → `@Controller` classes + JSPs.
  - In Struts → Action classes + JSPs.
- If you place business logic in controllers (Spring `@Controller` or Struts `Action` class), it becomes **tied to that specific framework**.

---

### 5. Migration Issues
- When migrating from **one framework to another** (e.g., Struts → Spring MVC, or Spring MVC → JSF), the **presentation layer changes**.
- Since business logic is written inside controllers, that logic will be **lost during migration**.
- This makes the application difficult to **maintain, extend, or port** to newer technologies.

---

## Best Practice
- Keep controllers **lightweight**—only handle request mapping, decision making, and forwarding to views.
- Write business logic in **Service/Model/DAO classes** (independent of framework).
- This ensures:
  - **Loose coupling**
  - **Reusability**
  - **Maintainability**
  - **Easy migration across frameworks**

---

✅ **Conclusion**:  
Never write business logic inside the **Controller (Spring)** or **Action Class (Struts)**. Instead, keep it in separate **service/model layers** to ensure that your application remains **flexible and portable** across different frameworks and technologies.


# What is POJO Class?

A **POJO (Plain Old Java Object)** class is a simple Java object that does not follow any special Java object model, conventions, or frameworks. It is used to encapsulate data and business logic in an object-oriented way.

## Key Points:
- A POJO class contains **attributes (fields/properties)** and **business methods**.  
- The methods most of the time will not have parameters.  
- It does not extend pre-specified classes like `javax.ejb.EntityBean`, nor implement pre-specified interfaces.  
- POJO classes are free from special restrictions and can be easily used across different Java frameworks.

## Example:

```java
public class Employee {
    private int id;
    private String name;

    // Constructor
    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // Business method
    public void displayEmployeeDetails() {
        System.out.println("Employee ID: " + id);
        System.out.println("Employee Name: " + name);
    }
}
````

## Advantages of POJO:

* Improves readability and reusability.
* Framework independent.
* Easy to test and maintain.


# Why We Should Not Use Delegate and DAO in a Typical J2EE Application

## Key Points

- In a **typical J2EE web application**, **Delegate** and **DAO** will not be there.  
- Most of the time, we write **business logic** and **persistency logic** as part of **POJO classes** only.  
- Our application does not contain any **complex business logic**; these are **less complicated solutions**.  
- Therefore, we should use **Servlet, JSP, and POJO classes** — not **Delegate** and **DAO classes**.  

---

## Limitations of Servlet and JSP

- **Complex solutions cannot be built** using only Servlet and JSP.  
- Servlets and JSPs are **not designed** to build **large, complex systems**.  
- They are meant for **simple application requirements**:
  - Receive the request  
  - Read the data  
  - Delegate the request to the POJO class  

---

## Issues with Complex Web Tier Requirements

- If there are **hundreds of pages** in the application, do not go for plain **Servlet and JSP**.  
- Navigation complexity increases:  
  - Pages and servlets become **tightly coupled**.  
  - It is **not easy to identify** which servlet forwards the request to which JSP.  
  - Example:  
    - If one servlet forwards to `x.jsp`, but later we want `y.jsp`, we must **change the servlet**.  
    - However, finding the exact servlet is difficult since we may need to check **every servlet**.  

---

## Navigability and Maintainability Issues

- Managing **navigability** is not easy with Servlet and JSP.  
- Frameworks solve this problem because:  
  - **Configuration** is handled in **properties files**.  
  - This makes navigation and maintenance **much easier** compared to raw Servlets and JSP.  
```
