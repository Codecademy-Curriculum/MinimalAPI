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
