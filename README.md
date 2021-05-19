

## .Net Core Serilog – Configuration

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
  
4. Now add the “Serilog.AspNetCore” NuGet package to your project.
  ![Image alt text](Images/Console-With-Debug.png?raw=true)

5.	In the appsettings.json file, you can try changing the “Serilog: MinimumLevel:Default” to “Debug”, “Warning” and “Error”. Re-run and notice the differences in the output. 

6.	In the appsettings.json file, you can also try changing the namespace overrides to control different log levels for different namespaces.
