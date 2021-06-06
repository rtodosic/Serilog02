## Context
1. [.Net Core Serilog – Basic](https://github.com/rtodosic/Serilog01/)
2. .Net Core Serilog – Configuration
3. [.Net Core Serilog - Structured JSON output](https://github.com/rtodosic/Serilog03/)
4. [.Net Core Serilog - Enrichers](https://github.com/rtodosic/Serilog04/)
5. [.Net Core Serilog - Custom JSON output](https://github.com/rtodosic/Serilog05/)
6. [.Net Core Serilog - Adding Sinks](https://github.com/rtodosic/Serilog06/)

This is part 2 of 6.

## 2. .Net Core Serilog – Configuration

Serilog has a lot that could be managed via [configuration](https://github.com/serilog/serilog-settings-configuration). 
To keep things as simple as possible, we will build on the prior sample and use the appsettings.json file to manage the output logging levels. 

1. In WeatherForecastController.cs, change the logging to “LogDebug”. The log line should be as follows:
   ```C#
      _logger.LogDebug("===>>>Get was called {data}", nameof(WeatherForecast));
   ```
 
2. Run the app and notice the output does not contain our log message.
  ![Image alt text](Images/Console-Without-Debug.png?raw=true)

3. In the appsettings.json file, replace the contents with the following (under “Override”, change “Serilog02” to match the namespace used in your WeatherForecastController.cs file):
  ```JSON
  {
    "Serilog": {
      "MinimumLevel": {
        "Default": "Information",
        "Override": {
          "Serilog02": "Debug",
          "Microsoft.Hosting.Lifetime": "Information"
        }
      }
    },
    "AllowedHosts": "*"
  }
  ```

4.  In Program.cs, change the CreateHostBuilder() method to the following:
  ```C#
  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          })
      .UseSerilog((hostingContext, loggerConfiguration) => loggerConfiguration
          .ReadFrom.Configuration(hostingContext.Configuration)
          .WriteTo.Console()
      );
   ```

5. Run the app again and notice that we now see the debug log (9th log message from the top).
  ![Image alt text](Images/Console-With-Debug.png?raw=true)

6.	In the appsettings.json file, you can try changing the “Serilog: MinimumLevel:Default” to “Debug”, “Warning” and “Error”. Re-run and notice the differences in the output. 

7.	In the appsettings.json file, you can also try changing the namespace overrides to control different log levels for different namespaces.
