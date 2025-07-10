# Lesson 1: Introduction to Minimal API

## Lesson Information

### Resource(s)

- [Microsoft Learn - .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new)
- [Microsoft Learn - Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview?view=aspnetcore-8.0)
- [Microsoft Learn - OpenAPI support in minimal API apps](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/overview?view=aspnetcore-8.0)


### Description
This lesson introduces ASP.NET Core Minimal APIs, a simplified approach for building fast HTTP APIs with .NET 8. Learners will learn the fundamentals of creating lightweight, efficient APIs without the overhead of traditional controllers. Through hands-on exercises, students will master the basics of minimal API development, from simple endpoints to structured applications with proper organization, validation, and documentation.

### Learning Objective
Create production-ready minimal APIs using .NET 8 with proper project organization, data validation, error handling, and documentation.

### Learning Standards

1. In ASP.NET Core, Minimal APIs are a simplified approach for building fast HTTP APIs with minimal code and configuration, ideal for microservices and apps that require minimal dependencies. 
2. In .NET, the `dotnet new web` command creates a new minimal API project with the basic structure and dependencies. 
3. In ASP.NET Core Minimal APIs, the `MapGet()` method is used to define `GET` endpoints that retrieve data from the API. 
4. In ASP.NET Core Minimal APIs, route parameters can be captured from the URL using parameter binding and constraints to validate input. 
5. In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API.
6. In ASP.NET Core Minimal APIs, the `MapPost()` method can be used with model binding to accept and process JSON data as C# objects.
7. In ASP.NET Core Minimal APIs, model binding automatically maps HTTP request data to method parameters using various binding sources like route values, query strings, and request bodies. 
8. In ASP.NET Core Minimal APIs, the `MapPut()` method is used to define `PUT` endpoints that update existing resources in the API.
9. In ASP.NET Core Minimal APIs, the `MapDelete()` method is used to define `DELETE` endpoints that remove resources from the API. 
10. In ASP.NET Core Minimal APIs, validation can be implemented using Data Annotations and the built-in validation framework. 
11. In ASP.NET Core Minimal APIs, error handling can be implemented using exception handling middleware and standardized problem details responses. 
12. In ASP.NET Core Minimal APIs, OpenAPI documentation can be automatically generated using built-in support for Swagger/OpenAPI specifications.

## Exercise # 1: Getting Started with Minimal API

**Learning Standards**: 
* In ASP.NET Core, Minimal APIs are a simplified approach for building fast HTTP APIs with minimal code and configuration, ideal for microservices and apps that require minimal dependencies.
* In .NET, the `dotnet new web` command creates a new minimal API project with the basic structure and dependencies.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:
* Understand what Minimal APIs are and how they differ from traditional controller-based APIs.
* Create a new Minimal API project using the .NET CLI.

* Build and run the app from the terminal.
* Explore the generated `Program.cs` file and understand the default route.
* Access the default endpoint in a browser.

### Narrative Summary

Imagine a scenario where a small application is needed to track expenses, log workouts, or collect feedback — but without the overhead of creating multiple files just to handle one request. This is exactly where Minimal APIs are most effective.

Minimal APIs offer a streamlined approach in ASP.NET Core for building web APIs with minimal code. Unlike traditional ASP.NET Core applications that rely on controllers, routing attributes, and multiple files, Minimal APIs allow developers to define routes and logic in a single place — typically in `Program.cs`. This reduces boilerplate and accelerates startup time, making it ideal for microservices or lightweight APIs.

A Minimal API project can be created using the .NET CLI:

```bash
dotnet new web -o Name
```

This command generates a project folder named `"Name"` with essential setup. Inside, the `Program.cs` file contains the code to define the application and map a simple route:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

In this example, `MapGet()` defines an HTTP GET endpoint at `/` that returns `"Hello World!"` as a response. Running the application locally and visiting the appropriate URL will display this message in the browser.

By the end of the exercise, learners will have a working Minimal API and a foundational understanding of how it operates and why it's referred to as “minimal.”


### Instructions

**Checkpoint 1:** Use `dotnet new web` command to create a new minimal API project in a new folder.

**Checkpoint 2:** Open the `Program.cs` file and locate the `MapGet()` method. Modify the response to return a custom message such as your name or a greeting.

**Checkpoint 3:** Build the project and confirm it compiles successfully.

**Checkpoint 4:** Run the application and note the URLs where it is listening.

**Checkpoint 5:** Access the root endpoint (`/`) in the web browser and verify it returns the updated custom response.

#### What is the purpose of these checkpoints?

To ensure that learners can independently set up and run a minimal API project, which is foundational before they can begin customizing behavior or structure.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise # 2: Basic `GET` Endpoints 

**Learning Standards**: 
* In ASP.NET Core Minimal APIs, the `MapGet()` method is used to define `GET` endpoints that retrieve data from the API.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

* Use `MapGet()` to define static `GET` endpoints.
* Understand how route paths map to endpoints.
* Return various response types including strings, numbers, and objects.


### Narrative Summary

This exercise begins by analyzing the default `Program.cs` file generated by the Minimal API template. The file contains a single configured endpoint:

```cs
app.MapGet("/", () => "Hello World!");
```
This line sets up a basic route that responds to HTTP GET requests at the root path `/`. The lambda function `() => "Hello World!"` returns a simple string as a response. This gives us an immediate working API endpoint with minimal setup.

Learners will explore how `MapGet()` works, and how easy it is to define more routes. They will extend the default API by adding new endpoints that return strings, numbers, and objects. These examples help learners understand how URL paths map to specific logic in code.


### Instructions

**Checkpoint 1:** Create a GET endpoint at `/hello` that returns a custom greeting string. Test it in the browser.

**Checkpoint 2:** Add a GET endpoint at `/time` that returns the current UTC time. Confirm the output updates on each refresh.

**Checkpoint 3:** Define a GET endpoint at `/info` that returns a JSON object with two properties — for example, `"name"` and `"version"`. Test the endpoint and observe the JSON structure.

#### What is the purpose of these checkpoints?

To help learners build confidence by writing simple but complete GET routes using Minimal API syntax. This reinforces the concept that Minimal APIs are designed for rapid setup and direct logic expression using methods like `MapGet()`.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise # 3: Working with Route Parameters

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, route parameters can be captured from the URL using parameter binding and constraints to validate input.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:
* Use route parameters to capture values from URLs.
* Apply route constraints to validate input.
* Bind parameters directly to lambda inputs.
  
### Narrative Summary

This exercise shows how Minimal APIs allows to capture values like IDs directly from the URL. Unlike frameworks that require route decorators, Minimal APIs bind parameters directly in the method signature.
* Create a route like `/items/{id}` using `MapGet()`.
* Explain how `{id}` binds to a parameter.
* Add route constraint `/items/{id:int}`.
* Show return based on ID
  
### Instructions

**Checkpoint 1:** Create a GET endpoint with a route parameter named `{id}` and test it using values like `/api/items/123` or `/api/items/abc123`

**Checkpoint 2:** Add another endpoint that accepts only integer values for `{id}` using a `:int` route constraint. Test it with `/api/items/int/42` and confirm invalid inputs like `/api/items/int/hello` return a `404` error.

**Checkpoint 3:** Add a third endpoint using a `{username}` parameter that returns a message in JSON format. Test it using names like `/api/users/alex` or `/api/users/guest42`.

#### What is the purpose of these checkpoints?

To give learners hands-on experience with parameter binding and validation using route constraints. This is key to building dynamic APIs.

#### What would you like to have in the workspace for this exercise? Share your plan below.
Code Editor | Terminal | Web Browser


## Exercise #4: Creating a Simple POST Endpoint

**Learning Standards**:

In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API.

### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

* Use `MapPost()` to create a POST endpoint.
* Understand how POST is used to send data to the server.
* Return a success message using `Results.Created()`.
* Test POST using the built-in Swagger UI.

### Narrative Summary

In this exercise, learners will learn how to create a POST endpoint using `MapPost()` in a Minimal API. The POST method is commonly used when clients need to send data to the server — such as submitting a message or creating a new record.

An endpoint will be defined at `/api/message` that accepts plain text from the request body and responds using `Results.Created()`, which returns an `HTTP 201` status code along with a confirmation message.

To make testing easier, Swagger UI will be enabled — a built-in interactive interface for trying out API endpoints in the browser. Since Swagger is not enabled by default in minimal API projects created with `dotnet new web`, it must first be configured by adding the required services and middleware in `Program.cs`.

By the end of this exercise, a working POST endpoint will be available that accepts input and returns confirmation — all tested through Swagger without needing external tools.


### Instructions:

**Checkpoint 1:** Enable Swagger UI in the Minimal API project and confirm that `/swagger` shows the existing `GET` endpoints.

**Checkpoint 2:** Add a `POST` endpoint at `/api/message` that accepts plain text from the request body and responds using `Results.Created()`.

**Checkpoint 3:** Open Swagger UI, test the new `POST` endpoint by sending a custom message, and verify that the response includes the submitted input and a `201 Created` status.

#### What is the purpose of these checkpoints?

These checkpoints are designed to give hands-on practice with concepts introduced in the summary. The learners will test their ability to configure API documentation, define a working POST endpoint, and verify its behavior using Swagger. Completing them reinforces understanding of how data flows into a Minimal API and how to provide proper HTTP responses for creation actions.

### What would you like to have in the workspace for this exercise? Share your plan below.

- Code Editor | Terminal | Web Browser
- Also need to check Swagger installation.

## Exercise #5: Accepting JSON Using Model Binding

**Learning Standards**:

* In ASP.NET Core Minimal APIs, use `MapPost()` method with model binding to accept and process JSON data as C# objects.


### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

* Accept JSON data in a POST request.
* Create a matching C# class for the expected data.
* Use model binding to convert JSON into a C# object.
* Test the endpoint using Swagger UI.

### Narrative Summary

In the previous exercise, a `POST` endpoint was created to accept plain text. However, most modern APIs exchange structured data using **JSON** — a lightweight format used to send multiple fields in one request.

Instead of manually reading and parsing JSON, ASP.NET Core uses **model binding**, which automatically maps JSON input to a C# class. A model can be defined with properties like `Id` and `Name`, and when a JSON object is sent, ASP.NET fills in that object during the request handling.

For example, sending:

```json
{
  "id": 101,
  "name": "Notebook"
}
```

…will automatically be deserialized into an `Item` object when the following model is defined:

```cs
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

This allows structured input to be handled easily without custom parsing logic. Testing will be done using Swagger, where JSON can be submitted through the browser and the structured response is returned with a `201 Created` status.


### Instructions

**Checkpoint 1:** Create a class called `Book` with two properties: `Title` (string) and `Author` (string). Place the class below `app.Run()` in `Program.cs`.

**Checkpoint 2:** Add a `POST` endpoint at `/api/books` that accepts a `Book` object and returns it using `Results.Created()`.

**Checkpoint 3:** In Swagger UI, test the new `POST /api/books` endpoint with this JSON input:

   ```json
   {
     "title": "Clean Code",
     "author": "Robert C. Martin"
   }
   ```
  Confirm that the API returns the same values along with status code `201 Created`.


#### What is the purpose of these checkpoints?

These checkpoints allows to apply model binding with a new data model of your own. The learners can practice defining a class, accepting structured input from JSON, and verifying that the binding and response work correctly. This ensures that the learners can handle a variety of input types and design meaningful resource models.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser


## Exercise #6: Combining GET and POST — Resource Creation and Retrieval

**Learning Standards:**

* In ASP.NET Core Minimal APIs, model binding automatically maps HTTP request data to method parameters using various binding sources like route values, query strings, and request bodies.

### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To:**

* Use POST to add a product with JSON data.
* Use GET to retrieve a product by ID (route binding) and search products by name (query string binding).
* Understand how model binding works across different HTTP request sources.


### Narrative Summary

In this exercise, the POST and GET methods are combined to build a simple product API using an in-memory list to store products during the app runtime.

Learners will create:

* A `POST` endpoint to add products using JSON data.
* A `GET` endpoint to retrieve a product by its ID using a route parameter.
* A `GET` endpoint to search for products by name using a query string parameter.

This practical example demonstrates how various types of model binding—route, query string, and body—work together to support basic API operations for creating and retrieving resources.


### Instructions

**Checkpoint 1:**

* Define a `Product` class with `Id`, `Name`, and `Price`.
* Create an in-memory list to hold products.
* Add POST endpoint `/api/products` to add products to the list.
* Add GET endpoint `/api/products/{id}` to retrieve product by ID.
* Add GET endpoint `/api/products/search` to search products by name.

**Checkpoint 2:**

* Test POST `/api/products` using Swagger with a JSON product.
* Test GET `/api/products/{id}` to retrieve a product by its ID.
* Test GET `/api/products/search` with a query string to find products by name.


#### What is the purpose of these checkpoints?

These checkpoints helps to practice creating and retrieving resources through HTTP POST and GET methods. The learners will see how ASP.NET Core Minimal APIs handle data binding from JSON request bodies, URL route parameters, and query strings, enabling to build basic but functional APIs.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise #7: Updating and Deleting Resources

**Learning Standards**: 

* In ASP.NET Core Minimal APIs, the `MapPut()` method is used to define **PUT** endpoints that update existing resources in the API.
* In ASP.NET Core Minimal APIs, the `MapDelete()` method is used to define **DELETE** endpoints that remove resources from the API.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

* Use `MapPut()` to define a PUT endpoint that updates an existing resource using route parameters and body data
* Use `MapDelete()` to define a DELETE endpoint that removes a resource using route parameters
* Return appropriate HTTP status codes such as `200 OK`, `204 No Content`, and `404 Not Found`
* Demonstrate full CRUD capability by integrating POST, GET, PUT, and DELETE in one app

### Narrative Summary

Previous exercises covered creating and retrieving products with POST and GET endpoints. This exercise completes the full CRUD cycle by introducing updating and deleting resources.

To update a resource, the **PUT** HTTP method is used, defined in Minimal APIs via `MapPut()`. This requires a route parameter to identify the resource (such as an ID) and a JSON body with the updated data. When a client sends a PUT request with a matching ID, the API updates the existing product and returns the updated resource or a 404 if not found.

To delete a resource, the **DELETE** HTTP method is used via `MapDelete()`. It also uses a route parameter for the resource ID. If the item is found, it is removed from the list and a `204 No Content` response is sent; otherwise, a `404` is returned.

This exercise demonstrates the full CRUD workflow — Create (`POST`), Read (`GET`), Update (`PUT`), and Delete (`DELETE`) — showing how Minimal APIs handle route parameters, JSON body binding, and status codes.


### Instructions

**Checkpoint 1:**
Add a PUT endpoint for updating products by ID with JSON data. Insert the provided `MapPut` code below existing endpoints.

**Checkpoint 2:**
Use Swagger to test the PUT endpoint:

* Add a product with POST.
* Update the product with PUT using its ID.
* Attempt to update a non-existent product ID.

**Checkpoint 3:**
Add a DELETE endpoint for removing products by ID. Insert the provided `MapDelete` code below existing endpoints.

**Checkpoint 4:**
Use Swagger to test the DELETE endpoint:

* Add a product with POST.
* Delete the product with DELETE using its ID.
* Attempt to delete the same product again.


### What is the purpose of these checkpoints?

These checkpoints guide learners to implement and test updating and deleting API resources, reinforcing full CRUD operations with Minimal APIs while using route parameters, JSON body binding, and appropriate HTTP status codes.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise #8: Basic Data Validation

**Learning Standards**: 

* In ASP.NET Core Minimal APIs, validation can be implemented using Data Annotations and the built-in validation framework.
* In ASP.NET Core Minimal APIs, error handling can be implemented using exception handling middleware and standardized problem details responses.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

* Add validation rules using Data Annotations like `[Required]`, `[Range]`, `[StringLength]`, etc.
* Automatically trigger validation during model binding
* Return proper HTTP status codes (`400 Bad Request`) when data is invalid
* Understand how to catch and respond to validation errors in a user-friendly way
  
### Narrative Summary

Previous exercises demonstrated creating and managing products via POST, GET, PUT, and DELETE endpoints. However, the API currently accepts any data, including invalid or incomplete values.

This exercise introduces data validation using Data Annotations such as `[Required]`, `[StringLength]`, and `[Range]` applied to the model. These attributes automatically enforce rules during model binding, ensuring input data like product names and prices meet defined constraints.

Since Minimal APIs do not validate models automatically like MVC, manual validation checks must be performed inside endpoint handlers. Invalid input results in a `400 Bad Request` response containing detailed error information, while valid input proceeds to process normally.

Implementing validation helps protect the API from bad data and improves client feedback with clear error messages. This is critical for real-world APIs, where data integrity and user guidance matter.


### Instructions

**Checkpoint 1:**

Add validation attributes to the existing `Product` class by updating it with `[Required]`, `[StringLength]`, and `[Range]` annotations as shown.

**Checkpoint 2:**

Modify the `POST /api/products` endpoint to validate incoming products manually using `Validator.TryValidateObject()`. Return `400 Bad Request` on validation failure.

**Checkpoint 3:**

Test the POST endpoint using Swagger with:

* Valid product data (expect `201 Created`)
* Invalid product data (too short name, missing or negative price; expect `400 Bad Request` with error details)

**Checkpoint 4:**

Update the `PUT /api/products/{id}` endpoint to perform the same validation checks before updating. Return validation errors or `404 Not Found` as appropriate.

### What is the purpose of these checkpoints?

These checkpoints guide learners to add and enforce data validation rules on API input models, perform manual validation in Minimal API handlers, and handle validation failures gracefully with proper HTTP status codes and error messages.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise #9: API Documentation with OpenAPI

**Learning Standards**: 
* In ASP.NET Core Minimal APIs, OpenAPI documentation can be automatically generated using built-in support for Swagger/OpenAPI specifications.

  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

- Enable OpenAPI/Swagger support in a minimal API project
- Describe endpoints using metadata for better documentation
- Use Swagger UI to test API endpoints interactively
- Understand the benefits of auto-generated API documentation for teams and tools
  
### Narrative Summary

Earlier exercises introduced Swagger as a UI tool for testing endpoints. This exercise builds on that by introducing OpenAPI — the underlying standard behind Swagger.

OpenAPI is a specification that describes what an API does. From this description, tools like Swagger generate interactive documentation, sample inputs/outputs, and client SDKs. Minimal APIs in .NET include native support for OpenAPI documentation via Swagger.

Enabling OpenAPI in a minimal API project involves three key steps:

1. Registering services that expose endpoint metadata.
2. Enabling middleware that serves the Swagger UI.
3. Enhancing endpoint definitions with summaries, descriptions, and names to improve generated documentation.

Clear documentation is critical when working in teams, integrating APIs with frontend/mobile clients, or exposing endpoints for public or partner consumption. Even for solo developers, OpenAPI reduces friction when debugging or scaling APIs.

### Instructions

**Checkpoint 1:**

Enable Swagger/OpenAPI documentation support in the application.

* Register the required services: `AddEndpointsApiExplorer()` and `AddSwaggerGen()`
* Enable the middleware: `UseSwagger()` and `UseSwaggerUI()` inside the development environment block

**Checkpoint 2:**

Add metadata to existing endpoints using `.WithName()`, `.WithSummary()`, and optionally `.WithDescription()` to improve the generated documentation and developer experience in Swagger UI.

**Checkpoint 3:**

Run the application and inspect the `/swagger` endpoint to verify:

* All endpoints appear with HTTP methods and routes
* Metadata like names and summaries are visible
* The interface allows executing live API requests with example inputs

**Checkpoint 4:**

Observe the practical advantages of OpenAPI, including:

* Easier onboarding for new developers
* Faster client integration using code generation or API exploration
* Reduced debugging time through interactive request/response testing

### What is the purpose of these checkpoints?

These checkpoints walk learners through enabling OpenAPI in a Minimal API application, improving documentation clarity through metadata, and evaluating Swagger UI as a development and integration tool. This exercise establishes foundational practices for maintaining professional-grade APIs with clear, accessible documentation.

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser

## Exercise #10: Review

**Learning Standards**: 

N/A

  
### Which course outcomes will be covered by this exercise?

- Learners will review concepts covered in lesson.

### Narrative Summary

Key concepts covered: 

* Project setup using `dotnet new web`
* Writing endpoints using `MapGet()`, `MapPost()`, `MapPut()`, and `MapDelete()`
* Handling input from the route, query string, and request body
* Structuring data using C# model classes like `Product`
* Applying validation using `[Required]`, `[Range]`, and checking results
* Returning consistent and meaningful HTTP responses
* Enabling OpenAPI/Swagger for interactive testing and self-documentation

### Instructions

1. Explore the final code of the Product API.
2. Enhance the API (not mandatory)

#### What is the purpose of these checkpoints?
To enable the user to explore the codebase used throughout the lesson and possibly enhance it. 

### What would you like to have in the workspace for this exercise? Share your plan below.

Code Editor | Terminal | Web Browser


