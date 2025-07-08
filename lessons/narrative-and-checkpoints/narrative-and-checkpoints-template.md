# Introduction to Minimal API

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

## Exercise 2: _Insert exercise title here._

### Narrative:

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

## Exercise 3: _Insert exercise title here._

### Narrative:

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but """"" recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._
