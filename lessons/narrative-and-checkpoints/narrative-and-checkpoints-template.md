# Introduction to Minimal API

## Exercise 1: Getting Started with Minimal API

### Narrative:

Let’s say you want to build a small app — maybe to track tasks or record expenses — and you want to start quickly, without setting up controllers, models, or lots of configuration files. This is where Minimal APIs in ASP.NET Core come in.

Minimal APIs in ASP.NET Core offer a fast, lightweight way to build HTTP APIs using just a single file—typically Program.cs. Unlike traditional ASP.NET Core apps that use controllers and models, Minimal APIs let you define routes and responses with minimal setup.

Why use Minimal APIs?
- Less boilerplate
- Faster startup
- Great for microservices or small APIs
- Beginner-friendly

**Tools You’ll Use**

Throughout course, we’ll use the .NET CLI inside Visual Studio Code (VS Code) to stay focused on the essentials. If you'd like to compare how to do this in Visual Studio, you can refer to the official documentation:

[Create a Minimal API with Visual Studio](https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api?view=aspnetcore-8.0&tabs=visual-studio)

To begin, you’ll use the command `dotnet new web -o Name` to create a new minimal API project. Here, `Name` is your desired project name.

This will generate a ready-to-run API inside a folder. Once that’s created, you’ll use three CLI commands to work with it:

- `cd` command to navigate to your project folder
- `dotnet build` compiles your project and checks for any errors.
- `dotnet run` launches a local web server.

You then copy the URL shown in the terminal (like `https://localhost:5001`) and paste it in your browser to view the API response.

**(gif)**

Here's what the default code in Program.cs looks like:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

In the above code:

- `CreateBuilder()` sets up the app’s configuration and services.
- `MapGet()` defines a GET endpoint at the root path (`/`). We shall learn more about this in the next exercise.
- `Run()` starts the web server and begins listening for requests.

This is all it takes to create a working API with a single response.


### Instructions:

1. Checkpoint: Open the terminal and run a command that creates a new web app inside a folder called `MinimalApiDemo`.

Hint: Use `dotnet new web -o Name` syntax to create your Minimal API project folder.
   
2. Checkpoint: Open the `Program.cs` file and locate the line that defines the `MapGet()` method. Change the response from "Hello World!" to a custom string of your choice, such as your name or a greeting. 

Hint: Replace the string inside `() => "Hello World!"` with any other string, like `"Hi from Sarah!"`.

3. Checkpoint: Using the terminal, navigate into the newly created folder and build the project to make sure all files and references are correctly configured. 

4. Checkpoint: Run the application using the terminal. After the server starts, look for the HTTPS URL (such as `https://localhost:5001`) printed in the terminal — this is your API’s base address. Open this URL in the browser. You should see the custom message that you added within `MapGet()` method.

Hint: Always use the full URL shown in the terminal — especially the HTTPS one.


## Exercise 2: Basic GET Endpoints 

### Narrative:
In the previous exercise, you created and ran your first ASP.NET Core Minimal API project. Now, let's look closely at the default code that comes with it.

In the Program.cs file, you likely saw this line:

```csharp
app.MapGet("/", () => "Hello World!");
```

This line defines what’s called a **GET** endpoint. But what does that mean?

**What is a GET endpoint?**

An endpoint is like a signpost or stop on your website or API. When someone visits a certain URL, the server decides what response to give based on which endpoint matches that URL.

A GET request is the most common kind of request used to retrieve data. When your browser visits a website, it's making GET requests behind the scenes.

So this line:

```csharp
app.MapGet("/", () => "Hello World!");
```

means:

"*When someone visits the root URL (i.e., /), send back the text: 'Hello World!'.*"

You can test it by:

1. Running your app with the `dotnet run` command.
2. Copying the URL printed in your terminal (like `https://localhost:5001`).
3. Pasting that into your browser. You’ll see `"Hello World!"` displayed.

**What does `MapGet()` do?**

The `MapGet()` method connects a route (like `/hello`) to a handler function. The handler is the part that generates the response.

Let’s break this down using an example:

```csharp
app.MapGet("/hello", () => "Hi there!");
```

`/hello` &rarr; This is the path the user will visit.

`() => "Hi there!"` &rarr; This is a lambda expression (a short function) that returns the message.

When someone visits `https://localhost:5001/hello`, they will see `"Hi there!"`.

ASP.NET Core Minimal API makes it easy to create multiple endpoints that return different types of data, such as:
* Strings (plain text)
* DateTime (like the current time)
* Objects (like JSON)

### Instructions:

1. Checkpoint: **Add a GET endpoint that returns a custom string**
 
- Add a new GET endpoint that listens at the path `/hello`.
- It should return the message `"Hello from /hello endpoint"`.
- After adding the endpoint, restart your app using `dotnet run` command.
- Open your browser and visit the route:
  `https://localhost:5001/hello` (Or the port shown in your terminal when you run the app.)


Hint: Use `app.MapGet("/hello", () => "your message")`;

2. Checkpoint: **Add a GET endpoint that returns the current time**

- Use `DateTime.UtcNow` to return the current UTC time when someone visits `/time`.
- Stop the app if it's already running by pressing `Ctrl + C` in the terminal.
- Run it again using `dotnet run`.
- Visit `https://localhost:5001/time` (or your actual port) in the browser.

Hint: You can directly return `DateTime.UtcNow` inside the lambda.

3. Checkpoint: **Add a GET endpoint that returns structured data such as a small JSON object**

- Add another GET endpoint at the route `/info`.
- Return an object like this: `{ name = "My API", version = "1.0" }`
- ASP.NET Core will automatically convert this to JSON format.

When you run the app and visit `/info`, the response will look like this:

```json
{
  "name": "My API",
  "version": "1.0"
}
```
This is the kind of format most APIs use when talking to frontend applications or other systems.

Hint: Use `new { name = "...", version = "..." }` to return an anonymous object.

## Exercise 3: Working with Route Parameters

### Narrative:

So far, your API returned the same fixed message every time — like `“Hello World”` or the current time. But most real-world APIs need to respond based on what the user asks for. For example, if someone wants to see a product by ID or view a user profile by name, the API must react to different inputs. That’s where route parameters are used.

A route parameter is a part of the URL that acts like a placeholder for values. For example:

```cs
app.MapGet("/greet/{name}", (string name) => $"Hello, {name}!");
```

Here, `{name}` is the route parameter. If you visit `/greet/Sam`, the `{name}` part captures `“Sam”` and passes it to the endpoint. The result will be `“Hello, Sam!”`

Route parameters can also be limited by constraints. A constraint ensures the value matches a specific type — like an integer. For example:

```cs
app.MapGet("/api/items/{id:int}", (int id) => $"Item ID is: {id}");
```

This only works if the id is a number. If you try a word like `/api/items/apple`, it won’t match and the browser will show a **404 Not Found** error. That’s helpful — it stops invalid data from reaching your code.

You can also use route parameters to return more useful responses. For example, return a message that includes a person’s name, or send back a custom JSON object.

By learning route parameters, you can start building smarter APIs that respond based on what users ask for.

### Instructions:

1. Checkpoint: Create a GET endpoint that accepts any ID from the URL
   
Add a new route to capture an `id` value directly from the URL using a route parameter. Use the captured value to return a message like: `"You requested item with ID: 123"`.

Hint: Use a pattern like `"/api/items/{id}"` and bind id as a string.

2. Checkpoint: Restrict a route to accept only integer IDs

Define another route that includes the `:int` constraint in the path.
- The endpoint should return a message like: `"Item ID as integer: 42"`.
- Try entering a word like `/hello` in place of the number — it should return a **404** page.
  
Hint: Add `:int` after `{id}` to only allow numbers.

3. Checkpoint: Return a JSON object using a username from the route
   
Create an endpoint that captures a username value and returns a simple object with a message like:
```json
{ "message": "Profile for user: alex" }
```
  
Hint: Return a result using `new { message = ... }`.

## Exercise 3: _Insert exercise title here._

### Narrative:

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but """"" recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

## Exercise 3: _Insert exercise title here._

### Narrative:

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but """"" recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

## Exercise 3: _Insert exercise title here._

### Narrative:

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but """"" recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._
