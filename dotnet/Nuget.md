# 1. NuGet.config

# 2. Nuspec

    A .nuspec file is not required to create packages for SDK-style projects (typically .NET Core and .NET Standard projects that use the SDK attribute).
   (Note that a .nuspec is generated when you create the package.)
   这句话的意思是：

对于 SDK 风格的项目（通常是使用 SDK 属性的 .NET Core 和 .NET Standard 项目），不需要一个 .nuspec 文件来创建包。
（需要注意的是，当你创建包时，系统会自动生成一个 .nuspec 文件。）

具体解释：
SDK 风格的项目是指使用了 Microsoft.NET.Sdk 或相关 SDK 的项目文件（.csproj 或 .vbproj），这些文件通过 SDK 属性简化了项目配置。
例如：
xml
复制代码
<Project Sdk="Microsoft.NET.Sdk">
</Project>
无需手动编写 .nuspec 文件

在传统的 NuGet 包创建方式中，开发者需要手动编写 .nuspec 文件来定义包的元数据（如名称、版本、依赖项等）。
对于 SDK 风格的项目，这些元数据可以直接在项目文件（.csproj）中定义，不需要额外的 .nuspec 文件。
系统自动生成 .nuspec

当你运行 dotnet pack 或其他创建 NuGet 包的命令时，系统会自动从项目文件生成一个 .nuspec 文件，并将其包含在最终的包中。
示例：
传统方式（需要 .nuspec 文件）：
需要手动创建类似以下的 .nuspec 文件：

xml
复制代码
```
<?xml version="1.0"?>
<package>
  <metadata>
    <id>MyPackage</id>
    <version>1.0.0</version>
    <authors>Author Name</authors>
    <description>Package description.</description>
    <dependencies>
      <dependency id="Newtonsoft.Json" version="12.0.3" />
    </dependencies>
  </metadata>
</package>
```
SDK 风格（无需 .nuspec 文件）：
只需在 .csproj 文件中定义：

xml
复制代码
```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <PackageId>MyPackage</PackageId>
    <Version>1.0.0</Version>
    <Authors>Author Name</Authors>
    <Description>Package description.</Description>
  </PropertyGroup>
</Project>
```
运行 dotnet pack 后，系统会自动生成 .nuspec 文件，并将其打包到 .nupkg 中。

总结：
对于 SDK 风格的项目，创建 NuGet 包更加简化，不需要手动编写 .nuspec 文件，直接使用 .csproj 文件即可完成配置。这种方式更高效，也更符合现代 .NET 项目开发的习惯。

# 打包方式
在使用 NuGet 打包时，每个项目不一定需要打包成一个单独的包。是否将每个项目打包成独立的包取决于你的具体需求和项目结构。以下是几种常见的情况和解决方案：

1. 单个项目打包成一个 NuGet 包
如果你的解决方案中只有一个库项目，并且你只需要将该库打包成 NuGet 包，可以只对该项目进行打包。这是最简单的一种情况，只需要使用 nuget pack 或 dotnet pack 命令。

示例：

只有一个库项目，生成的包中包含该项目的所有内容（程序集、依赖项等）。
适用于简单的单一库发布。
2. 多个项目打包成一个 NuGet 包
如果你有多个项目，但希望将它们打包成一个单独的 NuGet 包，可以通过以下几种方式实现：

通过 .nuspec 文件手动指定文件：可以在 .nuspec 文件中指定多个项目的输出文件（如程序集、内容文件等），并将它们打包成一个包。
多项目打包（项目依赖）：如果这些项目之间有依赖关系，你可以将这些项目的输出作为依赖项集成到一个包中。例如，你有一个主项目，它引用了多个子项目，这样你可以在主项目的 .nuspec 或 .csproj 中列出这些子项目的输出。
示例：

xml
复制代码
~~~
<files>
  <file src="..\ProjectA\bin\Release\ProjectA.dll" target="lib\netstandard2.0\ProjectA.dll" />
  <file src="..\ProjectB\bin\Release\ProjectB.dll" target="lib\netstandard2.0\ProjectB.dll" />
</files>
~~~
3. 每个项目打包成一个单独的 NuGet 包
如果你有多个项目且每个项目都是一个独立的组件（例如不同的库或模块），那么你可以为每个项目单独创建一个 NuGet 包。这种方式通常用于大规模的多项目开发，特别是当这些项目可以独立使用时。

示例：

为每个项目创建单独的 NuGet 包，并在解决方案中的其他项目中通过 PackageReference 引用这些包。
这种方式适合于需要维护和发布多个独立版本的库。
4. 组合多个项目生成的包
你还可以选择 组合多个包。例如，通过一个 聚合包（meta package），该包不包含实际的代码或资源，但引用了多个 NuGet 包，形成一个更大的整体。

示例：

xml
复制代码
~~~
<package>
  <metadata>
    <id>MyAggregatedPackage</id>
    <version>1.0.0</version>
    <dependencies>
      <dependency id="ProjectA" version="1.0.0" />
      <dependency id="ProjectB" version="1.0.0" />
    </dependencies>
  </metadata>
</package>
~~~
这种方式适用于将多个包捆绑在一起进行发布，并且所有包都能独立使用。

5. 使用 dotnet pack 多项目打包
在 SDK 风格的项目中，你可以通过 dotnet pack 打包多个项目，但你需要为每个项目单独运行该命令，或者在 csproj 文件中设置多项目打包。例如，使用一个单独的 .csproj 文件包含多个项目，并将它们一起打包。

总结：
不需要每个项目都打包成一个单独的 NuGet 包，你可以根据需要将多个项目打包成一个包，或者将每个项目打包为独立的包。
如果需要将多个项目合并成一个包，可以通过 .nuspec 文件手动控制。
如果你希望每个项目有单独的版本和独立发布，可以为每个项目打包一个独立的 NuGet 包。

# 4. Nuget 包的内容输出一起被打包到conan 包中， 那么需要将所有依赖的输出一起打包
    NuGet 包的标准结构要求 .dll 文件应放在 lib 目录下。
    常见错误信息：
    ~~~
    2>C:\Program Files\dotnet\sdk\8.0.204\Sdks\NuGet.Build.Tasks.Pack\buildCrossTargeting\NuGet.Build.Tasks.Pack.targets(221,5): warning NU5100: The assembly 'content\bin\debug\net6.0\ClassLibrary1.dll' is not inside the 'lib' folder and hence it won't be added as a reference when the package is installed into a project. Move it into the 'lib' folder if it needs to be referenced.
2>C:\Program Files\dotnet\sdk\8.0.204\Sdks\NuGet.Build.Tasks.Pack\buildCrossTargeting\NuGet.Build.Tasks.Pack.targets(221,5): warning NU5100: The assembly 'contentFiles\any\net6.0\bin\debug\net6.0\ClassLibrary1.dll' is not inside the 'lib' folder and hence it won't be added as a reference when the package is installed into a project. Move it into the 'lib' folder if it needs to be referenced.
2>C:\Program Files\dotnet\sdk\8.0.204\Sdks\NuGet.Build.Tasks.Pack\buildCrossTargeting\NuGet.Build.Tasks.Pack.targets(221,5): warning NU5100: The assembly 'contentFiles\any\net8.0\bin\debug\net6.0\ClassLibrary1.dll' is not inside the 'lib' folder and hence it won't be added as a reference when the package is installed into a project. Move it into the 'lib' folder if it needs to be referenced.
2>C:\Program Files\dotnet\sdk\8.0.204\Sdks\NuGet.Build.Tasks.Pack\buildCrossTargeting\NuGet.Build.Tasks.Pack.targets(221,5): warning NU5100: The assembly 'content\bin\debug\net6.0\ConsoleApp3.dll' is not inside the 'lib' folder and hence it won't be added as a reference when the package is installed into a project. Move it into the 'lib' folder if it needs to be referenced.
    ~~~

    这个警告 NU5100 通常出现在 NuGet 打包时，表示 程序集（如 ClassLibrary1.dll 或 ConsoleApp3.dll）没有被放置在 NuGet 包的 lib 文件夹中，因此无法作为引用被自动添加到目标项目中。这种情况通常发生在你通过 NuGet 打包时，某些文件（如程序集）被放置到了错误的目录，导致 NuGet 无法正确识别它们并将其作为引用添加到项目中。

解决方法：
要解决这个警告，你需要确保将程序集文件放置在正确的目录结构下，通常是 lib 文件夹，而不是其他目录（如 content 或 contentFiles）。NuGet 包的标准结构要求 .dll 文件应放在 lib 目录下。

1. 使用 .csproj 配置正确的打包路径
在 .csproj 文件中，你可以指定程序集应该被打包到 lib 目录下，这样当 NuGet 包安装时，程序集文件会被正确地添加为项目的引用。

以下是如何在 .csproj 文件中配置，将 bin\debug\net6.0\ClassLibrary1.dll 或其他程序集放到 lib\net6.0 目录下：

xml
复制代码
~~~
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <OutputPath>bin\Debug\net6.0\</OutputPath>  <!-- 你可以设置输出路径 -->
  </PropertyGroup>

  <ItemGroup>
    <!-- 将程序集文件放到 lib\net6.0 目录 -->
    <None Include="bin\Debug\net6.0\ClassLibrary1.dll" Pack="true" PackagePath="lib\net6.0\ClassLibrary1.dll" />
    <None Include="bin\Debug\net6.0\ConsoleApp3.dll" Pack="true" PackagePath="lib\net6.0\ConsoleApp3.dll" />
  </ItemGroup>

</Project>
~~~
2. 确保打包的文件在 lib 目录下
你要确保所有的程序集（如 ClassLibrary1.dll 和 ConsoleApp3.dll）在包内的路径符合 NuGet 包的标准结构。在 NuGet 包中，所有的程序集应该放在 lib/{target-framework} 目录下，例如：

markdown
复制代码
lib
 ├── net6.0
     ├── ClassLibrary1.dll
     ├── ConsoleApp3.dll
3. 避免错误的路径结构
警告中提到的文件路径显示为 content\bin\debug\net6.0\ClassLibrary1.dll 和 contentFiles\any\net6.0\bin\debug\net6.0\ClassLibrary1.dll，这表明你的文件被错误地放到了 content 或 contentFiles 文件夹中。一般来说，程序集文件应该放在 lib 目录下。如果它们出现在 content 或 contentFiles 目录中，NuGet 将不会自动将它们添加为引用。

你可以通过 PackagePath 属性来确保程序集被正确放置在 lib 目录中，如上述 .csproj 配置所示。

4. 使用 dotnet pack 或 nuget pack 打包
在修改 .csproj 文件后，可以通过以下命令打包项目：

bash
复制代码
dotnet pack
或者，如果你使用的是 .nuspec 文件，可以使用 nuget pack 命令。

5. 验证生成的包结构
生成的 .nupkg 文件可以通过以下命令解压并检查其内部结构，确保程序集文件被放置在正确的目录中：

bash
复制代码
~~~
nuget locals all -clear  # 清理缓存
nuget pack MyPackage.nuspec  # 打包
nuget verify MyPackage.nupkg  # 验证包
~~~
或者使用任何压缩软件打开 .nupkg 文件，并查看 lib/net6.0/ 目录，确认程序集文件是否在其中。

总结：
确保程序集文件放在 lib/{target-framework} 目录。
使用 Pack="true" 和 PackagePath 在 .csproj 文件中正确配置文件的路径。
避免将程序集文件放入 content 或 contentFiles 文件夹中。
使用 dotnet pack 或 nuget pack 打包，并检查包结构。
通过这些步骤，你可以解决 NuGet 打包过程中的 NU5100 警告，并确保程序集能够被正确引用。
   
# 打包策略：
    Nuget 包的内容输出一起被打包到conan 包中， 那么需要将所有依赖的输出一起打包， 因为在Conan的领域中， Nuget 包的依赖关系将不起作用
    传统SignalStudio的项目文件格式将由Sdk代替， 这是为了将来的升级方便， 但是只支持两种格式
    net framework 和 net 8(9, 10)
    或
    net and net-windows 
    对于前者， net-windows分支将交给SA 维护， 且不merge back

     传统SignalStudio 的项目可以做包但是不单独上传， 因为不共享。
     transport 必须提供net framework 和 net8.0 het6.0 3种格式， 但是依然保留原来的项目文件， 因为AWU 不需要进入linux
     signalstudio 和 signalstudioModels 也需要net framework 和 net8.0 het6.0 3种格式， 因为要被 transport 依赖。
     basebandframwork 待定。

    最好的策略， 才哟个sdk格式， 同时分离net8-windows， 只用 netframework 和net8,6, 10, 因为随着net 的演进， net-windows支持的控件是有变化的
     
    
    
    
