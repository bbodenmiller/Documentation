# Dynamic Logging Provider

This logging provider is a wrapper around the [Microsoft Console Logging](https://github.com/aspnet/Logging) provider. This wrapper allows for querying the currently defined loggers and their levels as well as then modifying the levels dynamically at runtime.

For more information on how to use [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/2-0/console/index.html) on Cloud Foundry for viewing and modifying logging levels, see the [Using Actuators with Apps Manager section](https://docs.pivotal.io/pivotalcf/2-0/console/using-actuators.html) of the Pivotal Cloud Foundry documentation.

>CAUTION: The Pivotal Apps Manager integration involves sending the fully-qualified logger name over HTTP. Avoid using colons in the name of a logger to prevent invalid HTTP Requests.

## Usage

Before starting to use Steeltoe provider, you should know how the .NET [logging service](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?tabs=aspnetcore2x) works, as it is nothing more than a wrapper around the existing Microsoft console logger.

To use the Steeltoe logging provider, you need to:

1. Add the logging NuGet package references to your project.
1. Configure Logging settings.
1. Add the dynamic logging provider to the logging builder.

### Add NuGet References

To use the logging provider, you need to add a reference to the Steeltoe Dynamic Logging NuGet.

The provider is found in the `Steeltoe.Extensions.Logging.DynamicLogger` package.

You can add the provider to your project by using the following `PackageReference`:

```xml
<ItemGroup>
...
    <PackageReference Include="Steeltoe.Extensions.Logging.DynamicLogger" Version= "2.1.0"/>
...
</ItemGroup>
```

### Configure Settings

As mentioned earlier, the Steeltoe Logging provider is a wrapper around the Microsoft Console logging provider. Consequently, you can configure it the same way you would the Microsoft provider. For more details on how this is done, see the section on [Log Filtering](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?tabs=aspnetcore2x#log-filtering).

### Add the Logging Provider

To use the provider, you need to add it to the logging builder by using the `AddDynamicConsole()` extension method:

```csharp
using Steeltoe.Extensions.Logging;
public class Program
{
    public static void Main(string[] args)
    {
        var host = WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .ConfigureLogging((builderContext, loggingBuilder) =>
            {
                // Add Steeltoe Dynamic Logging provider
                loggingBuilder.AddDynamicConsole();
            })
            .Build();

        host.Run();
    }
}
```
