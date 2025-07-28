# Introduction to Minimal API

## Exercise 1: Getting Started with Minimal API

### Narrative:

Let’s say we want to build a small app — like a task tracker or expense logger — and get started quickly, without setting up controllers, models, or configuration files. This is where **Minimal APIs** in ASP.NET Core come in.
Minimal APIs offer a fast, lightweight way to build HTTP APIs using just a single file —typically `Program.cs`. Unlike traditional ASP.NET Core apps, they require less setup and are great for simple, focused applications.

**Why use Minimal APIs?**
- Beginner-friendly
- Faster startup
- Ideal for small apps or microservices

**How to Create:**

To create a project in the current folder, we use
```cs
dotnet new web
 ```
Or, to create one in a new folder, we can use:
```cs
dotnet new web -o MyApiApp
 ```
The `-o` option specifies the output folder name. Both approaches produce the same structure, with `Program.cs` at its core. We’ll use the current folder method for simplicity.

**What’s inside `Program.cs`:**

This file drives our API. Here's what the default code looks like: 

![template code](https://github.com/user-attachments/assets/fe5594d9-c658-4c12-8448-f47584fa73c3)

In the above code:

- `CreateBuilder()`: Configures services
- `MapGet()`: Handles requests (we’ll explore this next)
- `Run()`: Starts the server
 
**How to Run:**

1.	Navigate to the folder (if needed):`cd MyApiApp`
   
2.	Build: `dotnet build`
   
3.	Start the server: `dotnet run`
   
The terminal will display a local URL such as `https://localhost:5001`.

<img width="719" height="256" alt="Image" src="https://github.com/user-attachments/assets/c2b0014c-900d-4b9b-8b4b-e447add63a4c" />

In most development environments, we would open this URL in a browser to view the API’s response. 

Here, however, a fixed address: `https://localhost:8000` is already set up for us, as shown in the workspace.

This is all it takes to create a working API with a single response.


### Instructions:

1. Checkpoint: Open the terminal and run a command that creates a new Minimal API web app inside the current folder.

Hint: Use `dotnet new web` syntax to create your Minimal API project.
   
2. Checkpoint: Open the `Program.cs` file. Click the **Run** button to see what is printed to the screen. Observe the response in the browser.

Hint: You don’t have to change anything in the code editor. After pressing Run button, you should be able to see the following output:

<img width="719" height="250" alt="Image" src="https://github.com/user-attachments/assets/d27b4c1a-c6c1-45e4-8537-8a627c6b827c" />


3. Checkpoint: In the `Program.cs` file, find the line that defines the `MapGet()` method. Change the response from "Hello World!" to a custom string of your choice, such as your name or a greeting.

Hint: Replace the string inside `() => "Hello World!"` with any other string, like `"Hi from Sarah!"`. 


## Exercise 2: Basic GET Endpoints 

### Narrative:
In the previous exercise, we created and ran your first ASP.NET Core Minimal API project. Now, let’s take a closer look at the default code inside `Program.cs`.

One line we had noticed is:

```cs
app.MapGet("/", () => "Hello World!");
```

This defines a GET endpoint. But what does that actually mean?

**What is a GET endpoint?**

An **endpoint** is like a stop or address in our API. When someone visits a specific URL, the server matches it to an endpoint and sends a response.

A **GET** request is the most common kind of request used to retrieve data. When your browser visits a website, it's making GET requests behind the scenes.

So this line:

```csharp
app.MapGet("/", () => "Hello World!");
```

means:

"*When someone visits the root URL (i.e., /), send back the text: 'Hello World!'.*"

**What does `MapGet()` do?**

The `MapGet()` method connects a route (like `/` or `/hello`) to a handler function. The handler is the part that generates the response.

Let’s break this down using an example:

```csharp
app.MapGet("/hello", () => "Hi there!");
```

`/hello` &rarr; This is the path the user will visit.

`() => "Hi there!"` &rarr; This is a lambda expression (a short function) that returns the message.

When someone visits `https://localhost:8000/hello`, they will see `"Hi there!"`.

Minimal APIs support various response types, such as:

- Plain text (string)
- Current time (`DateTime`)
- Structured data (JSON objects)

Let’s try creating all three.

### Instructions:

1. Checkpoint: **Add a GET endpoint that returns a custom string**
 
- Add a new GET endpoint that listens at `/hello`.
- It should return the message `"Hello from /hello endpoint"`.
- Visit `https://localhost:8000/hello` in the browser to view the response.


Hint: Use `app.MapGet("/hello", () => /* your message here */)`;

2. Checkpoint: **Add a GET endpoint that returns the current time**

- Add a new GET endpoint at `/time`.
- It should return the current UTC time.
- Visit `https://localhost:8000/time` in the browser to view the response.


Hint: Return `DateTime.UtcNow` inside the lambda expression.

3. Checkpoint: **Add a GET endpoint that returns structured data such as a small JSON object**

- Add a new GET endpoint at `/info`.
- Return an object with properties like `name` and `version`. The API will automatically convert it to JSON.
- Visit `https://localhost:8000/info` in the browser to view the JSON response.

Hint: Return an anonymous object like `new { name = "...", version = "..." }`.

## Exercise 3: Working with Route Parameters

### Narrative:

So far, our API returned the same fixed message every time — like `“Hello World”` or the current time. But think about a real website or app. It needs to do more than just say the same thing all the time. For example, imagine a website where users can look up information about different people or products.

If someone wants to see their profile page, the website needs to know which user to show. Or if they want to see details about a specific product, the app must know which product they mean. This means the API has to understand and respond differently depending on what the user asks for. That’s where **route parameters** are used.

A route parameter is a part of the web address (URL) that acts like a placeholder for values. For example:

```cs
app.MapGet("/greet/{name}", (string name) => $"Hello, {name}!");
```

Here, `{name}` is the route parameter. If you visit `/greet/Sam`, the `{name}` part captures `“Sam”` and passes it to the endpoint. The result will be `“Hello, Sam!”`

Route parameters can also be limited by constraints. A constraint ensures the value matches a specific type — like an integer. For example:

```cs
app.MapGet("/api/items/{id:int}", (int id) => $"Item ID is: {id}");
```

This only works if the `id` is a number such as `/api/items/42`. Visiting `/api/items/apple` won’t work — the server returns a **404 Not Found**. This helps prevent invalid data from reaching our code.

By using route parameters, we can create APIs that return personalized messages or custom JSON based on user input.

Learning route parameters is an important step toward building smarter, more flexible APIs that respond meaningfully to different requests.

### Instructions:

1. Checkpoint: **Favorite Color Message**
   
- Create a GET endpoint at `/color/{favoriteColor}` that returns a message like: `"Your favorite color is {favoriteColor}!"`
- Test it by visiting `/color/blue` or `/color/green`.

Hint: Use a string route parameter to capture the color and return it in the response.

2. Checkpoint: **Order Status Checker**

- Create a GET endpoint at `/order/{orderNumber:int}` that returns a message like: `"Order number {orderNumber} is being processed."`
- Test by visiting `/order/1234`. Non-numeric order numbers should return a 404 error.

Hint: Add `:int` after `{orderNumber}` to only allow numbers.

3. Checkpoint: **Book Details JSON**

Create a GET endpoint at `/book/{isbn}` that returns a JSON object with:

- `isbn`: the ISBN string from the route
- `title`: a fixed title, for example "API Basics"
- `available`: true

Test by visiting `/book/9781234567890` and check the JSON response.
  
Hint: Return an anonymous object using syntax like: 

`new { isbn = ..., title = ..., available = ... }`

## Exercise 4: Creating a Simple POST Endpoint


### Narrative:

Imagine a travel website where users can submit feedback after a trip. We want to build an endpoint where users can send a message like “Great service!” and responds with a confirmation.

To handle this kind of data submission, we use the **POST** method — which is designed to send data to the server, unlike GET which only retrieves it. Minimal APIs support this through the `MapPost()` method.

You can define a POST endpoint using `MapPost()` like this:

```cs
app.MapPost("/feedback", (string body) =>
{
    return Results.Created("/feedback", $"Feedback received: {body}");
});
```
Let’s break it down:

- The endpoint listens at `/feedback`.
- It accepts plain text from the request body (captured in body).
- It returns a **201 Created** response using `Results.Created()` — which indicates success and includes a message back to the user.
  
To test this, we’ll use Swagger UI, a built-in tool that that displays our API and lets us interact with endpoints.

Since the dotnet new web template doesn’t come with Swagger enabled, we need to configure it manually:
 
**Step 1. Install Swagger using the terminal:**

```bash
dotnet add package Swashbuckle.AspNetCore
```

**2. Enable Swagger services before `Build()`:**

```cs
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
```

**3. Add Swagger middleware after `Build()`:**

```cs
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```

Once this setup is complete, running the app and visiting `https://localhost:8000/swagger` (or port specified on your local machine) will show the Swagger UI where we can test GET and POST endpoints directly in the browser.

It looks something like this:

<img width="628" height="371" alt="image" src="https://github.com/user-attachments/assets/b5c93048-f6e9-4cf6-a56e-10b61d115117" />


In this UI, we can see a list of all the endpoints defined in the application. To test any endpoint — for example, the `GET /` endpoint — we can click on it to expand details, then click **"Try it out"** and **"Execute"** to send a real request to the API.

<img width="609" height="785" alt="image" src="https://github.com/user-attachments/assets/3fcfd7dd-ab87-4867-8cd1-864b8d7e8031" />


Similarly, to try out a POST endpoint like `/feedback`, click **Try it out**, enter a message, and hit **Execute**. The response appears just below — including the **Status Code** (`201 Created`) and the **Response body** showing your confirmation message.

<img width="597" height="746" alt="image" src="https://github.com/user-attachments/assets/70b93055-d38f-4676-bdf2-8fbfef70193c" />


This makes it easy to confirm whether our POST endpoint behaves as expected — without needing a separate API client like Postman.


### Instructions:

1. Checkpoint: **Enable Swagger UI in the project**

Swagger is already installed in this environment. You need to:

- Add Swagger services before `Build()`, and Swagger middleware after it.
- Click the **Run** button and open `https://localhost:8000/swagger` in the browser.

For now, you can only see the default GET endpoint.

Hint: Use `AddEndpointsApiExplorer()` and `AddSwaggerGen()` before `Build()`; add `UseSwagger()` and `UseSwaggerUI()` after `Build()`. 

2. Checkpoint: **Create a POST endpoint for support tickets**

- Define a new POST route at `/api/support` that accept plain text input from the request body.
- Return a message such as:
  
`"Support request received: <your message>"`
  
Hint:  Use `MapPost()` with a string parameter and return `Results.Created()`.

3. Checkpoint: **Test the `/api/support` POST endpoint using Swagger**

- Use Swagger UI to send a test message to your `/api/support` POST endpoint.
- Confirm that the response returns `201 Created` with your input message.

Hint: In Swagger UI, click **Try it out** next to `/api/support`, enter a sample message like `"My app keeps crashing on login."`, and hit **Execute** to see the response.


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
