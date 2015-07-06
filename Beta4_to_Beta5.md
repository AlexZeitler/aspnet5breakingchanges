### Configuration

* [Microsoft.Framework.ConfigurationModel assembly name changed to Microsoft.Framework.Configuration](https://github.com/aspnet/Announcements/issues/25)
* [Configuration source implementations now have their own packages](https://github.com/aspnet/Announcements/issues/32)

This means in `project.json` you have change this:
`````
"Microsoft.Framework.ConfigurationModel.Json": "1.0.0-beta4",
````

To this:
````
"Microsoft.Framework.Configuration": "1.0.0-beta5",
"Microsoft.Framework.Configuration.Json": "1.0.0-beta5",
````

Your code might have been looking like this:

````
var configuration = new Configuration();
configuration.AddJsonFile("config.json");
````

Now you have to use this:

`````
var configurationBuilder = new ConfigurationBuilder(
  new JsonConfigurationSource("config.json")
);
var configuration = configurationBuilder.Build();
````

### IOutputFormatter
* No official Announcement

`IOutputFormatter` no longer contains the `Instance` property.

Your old code like this:
````
services.ConfigureMvc(options =>
{
  (options.OutputFormatters.First(f => f.Instance is JsonOutputFormatter).Instance as
    JsonOutputFormatter).SerializerSettings.ContractResolver =
    new CamelCasePropertyNamesContractResolver();
});
````
  
has to be changed to this:
`````
services.ConfigureMvc(options =>
{
  (options.OutputFormatters.First(f => f is JsonOutputFormatter) as
      JsonOutputFormatter).SerializerSettings.ContractResolver =
      new CamelCasePropertyNamesContractResolver();
});
````
