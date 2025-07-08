# Minimal API Design Documents - .NET 8

### Title
Introduction to Minimal API

### Description
This lesson introduces ASP.NET Core Minimal APIs, a simplified approach for building fast HTTP APIs with .NET 8. Students will learn the fundamentals of creating lightweight, efficient APIs without the overhead of traditional controllers. Through hands-on exercises, students will master the basics of minimal API development, from simple endpoints to structured applications with proper organization, validation, and documentation.

### Learning Objective
Create production-ready minimal APIs using .NET 8 with proper project organization, data validation, error handling, and documentation.

### Learning Standards
1. In ASP.NET Core, Minimal APIs are a simplified approach for building fast HTTP APIs with minimal code and configuration, ideal for microservices and apps that require minimal dependencies. [Microsoft Learn - Minimal APIs Overview](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview?view=aspnetcore-8.0)
2. In .NET, the `dotnet new webapi --minimal` command creates a new minimal API project with the basic structure and dependencies. [Microsoft Learn - .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new)
3. In ASP.NET Core Minimal APIs, the `MapGet()` method is used to define `GET` endpoints that retrieve data from the API. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
4. In ASP.NET Core Minimal APIs, route parameters can be captured from the URL using parameter binding and constraints to validate input. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
5. In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
6. In ASP.NET Core Minimal APIs, model binding automatically maps HTTP request data to method parameters using various binding sources like route values, query strings, and request bodies. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
7. In ASP.NET Core Minimal APIs, the `MapPut()` method is used to define `PUT` endpoints that update existing resources in the API. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
8. In ASP.NET Core Minimal APIs, the `MapDelete()` method is used to define `DELETE` endpoints that remove resources from the API. [Microsoft Learn - Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-8.0)
9. In ASP.NET Core Minimal APIs, validation can be implemented using Data Annotations and the built-in validation framework. [Microsoft Learn - Model validation in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation?view=aspnetcore-8.0)
10. In ASP.NET Core Minimal APIs, error handling can be implemented using exception handling middleware and standardized problem details responses. [Microsoft Learn - Handle errors in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/error-handling?view=aspnetcore-8.0)
11. In ASP.NET Core Minimal APIs, OpenAPI documentation can be automatically generated using built-in support for Swagger/OpenAPI specifications. [Microsoft Learn - OpenAPI support in minimal API apps](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/openapi?view=aspnetcore-8.0)

### Lesson Outline

## Exercise 1
**Title**: Introduction to Minimal APIs

**Learning Standards**: 
- In ASP.NET Core, Minimal APIs are a simplified approach for building fast HTTP APIs with minimal code and configuration, ideal for microservices and apps that require minimal dependencies.
- In .NET, the `dotnet new webapi --minimal` command creates a new minimal API project with the basic structure and dependencies.

**Narrative**: 
- Define Minimal APIs and contrast with traditional controller-based approaches
- List key benefits: reduced code, better performance, simplified structure
- Explain when to choose Minimal APIs vs controllers
- Introduce the .NET CLI command for creating minimal API projects
- Create and run a new minimal API project to see it working

**Checkpoints/Instructions**:
1. Create a new minimal API project using `dotnet new webapi --minimal`
2. Run the application using `dotnet run`
3. Explore the generated Program.cs file structure
4. Test the default endpoint in a browser

**Notes**:
- Emphasize the simplicity and reduced ceremony compared to traditional APIs
- Show the performance benefits and reduced memory footprint
- Do not assume learner knows controller-based APIs
- Focus on getting students up and running with their first minimal API

## Exercise 2
**Title**: Basic GET Endpoints

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapGet()` method is used to define `GET` endpoints that retrieve data from the API.

**Narrative**: 
- Focus on `MapGet()` method for creating simple `GET` endpoints
- Show different static route patterns and how they map to URLs
- Demonstrate returning different data types (strings, numbers, simple objects)
- Test endpoints using a web browser for immediate feedback

**Checkpoints**:
1. Create simple `GET` endpoints using `MapGet()` with static routes
2. Build multiple `GET` endpoints with different route patterns
3. Return different data types from endpoints
4. Test all endpoints using a web browser

**Notes**:
- Start with the simplest possible endpoints to build confidence
- Focus on mastering the `MapGet()` syntax and route patterns
- Use browser testing to keep things accessible
- Build solid foundation before introducing parameters

## Exercise 3
**Title**: Working with Route Parameters

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, route parameters can be captured from the URL using parameter binding and constraints to validate input.

**Narrative**: 
- Introduce route parameters for resource identification
- Show how parameters are captured from URLs (e.g., `/api/items/{id}`)
- Demonstrate route constraints for parameter validation
- Build endpoints that use parameters to return specific data

**Checkpoints**:
1. Create `GET` endpoints with route parameters (e.g., `/api/items/{id}`)
2. Apply route constraints to parameters (e.g., `/api/items/{id:int}`)
3. Build endpoints that use parameters to filter or identify resources
4. Test parameterized endpoints with different values

**Notes**:
- Focus on the fundamentals of route parameters and constraints
- Show how parameters enable resource identification patterns
- Demonstrate route constraints as built-in validation
- Prepare students for using parameters in PUT/DELETE operations

## Exercise 4
**Title**: Creating Resources with MapPost()

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API.
- In ASP.NET Core Minimal APIs, model binding automatically maps HTTP request data to method parameters using various binding sources like route values, query strings, and request bodies.

**Narrative**: 
- Focus on `MapPost()` method for creating new resources
- Introduce model binding through JSON request body handling
- Demonstrate automatic deserialization of JSON to C# objects
- Show proper response codes and basic resource creation patterns

**Checkpoints**:
1. Create `POST` endpoints that accept JSON data in request bodies
2. Demonstrate automatic model binding from JSON to C# objects
3. Return appropriate status codes for successful resource creation
4. Test `POST` endpoints using simple HTTP tools

**Notes**:
- Build on the solid GET foundation from previous exercises
- Introduce model binding as an automatic process
- Focus on fundamental POST patterns without complex business logic
- Show how POST complements GET for basic API functionality

## Exercise 5
**Title**: Updating and Deleting Resources

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapPut()` method is used to define `PUT` endpoints that update existing resources in the API.
- In ASP.NET Core Minimal APIs, the `MapDelete()` method is used to define `DELETE` endpoints that remove resources from the API.

**Narrative**: 
- Complete CRUD operations with `MapPut()` and `MapDelete()` methods
- Use route parameters for resource identification (introduced in Exercise 2)
- Show appropriate response codes for update and delete operations
- Demonstrate full CRUD functionality working together

**Checkpoints**:
1. Add `PUT` endpoints for updating existing resources using route parameters
2. Create `DELETE` endpoints for removing resources using route parameters
3. Return appropriate HTTP status codes for update and delete operations
4. Test complete CRUD functionality with all HTTP methods

**Notes**:
- Complete the CRUD foundation with less common but important operations
- Reinforce route parameter concepts from Exercise 2
- Emphasize proper HTTP status codes for different operations
- Show how all CRUD operations work together as a complete API

## Exercise 6
**Title**: Basic Data Validation

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, validation can be implemented using Data Annotations and the built-in validation framework.
- In ASP.NET Core Minimal APIs, error handling can be implemented using exception handling middleware and standardized problem details responses.

**Narrative**: 
- Introduce Data Annotations for input validation on model properties
- Show basic validation implementation in endpoints
- Demonstrate simple error responses with appropriate HTTP status codes
- Implement basic error handling for common validation scenarios

**Checkpoints**:
1. Add Data Annotations to model classes for basic validation
2. Handle validation errors in endpoints with appropriate responses
3. Create proper error responses with appropriate HTTP status codes
4. Test validation with both valid and invalid data

**Notes**:
- Focus on essential validation using built-in Data Annotations
- Keep error handling simple and practical for beginners
- Show how validation integrates naturally with model binding
- Emphasize the importance of proper HTTP status codes

## Exercise 7
**Title**: API Documentation with OpenAPI

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, OpenAPI documentation can be automatically generated using built-in support for Swagger/OpenAPI specifications.

**Narrative**: 
- Introduce OpenAPI specification for automatic documentation generation
- Show enabling Swagger/OpenAPI services and middleware
- Demonstrate basic endpoint descriptions for better documentation
- Highlight interactive testing capabilities with Swagger UI

**Checkpoints**:
1. Enable OpenAPI support and configure Swagger UI in the minimal API project
2. Add basic descriptions to endpoints for clear documentation
3. Use Swagger UI to test and explore all API endpoints interactively
4. Understand the benefits of automatic API documentation for development

**Notes**:
- Focus on enabling and using Swagger UI rather than extensive customization
- Emphasize the importance of API documentation for developer experience
- Show how minimal APIs make it easy to generate comprehensive documentation
- Demonstrate the testing capabilities that Swagger UI provides

## Exercise 8
**Title**: Summary

**Learning Standards**: 
- N/A

**Narrative**: 
- Summarize key concepts learned throughout the lesson
- Review the complete API functionality built across all exercises
- Highlight the progression from simple endpoints to a full CRUD API
- Discuss next steps for advancing minimal API skills

**Instructions**:
- Review the complete API example demonstrating integrated functionality
- Identify the key concepts mastered: HTTP methods, routing, model binding, validation, documentation
- Suggest areas for improvement and potential extensions
- Provide guidance on continuing the minimal API learning journey

---

# Resources

## Microsoft Learn Official Resources

### Primary Learning Paths
- **[ASP.NET Core Minimal API Learning Path](https://learn.microsoft.com/en-us/training/paths/aspnet-core-minimal-api/)** - Complete training path from basics to database integration
- **[Build a web API with minimal API](https://learn.microsoft.com/en-us/training/modules/build-web-api-minimal-api/)** - Core module covering fundamental concepts and routing

### Essential Tutorials
- **[Tutorial: Create a minimal API with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api)** - Step-by-step tutorial building a complete CRUD API
- **[Back-end Web Development with .NET Video Series](https://learn.microsoft.com/en-us/shows/back-end-web-development-with-dotnet-for-beginners/implementing-a-web-api-in-dotnet)** - Video walkthrough of Todo API with CRUD operations

### Reference Documentation
- **[Minimal APIs Overview](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview)** - Introduction and comparison with controller-based APIs
- **[Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis)** - Comprehensive reference for all minimal API features
- **[Authentication and Authorization in Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/security)** - Security implementation guide
- **[Testing Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/test-min-api)** - Testing strategies and best practices

## Community Resources

### Comprehensive Guides
- **[FreeCodeCamp Minimal API Handbook](https://www.freecodecamp.org/news/create-a-minimal-api-in-net-core-handbook/)** - Complete guide from setup to production-ready API
- **[Minimal APIs without cluttering Program.cs](https://www.mattbutton.com/minimal-apis-dotnet/)** - Code organization and best practices

### Advanced Topics
- **[Organizing ASP.NET Core Minimal APIs](https://www.tessferrandez.com/blog/2023/10/31/organizing-minimal-apis.html)** - Enterprise-level code organization patterns
- **[Advanced Minimal API Techniques](https://blog.jetbrains.com/dotnet/2023/04/25/introduction-to-asp-net-core-minimal-apis/)** - Detailed exploration of advanced features
