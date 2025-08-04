# Introduction to Minimal API

## Exercise 1: Getting Started with Minimal API

### Narrative:

Let’s say we want to build a small app — like a task tracker or expense logger — and we want to get started quickly. Normally, setting up an API means creating controllers, models, and configuration files. That can take time and feel like a lot when we’re just starting out.

Minimal APIs in ASP.NET Core make things easier. We can build a working API using just one file — usually `Program.cs`. This is great when we want to try something out or build a small app fast.

Why this helps:

- Beginner-friendly
- No complex setup
- Quick to start
- Ideal for small apps 

To create a project in the current folder, we run:
```bash
dotnet new web
```


Or, to create one in a new folder:
```bash
dotnet new web -o MyApiApp
```

The `-o` option sets the folder name. Both commands gives us the same structure, with `Program.cs` at its core. 

Here's what the default code in `Program.cs` looks like: 

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

In the above code:

- `CreateBuilder()`: Configures services
- `MapGet()`: Handles requests (we’ll explore this next)
- `Run()`: Starts the server
 
To run the app:

1.	Navigate to the folder (if needed):
   ```bash
  	cd MyApiApp
   ```
   
2.	Build: 
   ```bash
  	dotnet build
   ```
3.	Start the server:
   ```bash
  	dotnet run
   ```
   
The terminal will display a local URL such as `https://localhost:5074`.

<img width="719" height="256" alt="Image" src="https://github.com/user-attachments/assets/c2b0014c-900d-4b9b-8b4b-e447add63a4c" />

In most development environments, we would open this URL in a browser to view the API’s response. 

Here, however, a fixed address: `https://localhost:8000` is already set up for us.

This is all it takes to create a working API with a single response.


### Instructions:

1. Checkpoint: Open the terminal and run a command that creates a new Minimal API web app inside the current folder.

Hint: Use `dotnet new web` syntax to create your Minimal API project.
   
2. Checkpoint: Open the `Program.cs` file. Click the Run button to see what is printed to the screen. Observe the response in the browser.

Hint: You don’t have to change anything in the code editor. After pressing the Run button, you should be able to see the following output:

<img width="719" height="250" alt="Image" src="https://github.com/user-attachments/assets/d27b4c1a-c6c1-45e4-8537-8a627c6b827c" />


3. Checkpoint: In the `Program.cs` file, find the line that defines the `MapGet()` method. Change the response from "Hello World!" to a custom string of your choice, such as your name or a greeting.

Hint: Replace the string inside `() => "Hello World!"` with any other string, like `"Hi from Sarah!"`. 


## Exercise 2: Basic GET Endpoints 

### Narrative:
In the previous exercise, we created and ran our first ASP.NET Core Minimal API project. Now, let’s take a closer look at the default code inside `Program.cs`.

One line we noticed was:

```cs
app.MapGet("/", () => "Hello World!");
```

This defines a GET endpoint. But what does that actually mean?

An **endpoint** is like a stop or address in our API. When someone visits a specific URL, the server matches it to an endpoint and sends a response.

A **GET** request is the most common type of request — it’s what browsers use to fetch web pages.

So the line above means:

"*When someone visits the root URL (`/`), send back the text: 'Hello World!'.*"

The `MapGet()` method connects a route (like `/` or `/hello`) to a handler function. The handler is the part that generates the response.

Here’s another example:

```csharp
app.MapGet("/hello", () => "Hi there!");
```

In this case:

`/hello` is the path someone visits.

`() => "Hi there!"` &rarr; This is a *lambda expression*. A lambda is a quick, inline function that takes inputs (inside the `()`) and returns a result. In this case, it doesn't take any input, and just returns a string (`"Hi there!"`).

Minimal APIs can return different kinds of responses. Here are some examples:

```cs
app.MapGet("/text", () => "Just a string");                // Plain text

app.MapGet("/time", () => DateTime.Now);                   // Current time

app.MapGet("/json", () => new { Name = "Dotnet", Age = 8 }); // JSON object

```
Each endpoint returns a different type of data — plain text, a timestamp, or JSON. Let’s try creating all three.

### Instructions:

1. Checkpoint: Let’s start by adding a friendly message.

Create a new GET endpoint that listens at `/hello` and returns the text `"Hello from /hello endpoint"`. Once added, click the Run button and visit `https://localhost:8000/hello` to see the result in your browser.

Hint: Use the `app.MapGet()` method like this:
```cs
app.MapGet("/hello", () => /* your message here */);
```


2. Checkpoint: Next, return the current time by adding a new GET endpoint at `/time` that returns the current UTC time.

After running the app, go to `https://localhost:8000/time` and check the output.

Hint: Inside the lambda, return `DateTime.UtcNow` to get the correct format.


3. Checkpoint: Now let’s send back some structured information.

Create a GET endpoint at `/info` that returns an object with two properties — like `name` and `version`. ASP.NET Core will automatically convert it to JSON for us.

Then, visit `https://localhost:8000/info` to view the response in JSON format.

Hint: Return something like `new { name = "...", version = "..." }` inside the lambda.

**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/hello", () => "Hello from /hello endpoint");
app.MapGet("/time", () => DateTime.UtcNow);
app.MapGet("/info", () => new { name = "Minimal API", version = "1.0" });

app.Run();
```

## Exercise 3: Working with Route Parameters

### Narrative:

So far, our API returned the same fixed message every time — like `“Hello World”` or the current time. But the real websites or apps need to respond differently depending on what the user asks for. For example, if someone wants to see their profile or check a product, the app needs to know *which* user or product they mean.

That’s where **route parameters** come in. They’re like method parameters but used in the URL. They let us pass values through the web address so the API can respond with something specific.

Here’s a simple example:

```cs
app.MapGet("/greet/{name}", (string name) => $"Hello, {name}!");
```

Here, `{name}` is the route parameter. If we visit `/greet/Sam`, the `{name}` part captures `“Sam”` and passes it to the endpoint. The result will be `“Hello, Sam!”`

Route parameters can also be limited by constraints. A constraint ensures the value matches a specific type — like an integer. For example:

```cs
app.MapGet("/api/items/{id:int}", (int id) => $"Item ID is: {id}");
```

This only works if the `id` is a number such as `/api/items/42`. Visiting `/api/items/apple` returns a **404 Not Found** — which means the server couldn’t find a matching endpoint. This helps prevent invalid data from reaching our code.

By using route parameters, we can create APIs that return personalized messages or custom JSON based on user input.

Let’s create some endpoints that return different responses based on what’s passed in the URL.

### Instructions:

1. Checkpoint: Ceate a GET endpoint at `/color/{favorite_color}` that returns a message like: `"Your favorite color is blue!"` or `"Your favorite color is green!"`

Just replace `{favorite_color}` with a route parameter and use it in the response.

Hint: Use string `favorite_color` inside the lambda.
   
2. Checkpoint: Now let’s handle numbers using constraints.

Create a GET endpoint at `/order/{order_number:int}` that responds with: `"Order number 1234 is being processed."`

Try `/order/1234` in the browser. If we use a word like `/order/abc`, we should see a 404 error — meaning the route only accepts numbers.

Hint: Add `:int` after the parameter name in the URL pattern.

3. Checkpoint: Let’s try returning a full object.

Create a GET endpoint at `/book/{isbn}` that sends back a JSON object with:

- `isbn`: the value from the URL

- `title`: `"API Basics"`

- `available`: `true`

Try visiting `/book/9781234567890` and check the JSON response.

Hint: Use `new { isbn = isbn, title = "...", available = true }` inside the lambda.

**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/color/{favorite_color}", (string favorite_color) => $"Your favorite color is {favorite_color}!");

app.MapGet("/order/{order_number:int}", (int order_number) => $"Order number {order_number} is being processed.");

app.MapGet("/book/{isbn}", (string isbn) => new { isbn = isbn, title = "API Basics", available = true });

app.Run();
```

## Exercise 4: Creating a Simple POST Endpoint

### Narrative:

Up until now, we’ve only received data from our API — like messages or the current time. But what if we want to send data to the server? For example, imagine a travel website where users submit feedback after a trip. We want to build an endpoint where someone can send a message like “Great service!” and get a confirmation back.

To do this, we use the POST method. Unlike GET, which only fetches data, POST is used to send data to the server. Minimal APIs support this through the `MapPost()` method.

Here’s a simple example:

```cs
app.MapPost("/feedback", (string body) =>
{
    return Results.Created("/feedback", $"Feedback received: {body}");
});
```
Let’s break it down:

- The endpoint listens at `/feedback`.
- It accepts plain text from the request body (captured in `body`).
- It returns a **201 Created** response using `Results.Created()`.

The `Results.Created()` method tells the client something was successfully created. The first argument is the location ("/feedback"), and the second is a confirmation message. 

So when someone sends a message like `“Great service!”` to `/feedback`, the server replies with:

- **Status:** `201 Created` 
- **Body:** Feedback received: Great service!


To test this, we’ll use Swagger UI — a built-in tool that shows our API and lets us try out endpoints in the browser. We just need to enable it by adding these lines to `Program.cs`:

Before ` var app = builder.Build()`:

```cs
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

```

After `var app = builder.Build()`:**

```cs
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```

Once that’s done, running the app and visiting `https://localhost:8000/swagger` (or port specified on your local machine) will show the Swagger UI where we can test GET and POST endpoints directly in the browser.

It looks something like this:

<img width="628" height="371" alt="image" src="https://github.com/user-attachments/assets/c8667a57-30aa-46e8-b1c0-f236d41182f8" />

In this UI, we can see a list of all the endpoints defined in the application. To test any endpoint — for example, the `GET /` endpoint — we can click on it to expand details, then click **"Try it out"** and **"Execute"** to send a real request to the API.

<img width="551" height="715" alt="Picture3" src="https://github.com/user-attachments/assets/de0bd2ba-4a50-4a04-8cb0-3919d0ea1975" />


Similarly, to try out a POST endpoint like `/feedback`, click **Try it out**, enter a message, and hit **Execute**. The response appears just below — including the **Status Code** (`201 Created`) and the **Response body** showing your confirmation message.

<img width="597" height="746" alt="Picture2" src="https://github.com/user-attachments/assets/dff3fe41-3f4b-422a-b20b-4432f9e1c8fb" />


This makes it easy to confirm whether our POST endpoint behaves as expected — without needing a separate API client like Postman.


### Instructions:

1. Checkpoint: **Enable Swagger UI in the project**

Swagger is already installed in this environment. You need to:

- Add Swagger services before `var app = builder.Build()`, and Swagger middleware after it.
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

**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);

// Enable Swagger
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Use Swagger middleware
app.UseSwagger();
app.UseSwaggerUI();

app.MapGet("/", () => "Hello World!");

app.MapPost("/api/support", (string body) =>
{
    return Results.Created("/api/support", $"Support request received:{body}");
});

app.Run();
```

## Exercise 5: Accepting JSON Using Model Binding

### Narrative:

In the previous Exercise, you created a basic `POST` endpoint that accepted **plain text**. That was a great first step toward understanding how clients send data to a server.

But in real-world applications, data is usually sent in a structured format like **JSON**. Imagine building an e-commerce app where customers can add new products. Each product has a name and a price or ID. When a user submits a form, the data gets sent to your API — not as plain text, but as a structured format like JSON.

Instead of extracting fields manually, ASP.NET Core’s model binding allows us to convert the incoming JSON directly into a C# object. This makes your code cleaner and safer.

Let’s say the client sends this JSON:

```json
{
  "id": 1,
  "name": "Notebook",
  "price": 20.01
}
```

We’ll first create a class in C# that matches this structure:

```cs
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

Now, we define a POST endpoint that uses this class:

```cs
app.MapPost("/api/products", (Item new_item) =>
{
    return Results.Created($"/api/products/{new_item.Id}", new_item);
});
```

Here’s what’s happening:

- `MapPost()` defines the route `/api/products` for POST requests.
- The Item `new_item` parameter tells ASP.NET to expect JSON, and it binds the JSON data to the `Item` object.
- `Results.Created()` sends a **201 Created** response, along with the submitted object.

To test this, go to `https://localhost:8000/swagger` in your browser. Swagger UI will list the endpoint. Click **Try it out**, paste your JSON, and click **Execute**.

We’ll see: 

- **Status Code:** 201 Created
- **Response Body:** Your submitted data.

This is a clear sign that model binding worked.

### Instructions:

1. Checkpoint: Create a class named `MovieDetails` with properties `Title` (string) and `Year` (int). Place this class below the `app.Run()` line in your `Program.cs`.

Hint: Create a class just like the `Item` class shown earlier. Remember, class names start with uppercase and the property names must match the JSON field names.


2. Checkpoint: Define a new POST endpoint at `/api/movies` that accepts a `MovieDetails` object from the request body. This object should automatically bind from the incoming JSON. The endpoint should return the same object using a `201 Created` status code.

Hint: Use `app.MapPost("/api/movies", (MovieDetails movie) => …)` with the route `/api/movies`. Use `Results.Created()` to return the submitted data.

3. Checkpoint: Locate the POST `/api/movies` endpoint in Swagger UI and and click **Try it out**. Paste the following JSON into the request body:

```json
{
  "title": "Inception",
  "year": 2010
}
```
Hint: In Swagger, each endpoint is listed with its method and path. Look for the POST `/api/movies` entry. Click **Try it out**, paste the JSON into the body box, and click **Execute**.

You should see:

- **Status:** 201 Created
- **Response Body:**

```json
{
  "title": "Inception",
  "year": 2010
}
```


**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.MapPost("/api/movies", (MovieDetails new_movie) =>
{
    return Results.Created($"/api/movies/{new_movie.Year}", new_movie);
});

app.Run();

public class MovieDetails
{
    public string Title { get; set; }
    public int Year { get; set; }
}
```

## Exercise 6: Combining GET and POST — Resource Creation and Retrieval

### Narrative:

In the last exercises, we learned how to:
* Use GET to return fixed responses
* Use POST to receive plain text or JSON data

Now we will build a tiny product API that lets us:
* Add a new product using POST
* Fetch a product by ID using a route parameter

This simulates how real APIs work. For example:

* A shopping app lets you **create** a product (POST)
* Then lets others **retrieve** it (GET)

We will store the products in a simple **in-memory list**, which is like a temporary database that resets every time you restart the app.

First, we define a class to represent this data. After `app.Run()`, we add:

```cs
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

Next, we define and initialize a temporary in-memory list to store products right after `var app = builder.Build();`:

```cs
var products_list = new List<Item>
{
    new Item { Id = 1, Name = "Laptop", Price = 999.99m },
    new Item { Id = 2, Name = "Mouse", Price = 25.5m }
};
```

Now we create a POST endpoint to add a product:

```cs
app.MapPost("/api/products", (Item new_item) =>
{
    products_list.Add(new_item);
    return Results.Created($"/api/products/{new_item.Id}", new_item);
});
```

This endpoint:
* Accepts a JSON Product (`p`) from the client  
* Adds it to the list (`products_list`)
* Returns the product with a `201 Created` status

We also add a GET endpoint to retrieve a product by ID:

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

When we run the app and open Swagger, we’ll see the GET `/api/products` endpoint. Testing it with a JSON body like:

```json
{ "id": 1, "name": "Shoes", "price": 150.99 }
```

creates a product. Then, fetching `/api/products/1` returns that product. This covers both model binding from JSON (POST) and route binding using a path value (GET).

### Instructions:

1. Checkpoint: Create a class named `Book` below the `app.Run()` line in `Program.cs`. It should have three properties: `Id` (int), `Title` (string), and `Author` (string).

Hint: Follow the same format as the `Item` class shown earlier. Each property should use `{ get; set; }`.

2. Checkpoint: Create a new in-memory list to hold `Book` objects after `var app = builder.Build();`. This list will store the data submitted to the API.

Hint: It should be a `List<Book>` declared and initialized after `var app = builder.Build()`;.

3. Checkpoint: Add a new POST endpoint at `/api/books`. It should accept a `Book` object from the request body, add it to the books list, and return a `201 Created` response containing the added book.

Hint: Use `MapPost`, accept a `Book` object as input, and return `Results.Created(...)`.

4. Checkpoint: Add a GET endpoint at `/api/books/title/{title}`. It should take the `title` from the route, search the list of books, and return the first match using a case-insensitive check. If no book matches, return a `404 Not Found`.

Hint: Use `ToLower()` and `Contains()` inside a `foreach` loop for partial, case-insensitive comparison like this: 

```cs
if (b.Title != null && b.Title.ToLower().Contains(title.ToLower()))
```

5. Checkpoint: In Swagger UI, test the POST `/api/books` endpoint. Click **Try it out**, and submit this JSON:

```json
{
  "id": 1,
  "title": "The Hobbit",
  "author": "J.R.R. Tolkien"
}
```

Then test the GET `/api/books/title/{title}` endpoint by entering `The Hobbit` and clicking **Execute**. You should see the book details in the response.

**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();


var book_list = new List<Book>
{    
    new Book { Id = 1, Title = "The Pragmatic Programmer", Author = "Andrew Hunt" },
    new Book { Id = 2, Title = "Clean Code", Author = "Robert C. Martin" },
    new Book { Id = 3, Title = "Design Patterns", Author = "Erich Gamma" }
};

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.MapPost("/api/books", (Book b) =>
{
    book_list.Add(b);
    return Results.Created($"/api/books/{b.Id}", b);
});

app.MapGet("/api/books/title/{title}", (string title) =>
{
    foreach (var b in book_list)
    {
        if (b.Title != null && b.Title.ToLower().Contains(title.ToLower()))
        {
            return Results.Ok(b);
        }
    }
    return Results.NotFound();
});

app.Run();

public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
}
```

## Exercise 7: Updating and Deleting Resources

### Narrative:

In earlier exercises, we created and retrieved items using POST and GET. Now, let's complete the full CRUD cycle by learning how to update and delete items. This means we'll be able to update existing data and remove it when it's no longer needed.

To update a resource, we use the **PUT** method. In Minimal APIs, this is done with `MapPut()`. Just like `MapGet()` uses a route like `"/api/products/{id}"` to get a specific item, `MapPut()` uses a similar route to identify which item to update. The updated data is sent in the request body as JSON.

Let’s add a PUT endpoint to update an existing product. Place this below your existing endpoints:

```cs
app.MapPut("/api/products/{id}", (int id, Item updated) =>
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

* Reads the `{id}` from the route
* Searches for a product with that ID
* Replaces it with the new data from the request body
* Returns:
   - `200 OK` with the updated product if found
   - `404 Not Found` if no matching product exists

When we run the app and open Swagger, we’ll see the PUT `/api/products/{id}` endpoint. We can test it by entering an `id` (e.g., 2) and providing updated details like this:

```json
{
  "id": 2,
  "name": "Premium Pen",
  "price": 29.99
}
```

This updates the product with `id` 2 to reflect the new name and price.

To delete a resource, we use the DELETE method. In Minimal APIs, this is done using `MapDelete()`. Let’s add a DELETE endpoint to remove an existing product:

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
- Returns:
     - `204 No Content` if the deletion is successful
     - `404 Not Found` if the product doesn’t exist

In Swagger, we’ll see the DELETE `/api/products/{id}` endpoint. To test it:

- Enter the `id` of the product we want to delete (e.g., 2)
- Click **Execute**
- If the product exists, we’ll receive a `204 No Content` response
- If it doesn’t exist, we’ll get a `404 Not Found`

This completes the CRUD operations using Minimal APIs with route parameter binding.

### Instructions:

1. Checkpoint: Add a new GET endpoint at `/api/books` that returns all books in the existing `book_list`. This will help to retrieve everything added so far.

Hint: Use `app.MapGet("/api/books", () => book_list);` to return the complete list.

2. Checkpoint: Add a PUT endpoint at `/api/books/{id}`. This should update the book with the given ID using the data from the request body. If the book is found, update its values. If not, return `404 Not Found`.

Hint: Loop through `book_list`, match the `id`, and replace the book. 

3. Checkpoint: Add a DELETE endpoint at `/api/books/{id}`. It should search for a book with the given ID, remove it if it exists, and return `204 No Content`. If no match is found, return `404 Not Found`.

Hint: Use `FirstOrDefault()` to find the book, then call `book_list.Remove()` if it exists. 

4. Checkpoint: In Swagger UI,

- Add a book using POST `/api/books`:

```json
{
  "id": 1,
  "title": "Learning C#",
  "author": "Alice",
}
```

- Then, use GET `/api/books` to confirm it appears.

- Update the book using PUT `/api/books/1` with:

```json
{
  "id": 1,
  "title": "Mastering C#",
  "author": "Alice B.",

}
```
 and verify the changes.
 
- Finally, delete the book using DELETE `/api/books/1`.
  
- Try deleting again — you should see `404 Not Found`.

Hint: In Swagger UI, make sure to execute each endpoint in the right order. Use clear IDs and double-check JSON formatting.

**Solution:**

```cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();


var book_list = new List<Book>
{    
    new Book { Id = 1, Title = "The Pragmatic Programmer", Author = "Andrew Hunt" },
    new Book { Id = 2, Title = "Clean Code", Author = "Robert C. Martin" },
    new Book { Id = 3, Title = "Design Patterns", Author = "Erich Gamma" }
};

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}


app.MapPost("/api/books", (Book b) =>
{
    book_list.Add(b);
    return Results.Created($"/api/books/{b.Id}", b);
});

app.MapGet("/api/books", () => book_list);

app.MapPut("/api/books/{id}", (int id, Book updated_book) =>
{
    for (int i = 0; i < book_list.Count; i++)
    {
        if (book_list[i].Id == id)
        {
            book_list[i] = updated_book;
            return Results.Ok(updated_book);
        }
    }
    return Results.NotFound();
});

app.MapDelete("/api/books/{id}", (int id) =>
{
    var book_to_remove = book_list.FirstOrDefault(p => p.Id == id);
    if (book_to_remove is null)
    {
        return Results.NotFound();
    }

    book_list.Remove(book_to_remove);
    return Results.NoContent();
});

app.Run();

public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
}
```

## Exercise 8: Basic Data Validation

### Narrative:

In the last exercise, we completed full CRUD using:

- `POST`, `GET`, `PUT`, and `DELETE`
- Route parameters, query strings, and JSON body binding

So far, our API accepts any data — even invalid or empty values.

Now, we’ll learn how to protect our API by:

- Rejecting bad input (e.g., empty names, negative prices)
- Returning clear error messages and `400 Bad Request` responses
- Writing simple, automatic validations using attributes in the model

This builds real-world habits. For example:

- A shopping cart shouldn’t allow products with no name or price below 0
- Users should see helpful error messages when they send bad input

In your existing `Item` class, add **Data Annotations** to enforce rules.

Go to the bottom of your `Program.cs` and update the `Product` class like this:

```cs
public class Item
{
    public int Id { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 3)]
    public string Name { get; set; }

    [Range(0.01, 100000)]
    public decimal Price { get; set; }
}
```

### Instructions:

1. Checkpoint: 
Hint:
   
2. Checkpoint: 

Hint: 



3. Checkpoint: 

Hint: 


## Exercise 9: API Documentation with OpenAPI

### Narrative:



### Instructions:

1. Checkpoint: 
Hint:
   
2. Checkpoint: 

Hint: 



3. Checkpoint: 

Hint: 

## Exercise 10: Summary

### Narrative:



### Instructions:

1. Checkpoint: 
Hint:
   
2. Checkpoint: 

Hint: 



3. Checkpoint: 

Hint: 
