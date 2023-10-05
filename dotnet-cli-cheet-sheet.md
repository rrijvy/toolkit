<h1>DOTNET CLI COMMANDS</h1>

Initialize Web API project

```sh
  dotnet new webapi
```

Initialize Web API project in new folder

```sh
  dotnet new webapi -n [FolderName]
```

Install code generation tool

```sh
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

Add controller

```sh
  dotnet aspnet-codegenerator controller -name [NameController] -outDir Controllers
```

Install NuGet Packages

```sh
  dotnet add package [PackageName]
```

