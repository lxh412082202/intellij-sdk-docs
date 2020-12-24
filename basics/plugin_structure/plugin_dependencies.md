---
title: Plugin Dependencies——插件依赖
---

A plugin may depend on classes from other plugins, either bundled, third-party, or by the same author.
This document describes the syntax for declaring plugin dependencies and optional plugin dependencies.
For more information about dependencies on the IntelliJ Platform modules, see Part II of this document: [Plugin Compatibility with IntelliJ Platform Products](/basics/getting_started/plugin_compatibility.md).

**CN:**  插件可能依赖于来自其他插件的类，可能是绑定的，也可能是第三方的，或者是同一作者的。
本文档描述了声明插件依赖项和可选插件依赖项的语法。
有关IntelliJ平台模块依赖关系的更多信息，请参阅本文档的第二部分:[Plugin Compatibility with IntelliJ Platform Products](/basics/getting_started/plugin_compatibility.md)。


 
To express dependencies on classes from other plugins or modules, perform the following three required steps:

**CN:**  要表达对来自其他插件或模块的类的依赖，需要执行以下三个步骤:

## 1. Preparing Sandbox——准备沙箱
If the plugin is not bundled with the target IDE, run the (sandbox) [IDE Development Instance](/basics/ide_development_instance.md) of your target IDE and install the plugin there.

**CN:**  如果插件没有与目标IDE绑定，运行目标IDE的(沙箱)[IDE Development Instance](/basics/ide_development_instance.md)并在那里安装插件。

## 2. Project Setup——项目设置
Depending on the chosen development workflow (Gradle or DevKit), one of the two following steps is necessary.

**CN:**  根据所选择的开发工作流程(Gradle或DevKit)，以下两个步骤自选其一。

### 2.1 Gradle
> **NOTE** Please see the `plugins` attribute [gradle-intellij-plugin: Configuration](https://github.com/JetBrains/gradle-intellij-plugin#configuration) for acceptable values.
>
>**CN:**  请查看`plugins`属性[gradle-intellij-plugin: Configuration](https://github.com/JetBrains/gradle-intellij-plugin#configuration)以获得可接受的值。

If the project is using [Gradle](/tutorials/build_system.md) with a Groovy build script to build the plugin, add the dependency to the `plugins` parameter of the `intellij` block in your build.gradle, for example:
如果项目使用[Gradle](/tutorials/build_system.md)和Groovy构建脚本来构建插件，那么在您的build.gradle中为 `intellij` 块的 `plugins` 参数添加依赖项，例如:

```groovy
intellij {
    plugins 'org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1'
}
```

If the project is using [Gradle](/tutorials/build_system.md) with a Kotlin build script to build the plugin, use `setPlugins()` within the `intellij` block, for example:

**CN:**  如果项目使用[Gradle](/tutorials/build_system.md)和Kotlin构建脚本来构建插件，在 `intellij` 块中使用 `setPlugins()` ，例如：

```kotlin
intellij {
        setPlugins("org.jetbrains.kotlin:1.3.11-release-IJ2018.3-1")
}
```

#### 2.2 DevKit
If the project is using [DevKit](/basics/getting_started/using_dev_kit.md), add the JARs of the plugin on which the project depends to the **classpath** of the *IntelliJ Platform SDK*.

**CN:**  如果项目使用 [DevKit](/basics/getting_started/using_dev_kit.md)，将项目所依赖的插件的jar添加到*IntelliJ Platform SDK*的**classpath**中。


> **WARNING** Do not add the plugin JARs as a library: this will fail at runtime because the IntelliJ Platform will load two separate copies of the dependency plugin classes.
>
>**CN:**  不要将插件jar作为库添加:这将在运行时失败，因为IntelliJ平台将加载依赖插件类的两个独立副本。

To do that, open the Project Structure dialog, select the SDK used in the project, press the + button in the Classpath tab, and
select the plugin JAR file or files:

**CN:**  为此，打开Project Structure对话框，选择项目中使用的SDK，按Classpath选项卡中的+按钮，然后
选择插件JAR文件或文件:

* For bundled plugins, the plugin JAR files are located in `plugins/<pluginname>` or `plugins/<pluginname>/lib` under the main installation directory.
  If you're not sure which JAR to add, you can add all of them.

* **CN:**  对于捆绑的插件，插件JAR文件位于主安装目录下的 `plugins/<pluginname>` 或 “plugins/<pluginname>/lib”中。
  如果您不确定要添加哪个JAR，您可以添加所有JAR。
  
* For non-bundled plugins, the plugin JAR files are located in `config/plugins/<pluginname>` or `config/plugins/<pluginname>/lib` under the directory specified as "Sandbox Home" in the IntelliJ Platform Plugin SDK settings.

* **CN:**  对于非绑定的插件，插件JAR文件位于IntelliJ Platform Plugin SDK设置中指定为"Sandbox Home"的目录下的`config/plugins/<pluginname>`或`config/plugins/<pluginname>/lib`中。

![Adding Plugin to Classpath](img/add_plugin_dependency.png)

## 3. Dependency Declaration in plugin.xml——plugin.xml中的依赖项声明
Regardless of whether a plugin project uses [Modules Available in All Products](/basics/getting_started/plugin_compatibility.md#modules-available-in-all-products), or [Modules Specific to Functionality](/basics/getting_started/plugin_compatibility.md#modules-specific-to-functionality), the correct module must be listed as a dependency in `plugin.xml`. 
If a project depends on another plugin, the dependency must be declared like a module.
If only general IntelliJ Platform features (APIs) are used, then a default dependency on `com.intellij.modules.platform` must be declared.
To display a list of available IntelliJ Platform modules, invoke the [code completion](https://www.jetbrains.com/help/idea/auto-completing-code.html#4eac28ba) feature for the `<depends>` element contents while editing the plugin project's `plugin.xml` file.

**CN:**  不管插件项目使用的是[Modules Available in All Products](/basics/getting_started/plugin_compatibility.md#modules-available-in-all-products)还是[Modules Specific to Functionality](/basics/getting_started/plugin_compatibility.md#modules-specific-to-functionality)，正确的模块必须在`plugin.xml`中作为依赖项列出。
如果一个项目依赖于另一个插件，依赖项必须像模块一样声明。
如果只使用一般的IntelliJ平台特性(APIs)，那么必须声明对`com.intellij.modules.platform`的默认依赖。
要显示可用IntelliJ平台模块的列表，可以在编辑插件项目的`plugin.xml`文件时调用`<depends>`元素内容的[code completion](https://www.jetbrains.com/help/idea/auto-completing-code.html#4eac28ba)特性。


### 3.1 Configuring plugin.xml——配置plugin.xml
In the `plugin.xml`, add a `<depends>` tag with the ID of the dependency plugin as its content.
Continuing with the example from [Section 2](#2-project-setup) above, the dependency declaration in `plugin.xml` would be:

**CN:**  在`plugin.xml`中，添加一个以依赖项插件的ID作为内容的`<depends>`标记。
继续上面[Section 2](#2-project-setup)的例子，`plugin.xml`中的依赖项声明是:

```xml
<depends>org.jetbrains.kotlin</depends>
```


## Optional Plugin Dependencies——可选插件依赖关系
A project can also specify an optional plugin dependency. In this case, the plugin will load even if the plugin it depends on
is not installed or enabled, but part of the functionality of the plugin will not be available. In order to do that,
add `optional="true" config-file="otherconfig.xml"` to the `<depends>` tag.

**CN:**  项目还可以指定一个可选的插件依赖项。在这种情况下，插件将加载，即使它所依赖的插件没有安装或启用，但是插件的部分功能将不可用。
为此，将`optional="true" config-file="otherconfig.xml"`添加到`<depends>`标签。

For example, if a plugin project adds additional highlighting for Java and Kotlin files, use the following setup. 
The main `plugin.xml` will define an annotator for Java and specify an optional dependency on the Kotlin plugin:

**CN:**  例如，如果插件项目为Java和Kotlin文件添加了额外的高亮显示，请使用以下设置。
主`plugin.xml`将为Java定义一个注释器，并在Kotlin插件上指定一个可选的依赖项:

```xml
<idea-plugin>
   ...
   <depends optional="true" config-file="withKotlin.xml">org.jetbrains.kotlin</depends>

   <extensions defaultExtensionNs="com.intellij">
      <annotator language="JAVA" implementationClass="com.example.MyJavaAnnotator"/>
   </extensions>
</idea-plugin>
```

Then create a file called `withKotlin.xml`, in the same directory as the main `plugin.xml` file. In that file, define an annotator for Kotlin:

**CN:**  然后在主`plugin.xml`文件所在的目录中创建一个名为`withKotlin.xml`的文件。在该文件中，为Kotlin定义一个注释器:

```xml
<idea-plugin>
   <extensions defaultExtensionNs="com.intellij">
      <annotator language="kotlin" implementationClass="com.example.MyKotlinAnnotator"/>
   </extensions>
</idea-plugin>
```
