# Introduction to Minimal API

## Exercise 1: Getting Started with Minimal API

1. Checkpoint: Open the terminal and run a command that creates a new Minimal API web app inside the current folder.

Hint: Use `dotnet new web` syntax to create your Minimal API project.

```js
var validCommands = ['dotnet new web', 'dotnet new web -o '];
if (Components.Terminal.didRunOneOf(validCommands, 0)) {
    return {pass: true};
}
var combinedCommands = [validCommands.slice(0, -1).join(', '), validCommands.slice(-1)[0]].join(validCommands.length < 2 ? '' : ' or ');
return {
  pass: false,
  errors: {
    friendly: 'Did you run ' + combinedCommands + ' in the terminal?'
  }
};
```

```js
if (!Components.Terminal.didRunCommand(/dotnet new web/, 0)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you run `dotnet new web` command in the terminal?'
    }
  };
}

return { pass: true };
```
   
2. Checkpoint: Open the `Program.cs` file. Click the Run button to see what is printed to the screen. Observe the response in the browser.




3. Checkpoint: In the `Program.cs` file, find the line that defines the `MapGet()` method. Change the response from "Hello World!" to a custom string of your choice, such as your name or a greeting.

Hint: Replace the string inside `() => "Hello World!"` with any other string, like `"Hi from Sarah!"`. 

```js
if (Components.CodeEditor.codeContains('Program.cs', /Hello World!/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you update the `/` endpoint to return a custom message instead of "Hello World!"?',
      component: 'PersistentCodeEditor'
    }
  };
}
return { pass: true };
```
## Exercise 2: Basic GET Endpoints 
### Instructions:

1. Checkpoint: Let’s start by adding a friendly message.

Create a new GET endpoint that listens at `/hello` and returns the text `"Hello from /hello endpoint"`. Once added, click the Run button and visit `https://localhost:8000/hello` to see the result in your browser.

Hint: Use the `app.MapGet()` method like this:
```cs
app.MapGet("/hello", () => /* your message here */);
```

**test code to check hello world**
```js
if (!Components.CodeEditor.codeContains('Program.cs', /Console.WriteLine(.*);/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Make sure to use the format: `Console.WriteLine("name");`.',
      component: 'PersistentCodeEditor'
    }
  };
}
if (Components.OutputTerminal.hasOutput(/Hello World!/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Change "Hello World!" to something else.',
      component: 'PersistentCodeEditor'
    }
  };
}
if (Components.OutputTerminal.hasOutput(/error/)) {
  return {
    pass: false,
    errors: {
      friendly: 'There might be an error in your code! Use the hint if you\'re stuck.',
      component: 'PersistentCodeEditor'
    }
  };
}
return { pass: true };
```

2. Checkpoint: Next, return the current time by adding a new GET endpoint at `/time` that returns the current UTC time.

After running the app, go to `https://localhost:8000/time` and check the output.

Hint: Inside the lambda, return `DateTime.UtcNow` to get the correct format.

```js
if (!Components.CodeEditor.codeContains('Program.cs',/app\.MapGet\("\/time",\s*\(\)\s*=>\s*DateTime\.UtcNow\);/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you add an endpoint at `/time` that returns the current UTC time in Program.cs?'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Now let’s send back some structured information.

Create a GET endpoint at `/info` that returns an object with two properties — like `name` and `version`. ASP.NET Core will automatically convert it to JSON for us.

Then, visit `https://localhost:8000/info` to view the response in JSON format.

Hint: Return something like `new { name = "...", version = "..." }` inside the lambda.

```js
if (!Components.CodeEditor.codeContains('Program.cs', /app\.MapGet\("\/info",\s*\(\)\s*=>\s*new\s*\{\s*name\s*=\s*.*,\s*version\s*=\s*.*\s*\}\s*\);/
)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you add an `/info` endpoint that returns an object with `name` and `version` properties?'
    }
  };
}

return { pass: true };
```

4. Checkpoint: Finally, test all three endpoints using Swagger.

We have already enabled Swagger for you. Just open `https://localhost:8000/swagger` in the browser, click **Try it out** next to each endpoint, and execute the request to see the response.
  
```js
var validUrls = ['https://localhost:8000/swagger', 'https://localhost:8000/swagger/index.html'];
if (Components.WebBrowser.loadedOneOf(validUrls)) {
    return {pass: true};
}
var combinedUrls = [validUrls.slice(0, -1).join(', '), validUrls.slice(-1)[0]].join(validUrls.length < 2 ? '' : ' or ');
return {
  pass: false,
  errors: {
    friendly: 'Did you visit ' + combinedUrls + ' in the browser?'
  }
};
```

## Exercise 3: Working with Route Parameters
### Instructions:

1. Checkpoint: Checkpoint: Create a GET endpoint at `/color/{favorite_color}` that returns a message like: `Your favorite color is <FAVORITE_COLOR>!`
   
Replace `<FAVORITE_COLOR>` with the value from the route parameter. After adding it, open the Swagger UI at `/swagger` and try entering different colors like blue, red, or yellow in the input field.

Hint: Use string `favorite_color` inside the lambda.

```js
if (!Components.CodeEditor.codeContains('Program.cs', /app\.MapGet\(\s*["']\/color\/\{favorite_color\}["']\s*,\s*\(\s*string\s+favorite_color\s*\)\s*=>\s*.*\);/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you create an endpoint `/color/{favorite_color}` that takes a color as a route parameter and returns a message?'
    }
  };
}

return { pass: true };
```
   
2. Checkpoint: Now let’s handle numbers using constraints.

Create a GET endpoint at `/order/{order_number}` that responds with: `Order number <ORDER_NUMBER> is being processed.`

Replace `<ORDER_NUMBER>` with the value from the route parameter. Make sure the endpoint only accepts integer values. If you try to visit `/order/abc` or any non-numeric value, the app should return a 404 error.

Try visiting `/order/1234` to see the successful response.

Hint: Add `:int` after the parameter name in the URL pattern.

```js
if (!Components.CodeEditor.codeContains('Program.cs', /app\.MapGet\(\s*["']\/order\/\{order_number:int\}["']\s*,\s*\(\s*int\s+order_number\s*\)\s*=>\s*.*\);/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you create an endpoint `/order/{order_number:int}` that takes an integer order number and returns a message?'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Let’s try returning a full object.

Create a GET endpoint at `/book/{isbn}` that sends back a JSON object with:

- `isbn`: the value from the URL

- `title`: `"API Basics"`

- `available`: `true`

Try visiting `/book/9781234567890` and check the JSON response.

Hint: Use `new { isbn = isbn, title = "...", available = true }` inside the lambda.

```js
if (!Components.CodeEditor.codeContains('Program.cs',/app\.MapGet\(\s*["']\/book\/\{isbn\}["']\s*,\s*\(\s*string\s+isbn\s*\)\s*=>\s*new\s*\{\s*isbn\s*=\s*isbn\s*,\s*title\s*=\s*.*\s*,\s*available\s*=\s*.*\s*\}\s*\);/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you create an endpoint `/book/{isbn}` that accepts an ISBN and returns an object with `isbn`, `title`, and `available` properties?'
    }
  };
}

return { pass: true };
```

4. Checkpoint: You can also test all three endpoints using Swagger.
   
Open `https://localhost:8000/swagger` in the browser, click **Try it out** next to each endpoint, and hit Execute to see the response.

```js
var validUrls = ['https://localhost:8000/swagger', 'https://localhost:8000/swagger/index.html'];
if (Components.WebBrowser.loadedOneOf(validUrls)) {
    return {pass: true};
}
var combinedUrls = [validUrls.slice(0, -1).join(', '), validUrls.slice(-1)[0]].join(validUrls.length < 2 ? '' : ' or ');
return {
  pass: false,
  errors: {
    friendly: 'Did you visit ' + combinedUrls + ' in the browser?'
  }
};
```

## Exercise 4: Creating a Simple POST Endpoint

### Instructions:

1. Checkpoint: Define a new POST route at `/api/support` that accept plain text input from the request body.

The response should be a message like: `Support request received: <YOUR_MESSAGE>`. Here, `<YOUR_MESSAGE>` is the text sent in the request body.
  
Hint:  Use `MapPost()` with a string parameter and return `Results.Created()`.

```js
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\(\s*["']\/api\/support["']\s*,\s*\(\s*string\s+body\s*\)\s*=>?\s*\{\s*return\s+Results\.Created\(\s*["']\/api\/support["']\s*,\s*.*body.*\)\s*;\s*\}\s*\);/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a POST endpoint at `/api/support` that accepts a string body and returns a `Results.Created` response containing that body?'
    }
  };
}

return { pass: true };
```

2. Checkpoint: Use Swagger UI to send a test message to your `/api/support` POST endpoint. Confirm that the response returns `201 Created` with your input message.

Hint: In Swagger UI, click "Try it out" next to `/api/support`, enter a sample message like `My app keeps crashing on login.`, and hit "Execute" to see the response.

```js
var validUrls = ['https://localhost:8000/swagger', 'https://localhost:8000/swagger/index.html'];
if (Components.WebBrowser.loadedOneOf(validUrls)) {
    return {pass: true};
}
var combinedUrls = [validUrls.slice(0, -1).join(', '), validUrls.slice(-1)[0]].join(validUrls.length < 2 ? '' : ' or ');
return {
  pass: false,
  errors: {
    friendly: 'Did you visit ' + combinedUrls + ' in the browser?'
  }
};
```


## Exercise 5: Accepting JSON Using Model Binding

## Instructions:

1. Checkpoint: Create a class named `MovieDetails` with properties `Title` (string) and `Year` (int). Place this class below the `app.Run()` line in your `Program.cs`.

Hint: Create a class just like the `Item` class shown earlier. Remember, class names start with uppercase and the property names must match the JSON field names.

```js
// Check if the MovieDetails class is created
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+class\s+MovieDetails\s*\{[\s\S]*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `MovieDetails` class with the required properties?'
    }
  };
}

// Check if the Title property exists (type must be string)
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+string\s+Title\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does your `MovieDetails` class have a `Title` property of type `string`?'
    }
  };
}

// Check if the Year property exists (type must be int)
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+int\s+Year\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does your `MovieDetails` class have a `Year` property of type `int`?'
    }
  };
}

// Ensure the class is defined after app.Run();
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.Run\(\)[\s\S]*public\s+class\s+MovieDetails/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Make sure the `MovieDetails` class is placed **after** the `app.Run();` statement.'
    }
  };
}

return { pass: true };
```

2. Checkpoint: Define a new POST endpoint at `/api/movies` that accepts a `MovieDetails` object from the request body. The endpoint should:
   
- Automatically bind the incoming JSON to `new_movie` and return the same object
- Respond with a 201 Created status including the URI with `new_movie.Year` as the location

Hint: Use `app.MapPost("/api/movies", (MovieDetails movie) => …)` with the route `/api/movies`. Use `Results.Created()` to return the submitted data.

```js
// Check if the MapPost endpoint exists with correct route and MovieDetails parameter
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\(\s*["']\/api\/movies["']\s*,\s*\(\s*MovieDetails\s+\w+\s*\)\s*=>\s*\{[\s\S]*?\}\s*\);/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you add a `MapPost` endpoint for `/api/movies` that accepts a `MovieDetails` parameter and returns a created result?'
    }
  };
}

// Check if Results.Created is used with new_movie
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /Results\.Created\s*\(\s*\$?["']\/api\/movies\/\{?\w+\.Year\}?["']\s*,\s*\w+\s*\)/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Are you using `Results.Created` with a path including the movie year and returning the movie object?'
    }
  };
}

// Ensure MapPost is defined before app.Run();
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost[\s\S]*app\.Run\(\);/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Make sure the `app.MapPost()` endpoint is defined **before** the `app.Run();` statement.'
    }
  };
}

return { pass: true };

```

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

## Exercise 6: Combining GET and POST — Resource Creation and Retrieval

### Instructions:

1. Checkpoint: Create a class named `Book` below the `app.Run()` line in `Program.cs`. It should have three properties: `Id` (int), `Title` (string), and `Pages` (int).

Hint: Follow the same format as the `Item` class shown earlier. Each property should use `{ get; set; }`.

```js
// Check if the Book class is created
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+class\s+Book\s*\{[\s\S]*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `Book` class with the required properties?'
    }
  };
}

// Check if the Id property exists (type must be int)
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+int\s+Id\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does your `Book` class have an `Id` property of type `int`?'
    }
  };
}

// Check if the Title property exists (type must be string)
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+string\s+Title\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does your `Book` class have a `Title` property of type `string`?'
    }
  };
}

// Check if the Pages property exists (type must be int)
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+int\s+Pages\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does your `Book` class have a `Pages` property of type `int`?'
    }
  };
}

// Ensure the class is defined after app.Run();
if (!Components.CodeEditor.codeContains('Program.cs', /app\.Run\(\);\s*[\s\S]*?public\s+class\s+Book/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Make sure the Book class is placed **after** the app.Run(); statement.'
    }
  };
}


return { pass: true };
```


2. Checkpoint: Create a new in-memory list to hold `Book` objects after `var app = builder.Build();`. This list will store the data submitted to the API.

Hint: It should be a `List<Book>` declared and initialized after `var app = builder.Build()`;.

```js
// Check if book_list is declared and contains at least one Book
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /var\s+book_list\s*=\s*new\s+List<Book>\s*\{\s*new\s+Book\s*\{[\s\S]*?\}\s*(?:,\s*new\s+Book\s*\{[\s\S]*?\}\s*)*\};/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create the `book_list` with at least **one** `Book` initialized?'
    }
  };
}

// Ensure book_list is placed **after** builder.Build() and **before** app.Run()
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /var\s+app\s*=\s*builder\.Build\(\);\s*[\s\S]*?var\s+book_list\s*=\s*new\s+List<Book>[\s\S]*?app\.Run\(\);/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Make sure the `book_list` is placed **after** `var app = builder.Build();` and **before** `app.Run();`.'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Add a new POST endpoint at `/api/books`. It should accept a `Book` object from the request body, add it to the books list, and return a `201 Created` response containing the added book.

Hint: Use `MapPost`, accept a `Book` object as input, and return `Results.Created(...)`.

```js
// Check if POST endpoint for /api/books exists, accepts Book, adds to list, and returns Results.Created
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*"\/api\/books"\s*,\s*\(\s*Book\s+\w+\s*\)\s*=>\s*\{\s*book_list\.Add\(\w+\);\s*return\s+Results\.Created\(\s*\$"\/api\/books\/\{\w+\.Id\}"\s*,\s*\w+\s*\);\s*\}\s*\)/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `POST` endpoint at `/api/books` that accepts a `Book`, adds it to `book_list`, and returns `Results.Created` with the book details?'
    }
  };
}

return { pass: true };
```

4. Checkpoint: Add a GET endpoint at `/api/books/title/{title}`. It should take the `title` from the route, search the list of books, and return the first match using a case-insensitive check. If no book matches, return a `404 Not Found`.

Hint: Use `ToLower()` and `Contains()` inside a `foreach` loop for partial, case-insensitive comparison like this: 

```cs
if (b.Title != null && b.Title.ToLower().Contains(title.ToLower()))
```

```js
// Check if GET endpoint for /api/books/title/{title} exists, iterates over book_list,
// matches title using ToLower + Contains, and returns Results.Ok or Results.NotFound
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapGet\s*\(\s*"\/api\/books\/title\/\{title\}"\s*,\s*\(\s*string\s+\w+\s*\)\s*=>\s*\{\s*foreach\s*\(\s*var\s+\w+\s+in\s+book_list\s*\)\s*\{\s*if\s*\(\s*\w+\.Title\s*!=\s*null\s*&&\s*\w+\.Title\.ToLower\(\)\.Contains\(\w+\.ToLower\(\)\)\s*\)\s*\{\s*return\s+Results\.Ok\(\w+\);\s*\}\s*\}\s*return\s+Results\.NotFound\(\);\s*\}\s*\)/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `GET` endpoint at `/api/books/title/{title}` that searches `book_list` using `ToLower().Contains()` and returns `Results.Ok` or `Results.NotFound`?'
    }
  };
}

return { pass: true };
```

5. Checkpoint: In Swagger UI, test the POST `/api/books` endpoint. Click **Try it out**, and submit this JSON:

```json
{
  "id": 1,
  "title": "The Hobbit",
  "Pages": 480
}
```

Then test the GET `/api/books/title/{title}` endpoint by entering `The Hobbit` and clicking **Execute**. You should see the book details in the response.

## Exercise 7: Updating and Deleting Resources

### Instructions:

1. Checkpoint: Add a new GET endpoint at `/api/books` that returns all books in the existing `book_list`. This will help to retrieve everything added so far.

Hint: Use `app.MapGet("/api/books", () => book_list);` to return the complete list.

```js
// Ensure the GET endpoint for /api/books is defined after book_list initialization and before app.Run()
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /book_list\s*=\s*new\s+List<Book>[\s\S]*?app\.MapGet\s*\(\s*"\/api\/books"\s*,\s*\(\s*\)\s*=>\s*book_list\s*\)[\s\S]*?app\.Run\s*\(\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `GET` endpoint at `/api/books` **after** `book_list` initialization and **before** the `app.Run();` statement? Ensure it directly returns the `book_list`.'
    }
  };
}

return { pass: true };
```

2. Checkpoint: Add a PUT endpoint at `/api/books/{id}`. This should update the book with the given ID using the data from the request body. If the book is found, update its values. If not, return `404 Not Found`.

Hint: Loop through `book_list`, match the `id`, and replace the book. 

```js
// Ensure the PUT endpoint is defined after book_list initialization and before app.Run()
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /book_list\s*=\s*new\s+List<Book>[\s\S]*?app\.MapPut\s*\(\s*"\/api\/books\/\{id\}"\s*,\s*\(\s*int\s+id\s*,\s*Book\s+\w+\s*\)\s*=>\s*\{[\s\S]*?for\s*\(\s*int\s+i\s*=\s*0\s*;\s*i\s*<\s*book_list\.Count\s*;\s*i\+\+\s*\)\s*\{[\s\S]*?if\s*\(\s*book_list\[i\]\.Id\s*==\s*id\s*\)[\s\S]*?book_list\[i\]\s*=\s*\w+\s*;[\s\S]*?return\s+Results\.Ok\s*\(\s*\w+\s*\)\s*;[\s\S]*?\}[\s\S]*?return\s+Results\.NotFound\s*\(\s*\)\s*;[\s\S]*?\}\s*\)[\s\S]*?app\.Run\s*\(\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `PUT` endpoint at `/api/books/{id}` **after** `book_list` initialization and **before** the `app.Run();` statement? Ensure it updates the matching book and returns the updated object or NotFound().'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Add a DELETE endpoint at `/api/books/{id}`. It should search for a book with the given ID, remove it if it exists, and return `204 No Content`. If no match is found, return `404 Not Found`.

Hint: Use `FirstOrDefault()` to find the book, then call `book_list.Remove()` if it exists. 

```js
// Ensure the DELETE endpoint for /api/books/{id} is defined after book_list initialization and before app.Run()
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /book_list\s*=\s*new\s+List<Book>[\s\S]*?app\.MapDelete\s*\(\s*"\/api\/books\/\{id\}"\s*,\s*\(\s*int\s+\w+\s*\)\s*=>\s*\{[\s\S]*?var\s+\w+\s*=\s*book_list\.FirstOrDefault\s*\(\s*p\s*=>\s*p\.Id\s*==\s*\w+\s*\);[\s\S]*?if\s*\(\s*\w+\s+is\s+null\s*\)[\s\S]*?return\s+Results\.NotFound\s*\(\s*\)\s*;[\s\S]*?book_list\.Remove\s*\(\s*\w+\s*\)\s*;[\s\S]*?return\s+Results\.NoContent\s*\(\s*\)\s*;[\s\S]*?\}[\s\S]*?app\.Run\s*\(\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Did you create a `DELETE` endpoint at `/api/books/{id}` **after** `book_list` initialization and **before** the `app.Run();` statement? Ensure it removes the correct book or returns NotFound, and then returns NoContent.'
    }
  };
}

return { pass: true };
```

4. Checkpoint: In Swagger UI,

- Add a book using POST `/api/books`:

```json
{
  "id": 1,
  "title": "Learning C#",
  "pages": 200,
}
```

- Then, use GET `/api/books` to confirm it appears.

- Update the book using PUT `/api/books/1` with:

```json
{
  "id": 1,
  "title": "Mastering C#",
  "pages": 1000,

}
```
 and verify the changes.
 
- Finally, delete the book using DELETE `/api/books/1`.
  
- Try deleting again — you should see `404 Not Found`.

Hint: In Swagger UI, make sure to execute each endpoint in the right order. Use clear IDs and double-check JSON formatting.

## Exercise 8: Basic Data Validation

### Instructions:

1. Checkpoint: We already have a `Book` class and a POST `/api/books` endpoint that accepts new books. Add validation rules to the model so the API only accepts valid data.

Make sure both `Id` and `Title` are marked as required fields. The `Title` must be between `3` and `100` characters, and the `Pages` value should be within a reasonable range.

Hint: Use `[Required]` on both `Id` and `Title`, `[StringLength(100, MinimumLength = 3)]` to limit title length, and `[Range(1, 2000)]` to ensure pages are within bounds.

```js
// Check if Book class exists
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /public\s+class\s+Book\s*\{[\s\S]*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you create a `Book` class with the required properties?'
    }
  };
}

// Check if Id property has [Required] attribute
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /\[Required\]\s*public\s+int\s+Id\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly: 'Does the `Id` property in `Book` have the `[Required]` attribute?'
    }
  };
}

// Check if Title property has [Required] and [StringLength(100, MinimumLength = 3)]
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /\[Required\]\s*\[StringLength\s*\(\s*100\s*,\s*MinimumLength\s*=\s*3\s*\)\]\s*public\s+string\s+Title\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Does the `Title` property in `Book` have `[Required]` and `[StringLength(100, MinimumLength = 3)]` attributes?'
    }
  };
}

// Check if Pages property has [Range(1, 2000)]
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /\[Range\s*\(\s*1\s*,\s*2000\s*\)\]\s*public\s+int\s+Pages\s*\{\s*get;\s*set;\s*\}/
  )
) {
  return {
    pass: false,
    errors: {
      friendly: 'Does the `Pages` property in `Book` have `[Range(1, 2000)]` attribute?'
    }
  };
}

return { pass: true };
```
  
2. Checkpoint: Inside the POST `/api/books` endpoint, set up a validation context for the incoming book so the system knows which object to check.

Hint: Pass the book object into `new ValidationContext(book)`.

```js
// Ensure ValidationContext is created inside the POST endpoint for /api/books 
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*"\/api\/books"\s*,\s*\(\s*Book\s+\w+\s*\)\s*=>\s*\{[\s\S]*?var\s+.*\s*=\s*new\s+ValidationContext\s*\(\s*\w+\s*\)\s*;[\s\S]*?book_list\.Add\s*\(\s*\w+\s*\)/ 
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Inside the POST endpoint at `/api/books`, did you create a `ValidationContext` for the book object before adding it to the book list?'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Prepare a container to hold any validation errors detected during the check.

Hint: A `List<ValidationResult>` works well for this.

```js
// Ensure ValidationResult list is created inside the POST endpoint for /api/books 
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*"\/api\/books"\s*,\s*\(\s*Book\s+\w+\s*\)\s*=>\s*\{[\s\S]*?var\s+.*\s*=\s*new\s+ValidationContext\s*\(\s*\w+\s*\)\s*;[\s\S]*?var\s+.*\s*=\s*new\s+List<ValidationResult>\s*\(\s*\)/ 
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Inside the POST endpoint at `/api/books`, did you create a list of `ValidationResult` objects to store validation results?'
    }
  };
}

return { pass: true };
```
   
4. Checkpoint: Run the validation process to ensure the book meets all rules using `Validator.TryValidateObject()`. If validation fails, return a 400 Bad Request containing the error details. If valid, add it to the list.

Hint: Pass the book, context, results list, and `true` to `Validator.TryValidateObject()` to validate all properties. Return `Results.BadRequest(results)` when validation fails.

```js
// Ensure Validator.TryValidateObject with book and 'true', and BadRequest logic exist in POST /api/books endpoint
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*"\/api\/books"\s*,\s*\(\s*Book\s+\w+\s*\)\s*=>\s*\{[\s\S]*?bool\s+\w+\s*=\s*Validator\.TryValidateObject\s*\(\s*book\s*,\s*ctx\s*,\s*results\s*,\s*true\s*\)\s*;[\s\S]*?if\s*\(\s*!\w+\s*\)[\s\S]*?return\s+Results\.BadRequest\s*\(\s*results\s*\)/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Inside the POST endpoint at `/api/books`, did you validate the book object using `Validator.TryValidateObject(book, ctx, results, true)` and return `Results.BadRequest(results)` if it is invalid?'
    }
  };
}

return { pass: true };
```

5. Checkpoint: Now test your endpoint using Swagger UI.
   
First, submit a valid book with all fields filled correctly. Then, try different invalid cases — like a missing title, a short title, or too many pages — and observe how the API responds with helpful error messages.

Hint: Use Swagger’s **Try it out** button on POST `/api/books`. Try this valid input:

```json
{
  "id": 1,
  "title": "Learn APIs",
  "pages": 250
}
```
Then try invalid inputs like:

- A title that’s only 1 character long
- A page count of 0
- A missing `Id` or `Title`

You should get `400 Bad Request` responses with specific validation errors.

## Exercise 9: API Documentation with OpenAPI

### Instructions:

1. Checkpoint: Open `localhost:8000/swagger/v1/swagger.json` in your browser and observe the structure of the generated OpenAPI specification. Take a moment to scan it. You’ll notice the structure includes paths, methods, parameters, and schema definitions for your `Book` model.

```js
var validUrls = ['https://localhost:8000/swagger/v1/swagger.json'];
if (Components.WebBrowser.loadedOneOf(validUrls)) {
    return {pass: true};
}
var combinedUrls = [validUrls.slice(0, -1).join(', '), validUrls.slice(-1)[0]].join(validUrls.length < 2 ? '' : ' or ');
return {
  pass: false,
  errors: {
    friendly: 'Did you visit ' + combinedUrls + ' in the browser?'
  }
};
```

2. Checkpoint: Now that we’ve seen the OpenAPI structure, let’s make the `/api/books` GET endpoint more descriptive.

Add metadata to the GET endpoint using `.WithName()`, `.WithSummary()`, and `.WithDescription()`.

Open Swagger at `https://localhost/swagger` and observe the metadata in the `/api/books` GET endpoint.

Hint: You can chain these methods directly after `MapGet(...)`.

```js
// Ensure .WithName, .WithSummary, and .WithDescription are chained to the GET endpoint before the semicolon
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapGet\s*\(\s*["']\/api\/books["']\s*,\s*\(\s*\)\s*=>[\s\S]*?\)\s*[\s\S]*?\.WithName\s*\(.*?\)\s*\.WithSummary\s*\(.*?\)\s*\.WithDescription\s*\(.*?\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Make sure `.WithName()`, `.WithSummary()`, and `.WithDescription()` are chained to the GET endpoint at `/api/books`, and all appear before the semicolon.'
    }
  };
}

return { pass: true };
```

3. Checkpoint: Let’s improve the /api/books POST endpoint. Use `.Accepts<Book>()` to declare the expected input model, and `.Produces<Book>()` to define what a successful response returns.
 
Hint: You can chain these methods directly after `MapPost(...)`.

```js
// Ensure .Accepts and .Produces are chained to the POST endpoint before the semicolon
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*["']\/api\/books["']\s*,\s*\(Book\s+\w+\)\s*=>[\s\S]*?\)[\s\S]*?\.Accepts\s*<\s*Book\s*>\s*\(\s*["']application\/json["']\s*\)\s*\.Produces\s*<\s*Book\s*>\s*\(\s*StatusCodes\.Status201Created\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly:
        'Make sure `.Accepts<Book>("application/json")` and `.Produces<Book>(StatusCodes.Status201Created)` are chained to the POST endpoint at `/api/books`, and all appear before the semicolon.'
    }
  };
}

return { pass: true };
```

4. Checkpoint: Organize your API better by adding `.WithTags("Books")` to both the GET and POST endpoints for `/api/books`.

```js
// Ensure .WithTags("Books") is chained to the GET endpoint 
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapGet\s*\(\s*["']\/api\/books["']\s*,\s*\(\s*\)\s*=>[\s\S]*?\)\s*[\s\S]*?\.WithName\s*\(.*?\)\s*\.WithSummary\s*\(.*?\)\s*\.WithDescription\s*\(.*?\)\s*\.WithTags\s*\(\s*["']Books["']\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly: 'Make sure `.WithTags("Books")` is chained to the GET endpoint at `/api/books`, and appear before the semicolon.'
    }
  };
}

// Ensure .WithTags("Books") is chained to the POST endpoint 
if (
  !Components.CodeEditor.codeContains(
    'Program.cs',
    /app\.MapPost\s*\(\s*["']\/api\/books["']\s*,\s*\(Book\s+\w+\)\s*=>[\s\S]*?\)\s*[\s\S]*?\.Accepts\s*<\s*Book\s*>\s*\(\s*["']application\/json["']\s*\)\s*\.Produces\s*<\s*Book\s*>\s*\(\s*StatusCodes\.Status201Created\s*\)\s*\.WithTags\s*\(\s*["']Books["']\s*\)\s*;/
  )
) {
  return {
    pass: false,
    errors: {
      friendly: 'Make sure `.WithTags("Books")` is chained to the POST endpoint at `/api/books`, and appear before the semicolon.'
    }
  };
}

return { pass: true };
```

## Exercise 10: Review

