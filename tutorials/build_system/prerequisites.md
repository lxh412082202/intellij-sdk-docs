---
title: Getting Started with Gradle——开始使用Gradle
---

Gradle is the preferred solution for creating IntelliJ Platform plugins.
The IntelliJ IDEA Ultimate and Community editions bundle the necessary plugins to support Gradle-based development.
These IntelliJ IDEA plugins are _Gradle_ and _Plugin DevKit_, which are enabled by default.
To verify these plugins are installed and enabled, see the help section about [Managing Plugins](https://www.jetbrains.com/help/idea/managing-plugins.html).

**CN:**  Gradle是创建IntelliJ平台插件的首选解决方案。
         IntelliJ IDEA Ultimate和Community版本都捆绑了必要的插件来支持基于层次的开发。
         这些IntelliJ IDEA插件是 _Gradle_ 和 _Plugin DevKit_，它们是默认启用的。
         要验证这些插件是否已安装并启用，请参阅有关[Managing Plugins](https://www.jetbrains.com/help/idea/managing-plugins.html)的帮助部分。

> **WARNING** When adding additional repositories to your Gradle build script, make sure to always use HTTPS protocol.
>
>**CN:**  警告-当向Gradle构建脚本添加额外的存储库时，确保始终使用HTTPS协议。


* bullet list
{:toc}

## Creating a Gradle-Based IntelliJ Platform Plugin with New Project Wizard——使用新建项目向导创建一个基于Gradle的IntelliJ平台插件
IntelliJ IDEA supports creating new Gradle-based IntelliJ Platform plugin projects using the [New Project Wizard](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle).
The Wizard creates all the necessary project files based on a few template inputs.

**CN:**  IntelliJ IDEA支持使用[New Project Wizard](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle)创建新的基于Gradle的IntelliJ平台插件项目。
         向导根据一些模板输入创建所有必要的项目文件。


Before creating a new Gradle project, familiarize yourself with the IntelliJ IDEA help topic [Creating a new Gradle project](https://www.jetbrains.com/help/idea/getting-started-with-gradle.html#create_gradle_project), which is a tutorial for creating general Gradle projects in IntelliJ IDEA.
This page emphasizes the steps in the process for creating IntelliJ Platform plugin projects that are Gradle-based.

**CN:**  在创建一个新的Gradle项目之前，先要熟悉IntelliJ IDEA帮助主题[Creating a new Gradle project](https://www.jetbrains.com/help/idea/getting-started-with-gradle.html#create_gradle_project)，这是一个在IntelliJ IDEA中创建通用Gradle项目的教程。
         本页重点介绍创建基于Gradle的IntelliJ平台插件项目的步骤。


Launch the [New Project Wizard](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle).
It guides you through the Gradle project creation process with four screens.

**CN:**  启动[New Project Wizard](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle)。
         它通过四个页面指导您完成Gradle项目的创建过程。


### New Project Configuration Screen——新建项目配置页面
On the first screen, the type of project is configured:

**CN:**  在第一个页面，配置了项目类型:

* From the _product category_ pane on the left, choose _Gradle_.

**CN:**  从左边的 _product category_ 窗格中，选择 _Gradle_。

* Specify the _Project SDK_ based on the Java 8 JDK.
  This SDK will be the default JRE used to run Gradle, and the JDK version used to compile the plugin Java source.
  Based on the Project SDK, the IntelliJ IDEA Gradle Plugin will download the corresponding version of the IntelliJ Platform-based IDE automatically.

**CN:**  基于Java 8 JDK指定 _Project SDK_。
         这个SDK将是用于运行Gradle的默认JRE，以及用于编译插件Java源代码的JDK版本。
         基于Project SDK, IntelliJ IDEA Gradle插件会自动下载相应版本的基于IntelliJ平台的IDE。

* In the _Additional Libraries and Frameworks_ panel, select _Java_ and _IntelliJ Platform Plugin_.
  These settings will be used for the remainder of this tutorial.

**CN:**  在 _Additional Libraries and Frameworks_ 面板中，选择 _Java_ 和 _IntelliJ Platform Plugin_。
         这些设置将在本教程的其余部分中使用。

* Optionally:
  * To include support for the Kotlin language in the plugin, check the _Kotlin/JVM_ box (circled in green below.)
    This option can be selected with or without the _Java_ language.
  * To create the `build.gradle` file as a Kotlin build script rather than Groovy, check the _Kotlin DSL build script_ box (circled in magenta below.)

**CN:**  
* 可选:
    * 要在插件中包含对Kotlin语言的支持，请选中 _Kotlin/JVM_ 框(下面用绿色圈出)。
可以使用或不使用 _Java_ 语言选择此选项。
    * 要将`build.gradle`文件创建为Kotlin构建脚本而不是Groovy，请选中 _Kotlin DSL build script_ 框(下面用洋红色圈出)。

Then click _Next_:

**CN:**  然后点击 _Next_：


![Select the Gradle facet in the Project Creation Wizard](img/step1_new_gradle_project.png){:width="800px"}

### Project Naming Screen——项目命名页面
On this, the second screen of the Wizard, specify a [Group ID, Artifact ID, and plugin Version](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle) using [Maven naming](https://maven.apache.org/guides/mini/guide-naming-conventions.html) conventions.

**CN:**  在向导的第二个页面上，使用[Maven naming](https://maven.apache.org/guides/mini/guide-naming-conventions.html)约定指定一个[Group ID, Artifact ID, and plugin Version](https://www.jetbrains.com/help/idea/gradle.html#project_create_gradle)。

* _Group ID_ is typically a Java package name, and it is used for the Gradle property `project.group` value in the project's `build.gradle` file.
  For this example, enter "com.your.company".

**CN:**   _Group ID_ 通常是一个Java包名，它用于项目的`build.gradle`文件中的Gradle属性`project.group`值。
         对于本例，输入“com.your.company”。

* _Artifact ID_ is the default name of the project JAR file (without version).
  It is also used for the Gradle property `rootProject.name` value in the project's `settings.gradle` file.
  For this example, enter "my_gradle_plugin".

**CN:**   _Artifact ID_ 是项目JAR文件的默认名称(没有版本)。
         也用于项目`settings.gradle`文件中的Gradle属性`rootProject.name`值。
         对于这个例子，输入“my _gradle_ plugin”。

* _Version_ is used for the Gradle property `project.version` value in the `build.gradle` file. 
  For this example, enter "1.0".

**CN:**   _Version_ 用于`build.gradle`文件中的Gradle属性`project.version`值。
         对于本例，输入“1.0”。


Click _Next_ to continue. 

**CN:**  单击 _Next_ 继续。


### Gradle Settings Screen——Gradle设置页面
The third screen prompts for Gradle-specific settings. 
All of these settings can be changed once the project is created via **Settings \| Build, Execution, Deployment \| Build Tools \| Gradle**. 
Select the [default settings](https://www.jetbrains.com/help/idea/gradle-settings.html) and click _Next_ to continue.

**CN:**  第三个页面提示进行Gradle特定的设置。
         一旦通过 **Settings | Build, Execution, Deployment | Build Tools | Gradle** 创建项目，所有这些设置都可以更改。
         选择[default settings](https://www.jetbrains.com/help/idea/gradle-settings.html)并单击 _Next_ 继续。


### Project Name and Location Screen——项目名称及路径页面
The final Wizard screen is for setting the [project name and location](https://www.jetbrains.com/help/idea/project-name-and-location.html#Project_Name_and_Location.xml).
The _Project name_ is pre-filled with the [Artifact ID](#project-naming-screen).

**CN:**  最后一个向导屏幕用于设置[project name and location](https://www.jetbrains.com/help/idea/project-name-and-location.html#Project_Name_and_Location.xml)。
          _Project name_ 是用[Artifact ID](#project-naming-screen)填写的。


Note the choice of _Project format_ under _More Settings_ is somewhat superfluous.
Although an `.idea` directory or `.ipr` file is generated as the project is created and built, it is Gradle and the IntelliJ IDEA Gradle plugin that control many aspects of the project.  

**CN:**  注意在 _More Settings_ 下选择 _Project format_ 有点多余。
         虽然`.idea`目录或`.ipr`文件是在项目创建和构建时生成的，但是控制项目许多方面的却是Gradle和IntelliJ IDEA Gradle插件。


Click _Finish_.

**CN:**  点击 _Finish_。


### Components of a Wizard-Generated Gradle IntelliJ Platform Plugin——向导生成的Gradle IntelliJ平台插件的组件
For the [example](#creating-a-gradle-based-intellij-platform-plugin-with-new-project-wizard) `my_gradle_plugin`, the New Project Wizard creates the directory content shown below:

**CN:**  对于[example(示例)](#creating-a-gradle-based-intellij-platform-plugin-with-new-project-wizard) `my_gradle_plugin`，新建项目向导将创建如下所示的目录内容:

* The default IntelliJ Platform `build.gradle` file.
  The contents are further discussed below.

**CN:**  默认的IntelliJ平台`build.gradle`文件。
  内容将在下面进一步讨论。

* The Gradle Wrapper files, and in particular the `gradle-wrapper.properties` file, which specifies the version of Gradle to be used to build the plugin.
  If needed, the IntelliJ IDEA Gradle plugin will download the version of Gradle specified in this file.

**CN:**  Gradle包装器文件，特别是`gradle-wrapper.properties`文件，它指定了用于构建插件的Gradle版本。
  如果需要，IntelliJ IDEA Gradle插件会下载该文件中指定的Gradle版本。

* The `settings.gradle` file, containing a definition of the `rootProject.name`.

**CN:**  `settings.gradle`文件，包含`rootProject.name`的定义。

* The `META-INF` directory under the default `main` [SourceSet](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout) contains the plugin [configuration file](/basics/plugin_structure/plugin_configuration_file.md).

**CN:**  默认`main` [SourceSet](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout)下的`META-INF`目录包含插件[configuration file](/basics/plugin_structure/plugin_configuration_file.md)。


```text
my_gradle_plugin
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   └── resources
    │       └── META-INF
    │           └── plugin.xml
    └── test
        ├── java
        └── resources
```

The New Project Wizard produces the `my_gradle_plugin` project `build.gradle` file shown below:

**CN:**  新建项目向导生成如下所示的`my_gradle_plugin`项目`build.gradle`文件:

* There is no explicit `buildscript{}` in the file.
  The IntelliJ IDEA Gradle plugin dynamically creates a `buildscript{}`.

**CN:**  文件中没有显式的`buildscript{}`。
         IntelliJ IDEA Gradle插件动态创建了一个`buildscript{}`。

* Two plugins to Gradle are explicitly declared:
  * The [Gradle Java](https://docs.gradle.org/current/userguide/java_plugin.html) plugin.
  * The [IntelliJ IDEA Gradle plugin](https://github.com/JetBrains/gradle-intellij-plugin/).

**CN:** 
* Gradle的两个插件被明确声明:
  * [Gradle Java](https://docs.gradle.org/current/userguide/java_plugin.html)插件。
  * [IntelliJ IDEA Gradle plugin](https://github.com/JetBrains/gradle-intellij-plugin/)。

* The _Group ID_ from the Wizard [Project Naming Screen](#project-naming-screen) is the `project.group` value.

**CN:**  向导[Project Naming Screen](#project-naming-screen)中的 _Group ID_ 是`project.group`值。

* The _Version_ from the Wizard [Project Naming Screen](#project-naming-screen) is the `project.version` value.

**CN:**  向导[Project Naming Screen](#project-naming-screen)中的 _Version_ 是`project.version`值。

* The `sourceCompatibility` line is injected to enforce using Java 8 JDK to compile Java source.

**CN:**  `sourceCompatibility`行被注入来强制使用Java 8 JDK来编译Java源代码。

* The only comment in the file is a link to the [README.md](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md) for the IntelliJ IDEA Gradle plugin, which is a reference for the DSLs defined by the plugin.

**CN:**  文件中唯一的注释是IntelliJ IDEA Gradle插件的[README.md](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md)链接，它是插件定义的DSL的参考。

* The value of the Setup DSL attribute `intellij.version` specifies the version of the IntelliJ Platform to be used to build the plugin.
  It defaults to the version of IntelliJ IDEA that was used to run the New Project Wizard.

**CN:**  Setup DSL属性`intellij.version`的值指定用于构建插件的IntelliJ平台的版本。
         它默认使用运行新项目向导的IntelliJ IDEA版本。

* The value of the Patching DSL attribute `patchPluginXml.changeNotes` is set to place holder text.

**CN:**  修补DSL属性`patchPluginXml.changeNotes`的值被设置为place holder文本。


```groovy
  plugins {
      id 'java'
      id 'org.jetbrains.intellij' version '0.4.15'
  }
  
  group 'com.your.company'
  version '1.0' 
  sourceCompatibility = 1.8
  
  repositories {
      mavenCentral()
  } 
  dependencies {
      testCompile group: 'junit', name: 'junit', version: '4.12'
  }
  
  // See https://github.com/JetBrains/gradle-intellij-plugin/
  intellij {
      version '2019.1'
  }
  patchPluginXml {
      changeNotes """
        Add change notes here.<br>
        <em>most HTML tags may be used</em>"""
  }
```

#### Plugin Gradle Properties and Plugin Configuration File Elements——插件Gradle属性和插件配置文件元素
The Gradle properties `rootProject.name` and `project.group` will not, in general, match the respective `plugin.xml` elements `<name>` and `<id>`.
There is no IntelliJ Platform-related reason they should as they serve different functions.
The `<name>` element is often similar to the content root, but is more explanatory than the `rootProject.name`.
The `<id>` is a unique identifier over all plugins, typically a concatenation of the Maven `groupId` and `artifactId`; the default Gradle `project.group` property is only the `groupId`.

**CN:**  一般来说，Gradle的属性`rootProject.name`和`project.group`不会与相应的`plugin.xml`元素`<name>`和`<id>`相匹配。
         没有智能平台相关的理由，因为它们有不同的功能。
         `<name>`元素通常与内容根类似，但比`rootProject.name`更具有解释性。
         `<id>`是所有插件的唯一标识符，通常是Maven `groupId`和`artifactId`的组合;默认的Gradle `project.group`属性只是`groupId`。



## Adding Gradle Support to an Existing DevKit-Based IntelliJ Platform Plugin——添加Gradle支持到现有的基于devkit的IntelliJ平台插件
Converting a DevKit-based plugin project to a Gradle-based plugin project can be done using the New Project Wizard to create a Gradle-based project around the existing DevKit-based project: 

**CN:**  将一个基于devkit的插件项目转换成一个基于等级的插件项目，可以使用新建项目向导围绕现有的基于devkit的项目创建一个基于等级的项目:

* Ensure the directory containing the DevKit-based IntelliJ Platform plugin project can be fully recovered if necessary.

**CN:**  如果需要的话，确保包含基于devkit的IntelliJ平台插件项目的目录可以完全恢复。

* Delete all the artifacts of the DevKit-based project:
  * `.idea` directory
  * IML file
  * `out` directory

**CN:**  
* 删除devkit为基础项目中的所有工件:
  * `.idea`目录
  * IML文件
  * `out`目录

* Arrange the existing source files within the project directory in Gradle [SourceSet](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout) format. 

**CN:**  以Gradle [SourceSet](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_project_layout)格式整理项目目录中的现有源文件。

* Use the New Project Wizard as though creating a [new Gradle project](#creating-a-gradle-based-intellij-platform-plugin-with-new-project-wizard) from scratch.

**CN:**  使用新建项目向导，就像从零开始创建[new Gradle project](#creating-a-gradle-based-intellij-platform-plugin-with-new-project-wizard)一样。

* On the [Project Naming Screen](#project-naming-screen) set the values to:
  * GroupID to the existing package in the initial source set.
  * ArtifactID to the name of the existing plugin.
  * Version to the same as the existing plugin.

**CN:**  
* 在[Project Naming Screen](#project-naming-screen)上设置的值为:
  * GroupID到初始源集中的现有包。
  * ArtifactID到现有插件的名称。
  * 版本与现有的插件相同。

* On the [Project Name and Location Screen](#project-name-and-location-screen) set the values to:
  * _Project name_ to the name of the existing plugin. 
    (It should be pre-filled from the _ArtifactID_)
  * Set the _Project location_ to the directory of the existing plugin.

**CN:**  
* 在[Project Name and Location Screen](#project-name-and-location-screen)上设置的值为:
  * _Project name_ 到现有插件的名称。
(由 _ArtifactID_ 预填)
  * 将 _Project location_ 设置为现有插件的目录。


* Click _Finish_ to create the new Gradle-based plugin.

**CN:**  单击 _Finish_ 创建新的基于Gradle的插件。

* [Add more modules](https://www.jetbrains.com/help/idea/gradle.html#gradle_add_module) using Gradle [_Source Sets_](https://www.jetbrains.com/help/idea/gradle.html#gradle_source_sets) as needed.

**CN:**  [Add more modules](https://www.jetbrains.com/help/idea/gradle.html#gradle_add_module)根据需要使用Gradle的[_Source Sets_](https://www.jetbrains.com/help/idea/gradle.html#gradle_source_sets)。



## Running a Simple Gradle-Based IntelliJ Platform Plugin——运行一个简单的基于Gradle的IntelliJ平台插件
Gradle projects are run from the IDE's Gradle Tool window.

**CN:**  Gradle项目是从IDE的Gradle工具窗口运行的。


### Adding Code to the Project——向项目添加代码
Before running [`my_gradle_project`](#components-of-a-wizard-generated-gradle-intellij-platform-plugin), some code could be added to provide simple functionality.
See the [Creating Actions](/tutorials/action_system/working_with_custom_actions.md) tutorial for step-by-step instructions for adding a menu action. 

**CN:**  在运行[`my_gradle_project`](#components-of-a-wizard-generated-gradle-intellij-platform-plugin)之前，可以添加一些代码来提供简单的功能。
         有关添加菜单操作的详细说明，请参阅[Creating Actions](/tutorials/action_system/working_with_custom_actions.md)教程。


### Executing the plugin——执行插件
Open the Gradle tool window and search for the `runIde` task: 

**CN:**  打开Gradle工具窗口，搜索`runIde`任务:

* If it’s not in the list, hit the [Refresh](https://www.jetbrains.com/help/idea/jetgradle-tool-window.html#1eeec055) button at the top of the Gradle window. 

**CN:**  如果它不在列表中，点击Gradle窗口顶部的[Refresh](https://www.jetbrains.com/help/idea/jetgradle-tool-window.html#1eeec055)按钮。

* Or [Create a new Gradle Run Configuration](https://www.jetbrains.com/help/idea/create-run-debug-configuration-gradle-tasks.html).
  
**CN:**    或者[Create a new Gradle Run Configuration(创建一个新的Gradle运行配置)](https://www.jetbrains.com/help/idea/create-run-debug-configuration-gradle-tasks.html)。
  

![Gradle Tool Window](img/gradle_tasks_in_tool_window.png){:width="398px"}
   
Double-click on the _runIde_ task to execute it.
See the IntelliJ IDEA help for more information about [Working with Gradle tasks](https://www.jetbrains.com/help/idea/gradle.html#96bba6c3).

**CN:**  双击 _runIde_ 任务来执行它。
         有关[Working with Gradle tasks](https://www.jetbrains.com/help/idea/gradle.html#96bba6c3)的更多信息，请参阅IntelliJ IDEA帮助。


Finally, when `my_gradle_plugin` launches in the IDE development instance, there should be a new menu under the **Tools** menu. 

**CN:**  最后，当`my_gradle_plugin`在IDE开发实例中启动时，应该在 **Tools** 菜单下有一个新菜单。
