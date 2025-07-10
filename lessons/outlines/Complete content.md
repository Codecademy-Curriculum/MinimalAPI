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

### Narrative
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

* In ASP.NET Core Minimal APIs, the `MapPost()` method can be used with model binding to accept and process JSON data as C# objects.

  
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


#### Narrative

In earlier exercises, you created and retrieved items using POST and GET. Now, we'll complete the full CRUD cycle by learning how to update and delete items. This means you’ll be able to update existing data and remove it when it's no longer needed.

**Updating resource:**

To update a resource, we use the **PUT** method. In Minimal APIs, this is done with `MapPut()`. Just like `MapGet()` uses a route like `"/api/products/{id}"` to get a specific item, `MapPut()` uses a route with an ID to know which item to update. The updated data is sent in the request body as JSON.

Here’s the basic structure:
```cs
app.MapPut("/items/{id}", (int id, Item updatedItem) =>
{
    // logic to update the item
});
```
* `/items/{id}` means the client must specify the ID of the item to update.
* `updatedItem` is the new data sent in the request body.

**Deleting resource:**

Just like `MapPost()` is used to create something and `MapPut()` is used to update something, `MapDelete()` is used to delete something from your API.

Here’s the basic structure:
```cs
app.MapDelete("/items/{id}", (int id) => {
    // logic to delete the item
});
```

* The `"{id}"` part is a route parameter. It means the client must specify the ID of the item they want to delete.
* The `id` is passed into the lambda function so we can use it to find the item.

Both methods should return appropriate HTTP status codes:

* **204 No Content** for a successful update or delete with no response body.
* **404 Not Found** if the item doesn’t exist.
* **200 OK** if you return the updated or deleted item as confirmation.

### Instructions

**Checkpoint 1:** Add a PUT Endpoint to Update Products

To update a product, you need:

* A route parameter to identify the product (`id`)
* A JSON body with the updated data

Below your existing endpoints, add this:

```cs
app.MapPut("/api/products/{id}", (int id, Product updated) =>
{
    for (int i = 0; i < products_list.Count; i++)
    {
        if (products_list[i].Id == id)
        {
            products_list[i] = updated;
            return Results.Ok(updated);
        }
    }
    return Results.NotFound();
});
```

This:

* Reads `{id}` from the route
* Searches for a product with that ID
* Replaces it with the new data from the request body
* Returns:
   - `200 OK` with the updated product if found
   - `404 Not Found` if no product matches the ID

This shows route parameter binding combined with a request body.

**Checkpoint 2:** Test the PUT endpoint using Swagger

1. Create a product first
   
* Go to `POST /api/products`
* Use this sample:
```json
{
  "id": 2,
  "name": "Pen",
  "price": 19.99
}
```

2. Update that product

* Go to `PUT /api/products/{id}`
* Enter `2` for ID

Use this updated JSON:

```json
{
  "id": 2,
  "name": "Premium Pen",
  "price": 29.99
}
```

Expected result:

- Status: `200 OK`
- The returned product shows the new name and price

3. Try updating a non-existent ID

- Enter `99` as ID and test again
- You should get: `404 Not Found`
  
**Checkpoint 3:** Add a DELETE Endpoint to Remove Products

To delete a product, you only need the `id` in the route.

Add this below the PUT endpoint:

```cs
app.MapDelete("/api/products/{id}", (int id) =>
{
    var item_to_remove = products_list.FirstOrDefault(p => p.Id == id);
    if (item_to_remove is null)
    {
        return Results.NotFound();
    }

    products_list.Remove(item_to_remove);
    return Results.NoContent();
});
```

This:

- Uses `{id}` from the URL to find the product
- Removes it from `products_list`

Returns:
- `204 No Content` if deletion is successful
- `404 Not Found` if no match is found

This completes the CRUD set using route parameter binding for deletion.

**Checkpoint 4:** Test the DELETE endpoint using Swagger

1. Add a product to delete

Use `POST` to add:

```json
{
  "id": 3,
  "name": "Marker",
  "price": 9.99
}
```

2. Delete it

- Go to `DELETE /api/products/{id}`
- Enter `3` and click **Execute**

**Expected result:**

- Status: `204 No Content`
- Product is removed

3. Try deleting again

- Enter `3` again
- You should now get: `404 Not Found`


You now have a complete working API that supports:

- **POST** - Create products
- **GET** - Read products
- **PUT** - Update products
- **DELETE** - Remove products

You’ve also seen how ASP.NET Core handles different input sources:

- Route parameters for id
- JSON body for POST and PUT
- Query strings for search

You’ve completed the entire CRUD workflow for a minimal API!

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

#### Narrative

In the last exercise, you completed full CRUD using:

- `POST`, `GET`, `PUT`, and `DELETE`
- Route parameters, query strings, and JSON body binding

So far, your API accepts any data — even invalid or empty values.

Now, you’ll learn how to protect your API by:

- Rejecting bad input (e.g., empty names, negative prices)
- Returning clear error messages and `400 Bad Request` responses
- Writing simple, automatic validations using attributes in the model

This builds real-world habits. For example:

- A shopping cart shouldn’t allow products with no name or price below 0
- Users should see helpful error messages when they send bad input

### Instructions

**Checkpoint 1:** Add Validation Attributes to Your Model

In your existing Product class, add Data Annotations to enforce rules.

Go to the bottom of your `Program.cs` and update the `Product` class like this:

```cs
public class Product
{
    public int Id { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 3)]
    public string Name { get; set; }

    [Range(0.01, 100000)]
    public decimal Price { get; set; }
}
```

What this means:

- `[Required]`: `Name` must not be null or empty
- `[StringLength]`: `Name` must be between `3` and `100` characters
- `[Range]`: `Price` must be between `0.01` and `100000`

These annotations are automatically checked during model binding when a client sends a request.

But for validation to take effect, we must manually check ModelState in minimal APIs (unlike MVC).

**Checkpoint 2:** Modify POST Endpoint to Validate Input

Update your existing `POST` endpoint to check if the product is valid.

Replace the existing `POST /api/products` endpoint with this:

```cs
app.MapPost("/api/products", (HttpContext context, Product pr) =>
{
    var validationResults = new List<ValidationResult>();
    var validationContext = new ValidationContext(pr);

    if (!Validator.TryValidateObject(pr, validationContext, validationResults, true))
    {
        return Results.BadRequest(validationResults);
    }

    products_list.Add(pr);
    return Results.Created($"/api/products/{pr.Id}", pr);
});
```

What this code does:

- `ValidationContext`: tells the system what to validate
- `TryValidateObject`: checks the model using your data annotations
- If invalid &rarr; returns a `400 Bad Request` with the list of errors
- If valid &rarr; proceeds to add the product and return `201 Created`

**Checkpoint 3:** Test Validation with Swagger

**Test 1: Valid product (should succeed)**

Go to `POST /api/products`

Use:

```json
{
  "id": 10,
  "name": "Gaming Keyboard",
  "price": 199.99
}
```

Expected Result:

- Status: `201 Created`
- Product is added

**Test 2: Invalid product (short name, missing price)**

```json
{
  "id": 11,
  "name": "AB"
}
```

Expected Result:

- Status: `400 Bad Request`
- Response body contains a list of errors:
    - Name must be at least 3 characters
    - Price is required and must be between 0.01 and 100000

**Test 3: Negative price**

```json
{
  "id": 12,
  "name": "Desk",
  "price": -10
}
```

Expected:

- Status: `400 Bad Request`
- Error message: price must be >= 0.01

**Checkpoint 4:** Add Basic Validation to PUT Endpoint

Just like `POST`, you must validate incoming data in the `PUT` handler too.

Update your existing `PUT` endpoint like this:

```cs
app.MapPut("/api/products/{id}", (int id, Product updated) =>
{
    var validationResults = new List<ValidationResult>();
    var context = new ValidationContext(updated);

    if (!Validator.TryValidateObject(updated, context, validationResults, true))
    {
        return Results.BadRequest(validationResults);
    }

    for (int i = 0; i < products_list.Count; i++)
    {
        if (products_list[i].Id == id)
        {
            products_list[i] = updated;
            return Results.Ok(updated);
        }
    }

    return Results.NotFound();
});
```

This ensures that updates also go through validation. Invalid updates will be blocked with clear error responses.

This exercise builds the foundation for better APIs by ensuring data correctness at the boundary.

## Exercise #9: API Documentation with OpenAPI

**Learning Standards**: 
* In ASP.NET Core Minimal APIs, OpenAPI documentation can be automatically generated using built-in support for Swagger/OpenAPI specifications.

  
### Which course outcomes will be covered by this exercise?

**Learners Will Be Able To**:

- Enable OpenAPI/Swagger support in a minimal API project
- Describe endpoints using metadata for better documentation
- Use Swagger UI to test API endpoints interactively
- Understand the benefits of auto-generated API documentation for teams and tools

### Narrative:

In Exercise 4, you briefly saw Swagger in action — it gave you a simple interface to test POST endpoints. Now, we’ll go deeper into OpenAPI, the underlying standard that powers Swagger.

OpenAPI is a widely-used specification that lets APIs describe themselves in a machine-readable way. Tools like Swagger can read this description and generate:

- Beautiful, interactive UIs for developers to explore and test endpoints
- Up-to-date docs automatically from your code
- Client SDKs and server mocks in other languages

For Minimal APIs, OpenAPI support comes built-in. You only need to:

- Enable the OpenAPI services and middleware
- Add descriptions and summaries to your endpoints
- Launch Swagger UI to try out all routes in a browser

This is a critical skill for building modern APIs — especially in teams. Whether you’re onboarding a teammate or integrating with a mobile app, clear API documentation makes your work accessible and trustworthy.

### Instructions:

**Checkpoint 1:** Enable Swagger/OpenAPI Services and Middleware

If you've completed Exercise 4, you may already have Swagger installed. If not, follow these steps:

**Step 1:** Add Swagger services at the top of Program.cs:

```csharp
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
```

This enables OpenAPI generation for your endpoints.

**Step 2:** Use Swagger middleware during app startup:

After `var app = builder.Build();`, add this:

```csharp
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```

This serves the Swagger JSON (`/swagger/v1/swagger.json`) and shows the interactive UI at `/swagger`.

**Checkpoint 2:** Add Descriptions to Your Endpoints

You can improve Swagger-generated docs by adding metadata using the `.WithName()` and `.WithSummary()` extensions on each endpoint.

Here’s how to update an existing GET endpoint:

```csharp
app.MapGet("/api/products/{id}", (int id) =>
{
    var product = products_list.FirstOrDefault(p => p.Id == id);
    return product is not null ? Results.Ok(product) : Results.NotFound();
})
.WithName("GetProductById")
.WithSummary("Retrieves a product using its ID.");
```

**Explanation:**

- `.WithName("...")`: Optional friendly name for documentation tools
- `.WithSummary("...")`: Brief description shown in Swagger UI
- You can also add `.WithDescription("...")` for longer explanations

Repeat this pattern for your key endpoints (`POST`, `PUT`, `DELETE`) to make your docs more readable and self-explanatory.

**Checkpoint 3:** Test API with Swagger UI

Run your app using:

```bash
dotnet run
```

Then open your browser and go to:
```bash
https://localhost:5001/swagger
```

You should see:
- All endpoints listed with their method and route
- Descriptions beside each endpoint (if added)
- Example request bodies and responses
- Input boxes to try out live API calls

**Checkpoint 4:** Understand the Value of OpenAPI

Swagger isn't just a dev tool — it helps with:

- Developer onboarding: Clear docs mean less confusion
- Client integration: Frontend/mobile teams can test APIs early
- Automation: Tools can generate client SDKs or mocks from OpenAPI JSON
- Debugging: You can test all API behavior interactively

Even if you're building a small app alone, clear documentation = fewer bugs and less confusion.

## Exercise #10: Summary

**Learning Standards**: 
* N/A
  
### Which course outcomes will be covered by this exercise?
- 

**Learners Will Be Able To**:

* Use `MapPut()` to define a PUT endpoint that updates an existing resource using route parameters and body data
* Use `MapDelete()` to define a DELETE endpoint that removes a resource using route parameters
* Return appropriate HTTP status codes such as `200 OK`, `204 No Content`, and `404 Not Found`
* Demonstrate full CRUD capability by integrating POST, GET, PUT, and DELETE in one app


#### Narrative

Great work completing the journey through building APIs with ASP.NET Core Minimal APIs. In this lesson, you built up your knowledge step-by-step — from writing your first endpoint to creating a fully functional and validated API with documentation.

In this lesson, you learned:
- how to create a new minimal API project using `dotnet new web`
- how to define your first GET and POST endpoints using `MapGet()` and `MapPost()`
- how to receive JSON data from the request body using model binding
- how to handle route parameters (`/api/products/{id}`) for resource identification
- how to use query string parameters (`/api/products/search?name=notebook`) for filtering
- how to create and use a C# model class to define your data shape (`Product`)
- how to build a simple CRUD system using `GET`, `POST`, `PUT`, and `DELETE`
- how to validate request data using Data Annotations like `[Required]` and `[Range]`
- how to manually trigger model validation and return structured `400 Bad Request` errors
- how to enable Swagger/OpenAPI for automatic documentation
- how to describe endpoints using `.WithSummary()` to enhance documentation
- how to use Swagger UI to explore, test, and debug all endpoints interactively
- the importance of meaningful responses with correct HTTP status codes
- how all these pieces come together to build a functional, documented, and testable API

You went from writing a hard-coded "Hello World" to building a professional-grade API with real-world patterns. This experience prepares you for building real-world web services — with a strong foundation in routing, data modeling, validation, and documentation.

### Instruction

Use the workspace to explore the Product API or experiment with what you’ve learned. Try adding new endpoints, modifying existing ones, or adjusting validation to observe how the API behaves.

