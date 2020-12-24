---
title: What is the IntelliJ Platform?——什么是IntelliJ平台？
---

The _IntelliJ Platform_ is not a product in and of itself but provides a platform for building IDEs. It is used to power JetBrains products such as [IntelliJ IDEA](https://www.jetbrains.com/idea/). It is also Open Source and can be used by third parties to build IDEs, such as [Android Studio](https://developer.android.com/studio/index.html) from Google.

**CN:**  IntelliJ平台本身并不是一个产品，而是一个构建ide的平台。它被用于为诸如[IntelliJ IDEA](https://www.jetbrains.com/idea/)之类的JetBrains产品提供支持。它也是开源的，第三方可以使用它来构建ide，例如从谷歌的Android Studio。

The IntelliJ Platform provides all of the infrastructure that these IDEs need to provide rich language tooling support. It provides a component driven, cross-platform JVM based application host with a high-level user interface toolkit for creating tool windows, tree views and lists (supporting fast search) as well as popup menus and dialogs.

**CN:**  IntelliJ平台提供了所有这些ide需要的基础设施，以提供丰富的语言工具支持。它提供了一个组件驱动的、跨平台的基于JVM的应用程序主机，以及一个高级用户界面工具包，用于创建工具窗口、树视图和列表(支持快速搜索)，以及弹出菜单和对话框。

It also includes an image editor as well as a full text editor, and provides abstract implementations of syntax highlighting, code folding, code completion, and other rich text editing features.

**CN:**  它还包括一个图像编辑器和一个全文编辑器，并提供了语法突出显示、代码折叠、代码完成和其他富文本编辑功能的抽象实现。

Furthermore, it includes open APIs to build common IDE functionality, such as a project model and a build system. It also provides infrastructure for a very rich debugging experience, with language agnostic advanced breakpoint support, call stacks, watch windows, and expression evaluation.

**CN:**  此外，它还包括用于构建公共IDE功能(如项目模型和构建系统)的开放api。它还为非常丰富的调试体验提供了基础设施，支持语言无关的高级断点、调用堆栈、监视窗口和表达式计算。

But the IntelliJ Platform's real power comes from the Program Structure Interface (PSI). This is a set of functionality that can be used to parse files and build rich syntactic and semantic models of the code, and to build indexes from this data. This powers a lot of functionality, from quick navigating to files, types and symbols, to the contents of code completion windows and find usages, code inspections and code rewriting, for quick fixes or refactorings, as well as many other features.

**CN:**  但是IntelliJ平台的真正厉害之处来自于程序结构接口(PSI)。这是一组功能，可用于解析文件并构建代码的丰富语法和语义模型，以及根据这些数据构建索引。这为很多功能提供了支持，从快速导航到文件、类型和符号，到代码完成窗口的内容和查找用法、代码检查和代码重写、快速修复或重构，以及许多其他功能。

The IntelliJ Platform includes parsers and a PSI model for a number of languages, and its extensible nature means that it is possible to add support for other languages.

**CN:**  IntelliJ平台包括了一些语言的解析器和PSI模型，它的可扩展性意味着可以增加对其他语言的支持。

## Plugins——插件

Products built on the IntelliJ Platform are extensible applications, with the platform being responsible for the creation of components, and the injection of dependencies into classes. The IntelliJ Platform fully supports plugins, and JetBrains hosts a [plugin repository](https://plugins.jetbrains.com) that can be used to distribute plugins that support one or more of the products. It is also possible to host your own repositories, and distribute plugins separately.

**CN:**  构建在IntelliJ平台上的产品是可扩展的应用程序，平台负责组件的创建，并将依赖注入到类中。IntelliJ平台完全支持插件，JetBrains拥有一个[plugin repository](https://plugins.jetbrains.com)，可以用来发布支持一个或多个产品的插件。您还可以托管自己的存储库，并单独分发插件。

Plugins can extend the platform in lots of ways, from adding a simple menu item to adding support for a complete language, build system and debugger. A lot of the existing functionality in the IntelliJ Platform is written as plugins that can be included or excluded depending on the needs of the end product. See the [Quick Start Guide](/basics.md) for more details.

**CN:**  插件可以以多种方式扩展平台，从添加简单的菜单项到添加对完整语言、构建系统和调试器的支持。IntelliJ平台中的许多现有功能都是作为插件编写的，可以根据最终产品的需求来包含或排除这些插件。有关更多细节，请参阅[Quick Start Guide](/basics.md)。

The IntelliJ Platform is a JVM application, written mostly in Java and Kotlin. You should be experienced with these languages, large libraries written in them, their associated tooling, and large open source projects to write plugins for products based on the IntelliJ Platform. At this time, it's not possible to extend the IntelliJ Platform in non-JVM languages.
IntelliJ平台是一个JVM应用程序，主要用Java和Kotlin编写。您应该熟悉这些语言、用它们编写的大型库、相关的工具，以及为基于IntelliJ平台的产品编写插件的大型开源项目。此时，不可能在非jvm语言中扩展IntelliJ平台。

## Open Source——开源

The IntelliJ Platform is Open Source, under the [Apache license](upsource:///LICENSE.txt), and [hosted on GitHub](https://github.com/JetBrains/intellij-community).

**CN:**  IntelliJ平台是开源的，在[Apache license（Apache许可）](upsource:///LICENSE.txt)下，[hosted on GitHub（托管在GitHub上）](https://github.com/JetBrains/intellij-community)上。

While this guide refers to the IntelliJ Platform as a separate entity, there is no "IntelliJ Platform" GitHub repo. Instead, the platform is considered to be an almost complete overlap with the IntelliJ IDEA Community Edition, which is a free and Open Source version of IntelliJ IDEA Ultimate (the GitHub repo linked above is the [JetBrains/intellij-community](https://github.com/JetBrains/intellij-community) repo).

**CN:**  虽然本指南将IntelliJ平台作为一个单独的实体，但并没有"IntelliJ Platform"的GitHub库。相反，该平台被认为与IntelliJ IDEA Community Edition几乎完全重叠，IntelliJ IDEA Community Edition是IntelliJ IDEA Ultimate的一个免费开源版本(上面链接的GitHub库是[JetBrains/intellij-community](https://github.com/JetBrains/intellij-community)库)。

The version of the IntelliJ Platform is defined by the version of the corresponding release of IntelliJ IDEA Community Edition. 
For example, to build a plugin against IntelliJ IDEA (v2019.1.1,) build #191.6707.61, means specifying the same build number tag to get the correct Intellij Platform files from the `intellij-community` repo. 
See the [build number ranges](/basics/getting_started/build_number_ranges.md) page for more information about build numbers corresponding to version numbering.

**CN:**  IntelliJ平台的版本是由IntelliJ IDEA Community Edition的相应版本定义的。
例如，要针对IDEA (v2019.1.1,)构建#191.6707.61的一个插件，意味着要指定相同的构建编号标签，以便从`intellij-community`仓库获得正确的Intellij平台文件。
有关版本编号对应的构建号的更多信息，请参见[build number ranges](/basics/getting_started/build_number_ranges.md)页面。


Typically, an IDE that is based on the IntelliJ Platform will include the `intellij-community` repo as a Git submodule and provide configuration to describe which plugins from the `intellij-community`, and which custom plugins will make up the product. This is how the IDEA Ultimate team work, and they contribute code to both the custom plugins and the IntelliJ Platform itself.

**CN:**  通常，基于IntelliJ平台的IDE将`intellij-community`库作为Git子模块，并提供配置来描述来自`intellij-community`的哪些插件，以及哪些自定义插件将构成产品。这就是IDEA Ultimate团队的工作方式，他们为自定义插件和IntelliJ平台本身贡献代码。

### IDEs Based on the IntelliJ Platform——基于IntelliJ平台的IDE
The IntelliJ Platform underlies many JetBrains IDEs. 
IntelliJ IDEA Ultimate is a superset of the IntelliJ IDEA Community Edition, but includes closed source plugins ([see this feature comparison](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html)). Similarly, other products such as WebStorm and DataGrip are based on the IntelliJ IDEA Community Edition, but with a different set of plugins included and excluding other default plugins.
This allows plugins to target multiple products, as each product will include base functionality and a selection of plugins from the IntelliJ IDEA Community Edition repo.

**CN:**  IntelliJ平台是许多JetBrains IDE的基础。
IntelliJ IDEA Ultimate是IntelliJ IDEA Community Edition的一个超集，但包含了闭源插件([see this feature comparison (查看这个特性比较)](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html))。类似地，其他产品，如WebStorm和DataGrip都是基于IntelliJ IDEA Community Edition的，但是包含了一组不同的插件，并且排除了其他的默认插件。

The following IDEs are based on the IntelliJ Platform:

**CN:**  基于IntelliJ平台的IDE如下
* JetBrains IDEs
  * [AppCode](https://www.jetbrains.com/objc/)
  * [CLion](https://www.jetbrains.com/clion/)
  * [DataGrip](https://www.jetbrains.com/datagrip/)
  * [GoLand](https://www.jetbrains.com/go/)
  * [IntelliJ IDEA](https://www.jetbrains.com/idea/)
  * [MPS](https://www.jetbrains.com/mps/)
  * [PhpStorm](https://www.jetbrains.com/phpstorm/)
  * [PyCharm](https://www.jetbrains.com/pycharm/)
  * [Rider](#rider)
  * [RubyMine](https://www.jetbrains.com/ruby/) 
  * [WebStorm](https://www.jetbrains.com/webstorm/) 
* [Android Studio](https://developer.android.com/studio/index.html) IDE from Google.
* [Comma](https://commaide.com/) IDE for Raku (formerly known as Perl 6)
* [CUBA Studio](https://www.cuba-platform.com/)
* [Cursive](https://cursive-ide.com/) IDE for Clojure 

#### Rider——骑手
JetBrains [Rider](https://www.jetbrains.com/rider/) uses the IntelliJ Platform differently than other IntelliJ based IDEs. It uses the IntelliJ Platform to provide the user interface for a C# and .NET IDE, with the standard IntelliJ editors, tool windows, debugging experience and so on. It also integrates into the standard Find Usages and Search Everywhere UI, and makes use of code completion, syntax highlighting, and so on.

**CN:**  JetBrains [Rider](https://www.jetbrains.com/rider/)使用IntelliJ平台的方式与其他基于IntelliJ的ide不同。它使用IntelliJ平台为C#和.NET IDE提供用户界面，具有标准的IntelliJ编辑器、工具窗口、调试体验等。它还集成到标准的Find Usages和Search Everywhere UI中，并利用了代码补全、语法突出显示等功能。

However, Rider doesn't create a full PSI (syntactic and semantic) model for C# files. Instead, it reuses [ReSharper](https://www.jetbrains.com/resharper/) to provide language functionality. All of the C# PSI model and all inspections and code rewriting, such as quick fixes and refactorings are run out of process, in a command line version of ReSharper. This means that creating a plugin for Rider involves two parts - a plugin that lives in the IntelliJ "front end" to show user interface, and a plugin that lives in the ReSharper "back end" to analyze and work with the C# PSI.

**CN:**  然而，Rider并没有为C#文件创建一个完整的PSI(语法和语义)模型。相反，它重用[ReSharper](https://www.jetbrains.com/resharper/)来提供语言功能。在命令行版本的ReSharper中，所有的C# PSI模型，所有的检查和代码重写，比如快速修复和重构，都会在进程中运行。这意味着为Rider创建一个插件涉及两个部分——一个是在IntelliJ“前端”来显示用户界面的插件，另一个是在ReSharper“后端”来分析和使用C# PSI的插件。

Fortunately, many plugins can simply work with the ReSharper backend - Rider takes care of displaying the results of inspections and code completion, and many plugins can be written that don't require an IntelliJ UI component. More details can be found in the Product Specific section.

**CN:**  幸运的是，许多插件可以简单地与ReSharper后端一起工作——Rider负责显示检查和代码完成的结果，并且许多插件可以不需要IntelliJ UI组件就可以编写。更多的细节可以在产品特定部分找到。
