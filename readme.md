[![AppVeyor](https://img.shields.io/appveyor/ci/magic-chunks/magic-chunks-dotnetcore/master.svg?maxAge=2592000)](https://ci.appveyor.com/project/magic-chunks/magic-chunks-dotnetcore) [![Github Releases](https://img.shields.io/github/downloads/magic-chunks/magic-chunks-dotnetcore/total.svg?maxAge=2592000)](https://github.com/magic-chunks/magic-chunks-dotnetcore/releases) [![NuGet](https://img.shields.io/nuget/v/MagicChunks.svg?maxAge=2592000)](https://www.nuget.org/packages/MagicChunks/) [![Visual Studio Marketplace](https://img.shields.io/badge/VS%20Marketplace-1.3.1-yellowgreen.svg)](https://marketplace.visualstudio.com/items?itemName=sergeyzwezdin.magic-chunks)

![Magic Chunks](assets/title.png)

Easy to use tool to config transformations for JSON, XML and YAML.

---

Everyone remember [XML Document Transform](https://msdn.microsoft.com/en-us/library/dd465326.aspx) syntax to transform configuration files during the build process. But world is changing and now you can have different config types in your .NET projects.

**Magic Chunks** allows you to transform you JSON, XML and YAML files. You can run it at [MSBuild](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/MSBuild), [Cake](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Cake), [PSake](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/PSake) or [Powershell](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Powershell) script as well as use [Visual Studio Team Services build extension](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Visual-Studio-Team-System-extension). Also, it's possible to reference Magic Chunks from your .NET projects in more complicated cases.

# How it works

The main idea is quite simple. Magic Chunks represents transformation as a key-value collection.

The key contains path in the source file which should be modified, and the value contains data for this path in modified file.

If you are using Magic Chunks from .NET or Cake, you can also pass in any keys to be removed into the constructor.

#### XML

Imagine you have following XML based configuration file.

```xml
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.5.1" />
    <authentication mode="None" />
  </system.web>
</configuration>
```

So following transformations could be applied to this config:

```json
{
  "configuration/system.web/compilation/@debug": "false",
  "configuration/system.web/authentication/@mode": "Forms"
}
```

As a result you will have config like this:

```xml
<configuration>
  <system.web>
    <compilation debug="false" targetFramework="4.5.1" />
    <authentication mode="Forms" />
  </system.web>
</configuration>
```

#### JSON

The same approach works if you have JSON based configuration:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=webapp"
  }
}
```

Transformation for the config could be:

```json
{
  "ConnectionStrings/DefaultConnection": "Data Source=10.0.0.5;Initial Catalog=Db1;Persist Security Info=True"
}
```

After transformation you will have:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=10.0.0.5;Initial Catalog=Db1;Persist Security Info=True"
  }
}
```

# Supported formats

Magic Chunks supports following file formats:

1. [XML](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/XML)
2. [JSON](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/JSON)
3. [YAML](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/YAML)

# Getting started

Let's say you have `appsettings.json` file at `C:\sources\project1` folder:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=webapp"
  }
}
```

To use Magic Chunks download [latest release](https://github.com/magic-chunks/magic-chunks-dotnetcore/releases) manually or get it directly from [Nuget](http://nuget.org).

For example we will use Powershell to transform configuration file. To do this you have to write something like this:

```powershell
Import-Module .\MagicChunks.psm1

Format-MagicChunks -path C:\sources\project1\appsettings.json -transformations @{
 "ConnectionStrings/DefaultConnection" = "Data Source=10.0.0.5;Initial Catalog=Db1;Persist Security Info=True"
}
```

To transform config files you can use any approach you like:

 - [MSBuild](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/MSBuild)
 - [Cake](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Cake)
 - [Powershell](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Powershell)
 - [PSake](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/PSake)
 - [VSTS extension](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Visual-Studio-Team-System-extension)

To learn more check [wiki page](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki).

# Maintainers

- Sergey Zwezdin ([@sergeyzwezdin](https://github.com/sergeyzwezdin))
- Gary Ewan Park ([@gep13](https://github.com/gep13))
- Pascal Berger ([@pascalberger](https://github.com/pascalberger))

# Contributions

Any contributions are welcome. Most probably someone will want to extend it with additional formats. So feel free to make pull requests for your changes. Read [contribution guidelines](https://github.com/magic-chunks/magic-chunks-dotnetcore/wiki/Contribution-guidelines) to start.

# License

Magic Chunks is released under the [MIT License](https://github.com/magic-chunks/magic-chunks-dotnetcore/blob/master/LICENSE).
