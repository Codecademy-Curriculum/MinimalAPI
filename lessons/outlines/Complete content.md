## Exercise #1: Getting Started with Minimal API

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

#### Narrative
Imagine you're building a small app to track expenses, log workouts, or collect feedback — but you don’t want to create a dozen files just to accept one request. That’s exactly what Minimal APIs are built for.

In this first exercise, you’ll learn how to create a new minimal API project using the .NET CLI and explore how this streamlined approach lets you define endpoints quickly — with just a few lines of code.

Minimal APIs are different from traditional controller-based ASP.NET Core applications. Instead of needing controllers, services, routing attributes, and lots of setup, you define everything you need in a single file — usually Program.cs.

**Why use Minimal APIs?**
* Less boilerplate code
* Faster startup time
* Great for microservices or small web APIs
* Easier to learn when starting out

By the end of this exercise, you'll have a fully working Minimal API up and running on your machine — and you’ll understand why it's minimal, how it works, and what it can become as you build on it in future lessons.

### Instructions

**Checkpoint 1:** Create a new minimal API project.

Open your terminal and type:
```bash
dotnet new web -o MinimalApiDemo
```

This does the following:
* `dotnet new web`: Creates a new web project using the Minimal API template.
* `-o MinimalApiDemo`: Puts the project in a new folder named `MinimalApiDemo`.

**Checkpoint 2:** Move into your new project folder

```bash
cd MinimalApiDemo
```

**Checkpoint 3:** Build the project

```bash
dotnet build
```
You should see a Build succeeded message if everything is okay.

**Checkpoint 4:** Run the application
```bash
dotnet run
```
You’ll see output like this:

```bash
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
```
This means your API is now running locally.

**Checkpoint 5:** Test the default endpoint in your browser
Copy the HTTPS URL shown in the terminal — for example:

```bash
https://localhost:5001
```
Then paste it in your browser and press Enter.

You should see:

```bash
Hello World!
```

This is the default response from the root endpoint `/`.

Open Program.cs in your code editor. You’ll see:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

Here’s what this does:
- `CreateBuilder()` sets up the web server.
- `MapGet("/", ...)` defines a route — a GET endpoint for `/` (the root path).
- `app.Run()` starts the application.

This is the essence of a Minimal API — just enough code to create and respond to HTTP requests.

## Exercise # 2: Basic `GET` Endpoints 
**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapGet()` method is used to define `GET` endpoints that retrieve data from the API.

### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:
* Use `MapGet()` to define static `GET` endpoints.
* Understand how route paths map to endpoints.
* Return various response types including strings, numbers, and objects.

### Narrative 
In the previous exercise, you created and ran a minimal API project. You might have seen a line like this in the generated `Program.cs` file:
``` csharp
app.MapGet("/", () => "Hello World!");
```
This line defines a GET endpoint for the default route path `/`. 

**What is an endpoint?**

An endpoint is just a specific URL path in your API (like `/ `or `/hello`) that users or systems can call to get a response.
So, the line:

` app.MapGet("/", () => "Hello World!");` means when someone visits the default path `https://localhost:5001`, they’ll get `"Hello World!"` as the response.

**Note:** The port number (`5001`) might be different on your system. Always use the exact URL shown in your terminal after running `dotnet run`.

Now let’s add more endpoints to return:
* A custom string
* The current time
* A JSON object

### Instructions
**Checkpoint 1:** Add a new static GET endpoint that returns a string. To do this, add this new line just below the default route `/`:
``` csharp
app.MapGet("/hello", () => "Hello from /hello endpoint");
```
 * Type `dotnet run` in the terminal to start the app. 
* Then, visit the new `hello` endpoint in the browser by going to:
`https://localhost:5001/hello` (or the URL shown in your terminal with`/hello` added at the end).

**Checkpoint 2:** Add a new GET endpoint that returns the current time. To do this, add this new line just below the previous one:
``` csharp
app.MapGet("/time", () => DateTime.UtcNow);
```
* If your app is still running, stop it by pressing `Ctrl + C` in the terminal.
* Restart the app by typing `dotnet run` in the terminal.
* In your browser, go to: `https://localhost:5001/time` (or whatever port is shown in your terminal).
* You should see a date and time which is nothing but the current UTC time, automatically returned by your API.

**Checkpoint 3:** Add a new GET endpoint that returns a JSON object. To do this, add this new line just below the previous one:
``` csharp
app.MapGet("/info", () => new { name = "My API", version = "1.0" });
```

* If your app is still running, stop it by pressing `Ctrl + C` in the terminal.
* Restart the app by typing `dotnet run` in the terminal.
* In your browser, go to: `https://localhost:5001/info` (or whatever port is shown in your terminal).
* You should see JSON output like this:
```json
{
  "name": "My API",
  "version": "1.0"
}

```
## Exercise # 3: Working with Route Parameters
**Learning Standards**: 
- In ASP.NET Core Minimal APIs, route parameters can be captured from the URL using parameter binding and constraints to validate input.

### Which course outcomes will be covered by this exercise?
**Learners Will Be Able To**:
* Use route parameters to capture values from URLs.
* Apply route constraints to validate input.
* Bind parameters directly to lambda inputs in `MapGet()`.

### Narrative 
So far, your endpoints returned fixed responses like a string or the current time. But real-world APIs often need to work with dynamic input — for example, greeting different users or returning a specific item based on the URL. This is where route parameters come in.

Let’s start with a simple example:
```csharp
app.MapGet("/greet/{name}", (string name) => $"Hello, {name}!");
```
This defines an endpoint that listens to paths like:
* `/greet/John` &rarr; returns `"Hello, John!"`
* `/greet/Sarah` &rarr; returns `"Hello, Sarah!"`
Hence, in this route, `{name}` is a route parameter. When someone visits `/greet/John`, the value `"John"` is passed into the lambda as the `name` variable.

**What is a route parameter?**
A route parameter is a value placed directly in the URL path, allowing your API to respond dynamically based on user input. It replaces part of the URL with a variable value.

Now let’s make things more specific — suppose you want to return data only when a valid **numeric ID** is provided. You can use a route constraint:
```csharp
app.MapGet("/api/items/{id:int}", (int id) => $"Item ID is: {id}");
```

The `:int` constraint ensures this route only matches if `{id}` is an integer. 
So`/api/items/5` works, but `/api/items/abc` returns a **404**.
Route constraints act like built-in validation — they help ensure that the input is correct before your code runs.
These patterns are the foundation for building real-world APIs that identify resources by ID, like `/users/{id}`, `/products/{id}`, or `/orders/{id}` — and prepare you to build `GET by ID`, `PUT`, and `DELETE` operations in upcoming exercises.

### Instructions
**Checkpoint 1:** Create a `GET` endpoint with a route parameter. To do this, add the following line below the previous routes in your `Program.cs` file:
```csharp
app.MapGet("/api/items/{id}", (string id) => $"You requested item with ID: {id}"); 
```
 * Type dotnet run in the terminal to start the app (or restart it if already running).
* Copy the URL shown in your terminal (for example: `https://localhost:5001`) and paste it into your browser.
* Add `/api/items/123` at the end, like this:
```bash
https://localhost:5001/api/items/123
```
You should see:
```csharp
You requested item with ID: 123
```
Try it with other IDs like:
```bash
/api/items/abc123
/api/items/item-999
```

**Checkpoint 2:** Add a `GET` endpoint with an integer-only route parameter using a constraint. To do this, add this new line just below the previous one:
```csharp
app.MapGet("/api/items/int/{id:int}", (int id) => $"Item ID as integer: {id}");
```

* If your app is still running, stop it by pressing `Ctrl + C` in the terminal.
* Restart the app by typing `dotnet run` in the terminal. 
* In your browser, go to: `https://localhost:5001/api/items/int/42` (or use the URL shown in your terminal with `/api/items/int/42` added).
* You should see:
```bash
Item ID as integer: 42
```
Now try an invalid value like:
```bash
/api/items/int/hello
```
You should see a **`404 Not Found`** page. This is because the route only accepts integer values, thanks to the `:int` constraint.

**Checkpoint 3:** Create an endpoint that filters data based on a username. To do this, add this new line just below the previous one:
```csharp
app.MapGet("/api/users/{username}", (string username) =>
{
    var result = new { message = $"Profile for user: {username}" };
    return result;
}); 
```
In the above code, you're using `{username}` to return a personalized message or profile.
* Restart the app by typing dotnet run.
* In your browser, go to:
```bash
https://localhost:5001/api/users/alex
```
You should see:
```json
{
  "message": "Profile for user: alex"
}
```
Try with other usernames:
```bash
/api/users/sam
/api/users/guest42
```
## Exercise #4: Creating a Simple POST Endpoint

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:
* Use `MapPost()` to create a POST endpoint.
* Understand how POST is used to send data to the server.
* Return a success message using `Results.Created()`.
* Test POST using the built-in Swagger UI.
  
### Narrative
So far, you've used `MapGet()` to return responses. Now, you'll learn how to use `MapPost()` to receive data.
POST is used when the client wants to send data to the server — for example, when submitting a message or creating a new record.
To test this easily, you'll also learn how to enable Swagger, a built-in UI for trying out API endpoints directly in your browser.

### Instructions

**Checkpoint 1:** 
Since we had used the `dotnet new web` template to create a new project, Swagger is not pre-configured. Follow these steps to enable Swagger in your project:

**1. Add Swagger package to your project**
In the terminal, inside your project folder, run:
```bash
dotnet add package Swashbuckle.AspNetCore
```
**2. Update your `Program.cs` file**

At the top of Program.cs, before the `Build()`, add:
```cs
// Add Swagger services
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
var builder = WebApplication.CreateBuilder(args);
```
After the `Build()`, add the following:

```csharp
var app = builder.Build();
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```
This allows Swagger UI to be available during development.

Now, run your app using `dotnet run` and visit `https://localhost:5001/swagger`(or whatever port is shown in your terminal) to try out your API endpoints. 

For now, you can only see GET endpoint as we have only added `MapGet()`. Let us add `MapPost()` to get POST endpoint.

**Checkpoint 2:**  Let’s create a simple POST endpoint. To do this, add the following code below the default `MapGet()` line in the `Program.cs` file. 
``` csharp
app.MapPost("/api/message", (string body) =>
{
    return Results.Created("/api/message", $"You sent: {body}");
});
```
This code:
* Defines a POST route at `/api/message`.
* Accepts plain text from the request body.
* Sends back a confirmation using `Results.Created()`.

Save the file and run your app using `dotnet run`.
In the browser, open Swagger by visiting `https://localhost:5001/swagger` (or whatever port is shown in your terminal). If it is already opened, simply refresh the page. You’ll now see your API documented in Swagger UI.

**Checkpoint 3:** Test the POST endpoint using Swagger by following these steps:
1. Find: `POST /api/message`
2. Click **"Try it out"**
3. In the Request body, type:
```csharp
Hello from Swagger!
```
4. Click ** Execute** 

You should see:
* Status: `201 Created`
* Response body:
```arduino
"You sent: Hello from Swagger!"
```

## Exercise #5: Accepting JSON Using Model Binding

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, the `MapPost()` method is used to define `POST` endpoints that create new resources in the API.
  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:
* Accept JSON data in a POST request.
* Create a matching C# class for the expected data.
* Use model binding to convert JSON into a C# object.
* Test the endpoint using Swagger UI.

### Narrative
In the previous Exercise, you created a basic `POST` endpoint that accepted **plain text**. That was a great first step toward understanding how clients send data to a server.
But in real-world applications, data is usually sent in a structured format like **JSON**. For example, when you add a new item to a shopping cart or create a user account, the client sends several values (like `id`, `name`, or `email`) bundled as a JSON object.

Here’s an example of JSON:
```json
{
  "id": 1,
  "name": "Notebook"
}
```
You don’t want to manually extract each field from raw text. Instead, ASP.NET Minimal APIs handle this automatically for you — it’s called **model binding**.
In this exercise, you’ll:

* Define a class in C# to represent the incoming data.
* Accept a JSON object in the request body.
* Let ASP.NET Core automatically convert (or bind) the JSON to your class.
* Test everything using Swagger — just like in the previous exercise.
Let’s get started.

### Instructions

**Checkpoint 1:** 
At the bottom of your `Program.cs` file, add the following C# class that matches the JSON structure below  `app.Run()`:
```csharp
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```
**What’s happening here?**
* This class has two properties: `Id` (a number) and `Name` (a string).
* It matches the format of the JSON object we’ll be sending from Swagger.
Now, add a POST endpoint that accepts JSON. To do this, add the following below your existing endpoints:
```csharp
app.MapPost("/api/items", (Item item) =>
{
    return Results.Created($"/api/items/{item.Id}", item);
});
```
**What’s happening here?**
* This route listens for a POST request at `/api/items`.
* When a request comes in with a JSON body, ASP.NET automatically reads it and fills the Item object for you.
* You send the same object back in the response with status code **201 Created**.

Save the file and run your app using `dotnet run`.
In the browser, open Swagger by visiting `https://localhost:5001/swagger` (or whatever port is shown in your terminal). You’ll see Swagger UI listing your API endpoints.

**Checkpoint 2:** Test the JSON POST endpoint using Swagger by following these steps:
1. Find: `POST /api/items`
2. Click **"Try it out"**
3. In the Request body, replace any default JSON with this:
```json
{
  "id": 101,
  "name": "Notebook"
}
```
4. Click **Execute** 

You should see:
* Status: `201 Created`
* Response body:
```json
{
  "id": 101,
  "name": "Notebook"
}
```

## Exercise #6: Combining GET and POST — Resource Creation and Retrieval

**Learning Standards**: 
- In ASP.NET Core Minimal APIs, model binding automatically maps HTTP request data to method parameters using various binding sources like route values, query strings, and request bodies.

### Which course outcomes will be covered by this exercise?
**Learners Will Be Able To**:
* Use POST to add a new item with JSON data.
* Use GET to retrieve:
   - An item by ID using **route binding**
  - Items by name using **query string binding**
 * Understand how model binding handles route, query, and body data.
* See how POST and GET form the core of a basic API.

In the last exercises, you learned how to:
* Use GET to return fixed responses
* Use POST to receive plain text or JSON data

Now you’ll build a tiny product API that lets you:
* Add a new product using POST
* Fetch a product by ID using a route parameter
* Search products by name using a query string

This simulates how real APIs work. For example:

* A shopping app lets you **create** a product (POST)
* Then lets others **retrieve** it (GET)

You’ll store the products in a simple **in-memory list**, which is like a temporary database that resets every time you restart the app.

**Checkpoint 1:** 

Define a Product class at the bottom of `Program.cs` by adding the following code after `app.Run()`:

```cs
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

This class defines the shape of each product. It has:
* `Id`: a unique identifier (like 1, 2, 3…)
* `Name`: the product's name (like “Notebook”)
* `Price`: the product's cost

Then, at the top of your `Program.cs`, right after `var app = builder.Build();`, add this:

```cs
var products_list = new List<Product>();
```
This is a simple list that will hold the products temporarily in memory. It gets cleared when you restart the app — perfect for learning.

Next, add a POST endpoint to add new products by adding the following to your existing endpoints:

```cs
app.MapPost("/api/products", (Product pr) =>
{
    products_list.Add(pr);
    return Results.Created($"/api/products/{pr.Id}", pr);
});
```

* Accepts a JSON Product (`pr`) from the client  
* Adds it to the list (`products_list`)
* Returns the product with a **201 Created** status

Now, add a GET endpoint to fetch by `ID` (route parameter) by adding this:

```cs
app.MapGet("/api/products/{id}", (int id) =>
{
    foreach (var item in products_list)
    {
        if (item.Id == id)
        {
            return Results.Ok(item);
        }
    }
    return Results.NotFound();
});
```
This:
* Matches the `{id}` from the URL (e.g., `/api/products/2`)
* Searches the list `products_list`
* Returns the product or a `404` if not found

       
This shows route value binding in action: `{id}` in the URL is passed to the `(int id)` parameter.

Next, add a GET endpoint to search by name (query string)
Now add this:

```cs
app.MapGet("/api/products/search", (string search_name) =>
{
    var product_found = new List<Product>();

    foreach (var item in products_list)
    {
        if (item.Name != null && item.Name.ToLower().Contains(search_name.ToLower()))
        {
            product_found.Add(item);
        }
    }
    return Results.Ok(product_found);
});
```
 This:
* Reads the `?name=...` from the URL (e.g., `/api/products/search?name=notebook`)
* Searches the list `products_list` for matching names
* Returns the matching products as a list `product_found`

This shows query string binding in action: `?name=value` is mapped to the (`search_name`) parameter.

Save the file and run your app using `dotnet run`.
In the browser, open Swagger by visiting `https://localhost:5001/swagger` (or whatever port is shown in your terminal). You’ll see Swagger UI listing your API endpoints.

**Checkpoint 2:** Test the JSON POST endpoint using Swagger by following these steps

**1. Add a product using POST**
* Go to POST `/api/products`
* Click `"Try it out"`
* Enter this JSON:
  
```json
{
  "id": 1,
  "name": "Notebook",
  "price": 149.99
}
```

* Click **Execute**

You should get:
* Status: 201 Created
* Response: the product you added

**2. Get product by `ID` using route parameter**
* Go to GET `/api/products/{id}`
* Click `"Try it out"`
* Enter `1` as the `ID`
* Click **Execute**

You should see:
```json
{
  "id": 1,
  "name": "Notebook",
  "price": 149.99
}
```

3. Search by name using query string
* Go to GET `/api/products/search`
* Click `"Try it out"`
* Enter `notebook` as the name
* Click **Execute**


