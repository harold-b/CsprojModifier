# CsprojModifier

CsprojModifier improves the developer experience in Visual Studio and Rider by performing additional processing when Unity Editor generates .csproj.

CsprojModifier provides following features:

- Insert additional projects as `Import` elements into generated .csproj
- Add analyzer references to generated .csproj

![](docs/images/Screen-01.png)

<!-- START doctoc -->
<!-- END doctoc -->

## Works with
- Unity Editor 2019.4 (LTS) or later
- Visual Studio 2019 or Rider

## Install
Add the package via git from Package Manager.

If you are using Unity 2019.1 or later, you can use Package Manager to install a package via git.

```
https://github.com/Cysharp/CsprojModifier.git?path=src/CsprojModifier/Assets/CsprojModifier
```

## Features

### Insert additional projects as `Import` elements into generated .csproj
Add references to additional project files (.props or .target) to the generated .csproj using `Import` element. This enables you to add files to the project, add references, and so on.

**Note:** .csproj is only used in IDEs such as Visual Studio and Rider, and does not affect the actual compile time by Unity Editor.

#### Example
For example, you can create the following file `YourAwesomeApp.DesignTime.props` and import it to use [BannedApiAnalyzer](https://github.com/dotnet/roslyn-analyzers/tree/main/src/Microsoft.CodeAnalysis.BannedApiAnalyzers).

```xml
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ItemGroup>
    <Analyzer Include="Assets/Plugins/Editor/Analyzers/Microsoft.CodeAnalysis.BannedApiAnalyzers.dll" />
    <Analyzer Include="Assets/Plugins/Editor/Analyzers/Microsoft.CodeAnalysis.CSharp.BannedApiAnalyzers.dll" />
  </ItemGroup>
  <ItemGroup>
    <AdditionalFiles Include="$(ProjectDir)\BannedSymbols.txt" />
  </ItemGroup>
</Project>
```

[BannedApiAnalyzer](https://github.com/dotnet/roslyn-analyzers/tree/main/src/Microsoft.CodeAnalysis.BannedApiAnalyzers) expects BannedSymbols.txt as `AdditionalFiles` to be included in the project, so adding it this way will work.

### Add analyzer references to generated .csproj
[Roslyn analyzers are supported in Unity 2020.2 or later](https://docs.unity3d.com/Manual/roslyn-analyzers.html). However,  currently, Roslyn analyzers are not included in .csproj, and those are only used at compile time.

The extension will insert `Analzyer` element into .csproj when generating the project file. As a result, you can run Roslyn Analyzer when editing code in Visual Studio. (of course, you can use it before 2020.2!)

#### How to use
[Add Roslyn Analyzer to your project in the same way that it is supported in 2020.2.](https://docs.unity3d.com/Manual/roslyn-analyzers.html)

- Add the Roslyn analyzer libraries
- Uncheck all target platforms in the Plugin inspector
- Add Asset Label `RoslynAnalyzer` to the libraries

## License
MIT License