# Create a minimal API with ASP.NET Core

## Option 1: Using Visual Studio Code

### Prerequisites:
- [.NET SDK 6.0 or later](https://dotnet.microsoft.com/en-us/download)
- [Visual Studio Code](https://code.visualstudio.com/)
- C# extension for VS Code (Microsoft)

### Steps:

#### 1. **Create a new project**
Open a terminal and run:

```bash
dotnet new web -o Name
```
* This creates a new ASP.NET Core project using the minimal API template.
* Replace `Name` with your desired project name.

#### 2. **Navigate into the project folder**
```bash
cd Name
```

#### 3. **Build the project**
```bash
dotnet build
```
* This compiles the project and checks for any errors.

#### 4. **Run the application**
```bash
dotnet run
```
* This starts the web server.
* You’ll see output like:
```
Now listening on: https://localhost:5001
Application started. Press Ctrl+C to shut down
```
#### 5. **Test the default endpoint**
```bash
dotnet build
```
* Open your browser and go to `https://localhost:5001` (or the URL shown in the terminal).  
* You should see a basic **"Hello World"** response.
---
## Option 2: Using Visual Studio (2022 or later)

### Prerequisites:
- [.NET SDK 6.0 or later](https://dotnet.microsoft.com/en-us/download)
- [Visual Studio 2022](https://visualstudio.microsoft.com/downloads/) with the ASP.NET and web development workload.

  ![ASP.NET and web development workload](image_url)

### Steps:

#### 1. **Create a new project**
Open a terminal and run:

```bash
dotnet new web -o Name
```
* This creates a new ASP.NET Core project using the minimal API template.
* Replace `Name` with your desired project name.

#### 2. **Navigate into the project folder**
```bash
cd Name
```
