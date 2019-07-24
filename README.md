# Extensions.Hosting.AsyncInitialization

A simple helper to perform async application initialization for the generic host in .NET Core 2.1 or higher.

## Usage

1. Install the [Extensions.Hosting.AsyncInitialization](https://www.nuget.org/packages/Extensions.Hosting.AsyncInitialization/) NuGet package:

    Command line:

    ```PowerShell
    dotnet add package Extensions.Hosting.AsyncInitialization
    ```

    Package manager console:
    ```PowerShell
    Install-Package Extensions.Hosting.AsyncInitialization
    ```


2. Create a class (or several) that implements `IAsyncInitializer`. This class can depend on any registered service.

    ```csharp
    public class MyAppInitializer : IAsyncInitializer
    {
        public MyAppInitializer(IFoo foo, IBar bar)
        {
            ...
        }

        public async Task InitializeAsync()
        {
            // Initialization code here
        }
    }
    ```

3. Register your initializer(s) in the same place as other services:

    ```csharp
        services.AddAsyncInitializer<MyAppInitializer>();
    ```

4. In the `Program` class, make the `Main` method async and change its code to initialize the host before running it:

    ```csharp
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();
        await host.InitAsync();
        await host.RunAsync();
    }
    ```

(Note that you need to [set the C# language version to 7.1 or higher in your project](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/configure-language-version#edit-the-csproj-file) to enable the "async Main" feature.)

This will run each initializer, in the order in which they were registered.