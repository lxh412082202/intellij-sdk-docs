---
title: Configuring Gradle for IntelliJ Platform Plugins——为IntelliJ平台插件配置Gradle
---

This page serves as a guide to Gradle-based plugin configuration for _IntelliJ Platform_ projects.
The IntelliJ IDEA Ultimate and Community editions bundle the Gradle and Plugin DevKit plugins to support Gradle-based development. 
The [Getting Started with Gradle](prerequisites.md) page provides a tutorial for creating Gradle-based IntelliJ Platform plugins.
It may be useful to review the IntelliJ Platform page, particularly the description of versioning in the [Open Source](/intro/intellij_platform.md#open-source) section.

**CN:**  这个页面可以作为 _IntelliJ Platform_ 项目基于级别的插件配置的指南。
         IntelliJ IDEA Ultimate和社区版将Gradle和插件DevKit插件捆绑在一起，以支持基于Gradle的开发。
         [Getting Started with Gradle](prerequisites.md)页面提供了创建基于渐变的IntelliJ平台插件的教程。
         回顾IntelliJ平台页面可能是有用的，特别是[Open Source](/intro/intellij_platform.md#open-source)部分对版本控制的描述。

> **WARNING** When adding additional repositories to your Gradle build script, make sure to always use HTTPS protocol.
>
>**CN:**  警告：当向Gradle构建脚本添加额外的存储库时，确保始终使用HTTPS协议。

* bullet list
{:toc}

## Overview of the IntelliJ IDEA Gradle Plugin——IntelliJ IDEA Gradle插件概述
The IntelliJ IDEA Gradle plugin is built from the open-source project `gradle-intellij-plugin`.
The plugin adds Gradle tasks for the `build.gradle` file that enable developing IntelliJ Platform plugins.
The [README](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md) file for the `gradle-intellij-plugin` project is the reference for configuring these tasks.

**CN:**  IntelliJ IDEA Gradle插件是由开源项目`gradle-intellij-plugin`开发的。
         该插件为`build.gradle`文件添加了Gradle任务，使开发IntelliJ平台插件成为可能。
         `gradle-intellij-plugin`项目的[README](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md)文件是配置这些任务的参考。

When getting started, there are several items to note on the README page:

**CN:**  在开始阅读时，在自述页面上有几项需要注意:

* At the top of the page, the [latest production version](https://github.com/JetBrains/gradle-intellij-plugin#the-latest-version) of the IntelliJ IDEA Gradle plugin is listed.

**CN:**  在页面的顶部，列出了IntelliJ IDEA Gradle插件的[latest production version](https://github.com/JetBrains/gradle-intellij-plugin#the-latest-version)。

* Also at the top is the minimum version of Gradle required to support the IntelliJ IDEA Gradle plugin.

**CN:**  同样在顶部的是支持IntelliJ IDEA Gradle插件所需的Gradle的最小版本。

* The table of extended Gradle [Tasks](https://github.com/JetBrains/gradle-intellij-plugin#tasks) has a succinct description for each task added by the plugin.
  This documentation will focus on the configuration and use four of those tasks:
  * [Setup DSL](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl) - `intellij { ... }`.
  * [Running DSL](https://github.com/JetBrains/gradle-intellij-plugin#running-dsl) - `runIde { ... }`
  * [Patching DSL](https://github.com/JetBrains/gradle-intellij-plugin#patching-dsl) - `patchPluginXml { ... }`
  * [Publishing DSL](https://github.com/JetBrains/gradle-intellij-plugin#publishing-dsl) - `publishPlugin { ... }`

**CN:** 
* 扩展的Gradle [Tasks](https://github.com/JetBrains/gradle-intellij-plugin#tasks)表格对插件添加的每个任务都有一个简洁的描述。
   本文档将重点介绍配置并使用其中四个任务:
  * [Setup DSL](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl) - `intellij { ... }`.
  * [Running DSL](https://github.com/JetBrains/gradle-intellij-plugin#running-dsl) - `runIde { ... }`
  * [Patching DSL](https://github.com/JetBrains/gradle-intellij-plugin#patching-dsl) - `patchPluginXml { ... }`
  * [Publishing DSL](https://github.com/JetBrains/gradle-intellij-plugin#publishing-dsl) - `publishPlugin { ... }`

* Examples are always a helpful resource, and at the bottom of the page are links to [example](https://github.com/JetBrains/gradle-intellij-plugin#examples) open source IntelliJ Platform plugin projects based on Gradle.

**CN:**  例子总是有用的资源，在页面的底部是基于Gradle的[example](https://github.com/JetBrains/gradle-intellij-plugin#examples)开源IntelliJ平台插件项目的链接。

* Almost every Gradle plugin attribute has a default value that will work to get started on a Gradle-based IntelliJ Platform plugin project.

**CN:**  几乎每个Gradle插件属性都有一个默认值，这个默认值可以用来启动一个基于Gradle的IntelliJ平台插件项目。


## Guide to Configuring Gradle Plugin Functionality——配置Gradle插件功能的指南
This section presents a guided tour of Gradle plugin attributes to achieve commonly desired functionality.

**CN:**  本节将介绍Gradle插件属性的向导，以实现通常需要的功能。

### Configuring the Gradle Plugin for Building IntelliJ Platform Plugin Projects——为构建IntelliJ平台插件项目配置Gradle插件
By default, the Gradle plugin will build a plugin project against the IntelliJ Platform defined by the latest EAP snapshot of the IntelliJ IDEA Community Edition.

**CN:**  默认情况下，Gradle插件会针对IntelliJ社区版最新EAP快照中定义的IntelliJ平台构建一个插件项目。

> **NOTE** Using EAP versions of the IntelliJ Platform requires adding the _Snapshots repository_ to the `build.gradle` file (see [IntelliJ Platform Artifacts Repositories](/reference_guide/intellij_artifacts.md)). 
>
>**CN:**  注意：使用IntelliJ平台的EAP版本需要将 _Snapshots repository_ 添加到`build.gradle`文件中(参见[IntelliJ Platform Artifacts Repositories](/reference_guide/intellij_artifacts.md))。


If a matching version of the specified IntelliJ Platform is not available on the local machine, the Gradle plugin downloads the correct version and type.
IntelliJ IDEA then indexes the build and any associated source code and JetBrains Java Runtime.

**CN:**  如果指定IntelliJ平台的匹配版本在本地机器上不可用，Gradle插件会下载正确的版本和类型。
         IntelliJ IDEA然后对构建和任何相关的源代码以及JetBrains Java运行时进行索引。


#### IntelliJ Platform Configuration——IntelliJ平台配置
Explicitly setting the [Setup DSL](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl) attributes `intellij.version` and `intellij.type` tells the Gradle plugin to use that configuration of the IntelliJ Platform to build the plugin project.
If a local installation of IntelliJ IDEA is the desired type and version of the IntelliJ Platform, use `intellij.localPath` to point to that installation.
If the `intellij.localPath` attribute is set, do not set the `intellij.version` and `intellij.type` attributes as this could result in undefined behavior.

**CN:**  显式地设置[Setup DSL](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl)属性`intellij.version`和`intellij.type`告诉Gradle插件使用IntelliJ平台的配置来构建插件项目。
         如果IntelliJ IDEA的本地安装是IntelliJ平台的期望类型和版本，那么使用`intellij.localPath`来指向该安装。
         如果设置了`intellij.localPath`属性，不要设置`intellij.version`和`intellij.type`属性，因为这会导致未定义的行为。

#### Plugin Dependencies——插件依赖
IntelliJ Platform plugin projects may depend on either bundled or third-party plugins.
In that case, a project should build against a version of those plugins that match the IntelliJ Platform version used to build the plugin project.
The Gradle plugin will fetch any plugins in the list defined by `intellij.plugins`.
See the Gradle plugin [README](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl) for information about specifying the plugin and version.

**CN:**  IntelliJ平台插件项目可以依赖于绑定的插件，也可以依赖于第三方插件。
         在这种情况下，一个项目应该针对那些与用于构建插件项目的IntelliJ平台版本相匹配的插件版本来构建。
         Gradle插件将获取由`intellij.plugins`定义的列表中的任何插件。
         有关指定插件和版本的信息，请参阅Gradle plugin [README](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl)。

Note that this attribute describes a dependency so the Gradle plugin can fetch the required artifacts.
The IntelliJ Platform plugin project is still required to declare these dependencies in its [Plugin Configuration](/basics/plugin_structure/plugin_configuration_file.md) (`plugin.xml`) file.

**CN:**  请注意，该属性描述了一个依赖项，因此Gradle插件可以获取所需的工件。
         IntelliJ平台插件项目仍然需要在其[Plugin Configuration](/basics/plugin_structure/plugin_configuration_file.md) (`plugin.xml`)文件中声明这些依赖关系。

### Configuring the Gradle Plugin for Running IntelliJ Platform Plugin Projects——为运行IntelliJ平台插件项目配置Gradle插件
By default, the Gradle plugin will use the same version of the IntelliJ Platform for the IDE Development Instance as was used for building the plugin.
Using the corresponding JetBrains Runtime is also the default, so for this use case no further configuration is required.

**CN:**  默认情况下，Gradle插件将使用与构建插件相同版本的IDE开发实例IntelliJ平台。
         使用相应的JetBrains运行时也是默认的，所以对于这个用例不需要进一步的配置。

#### Running Against Alternate Versions and Types of IntelliJ Platform-Based IDEs——在基于IntelliJ平台的IDE的替代版本和类型上运行
The IntelliJ Platform IDE used for the Development Instance can be different from that used to build the plugin project.
Setting the [Running DSL](https://github.com/JetBrains/gradle-intellij-plugin#running-dsl) attribute `runIde.ideDirectory` will define an IDE to be used for the Development Instance.
This attribute is commonly used when running or debugging a plugin in an [alternate IntelliJ Platform-based IDE](/intro/intellij_platform.md#ides-based-on-the-intellij-platform).

**CN:**  用于开发实例的IntelliJ平台IDE可以与用于构建插件项目的IDE不同。
         设置[Running DSL](https://github.com/JetBrains/gradle-intellij-plugin#running-dsl)属性`runIde.ideDirectory`将定义一个用于开发实例的IDE。
         此属性通常用于在[alternate IntelliJ Platform-based IDE](/intro/intellij_platform.md#ides-based-on-the-intellij-platform)中运行或调试插件。

#### Running Against Alternate Versions of the JetBrains Runtime——运行在JetBrains运行时的替代版本上
Every version of the IntelliJ Platform has a corresponding version of the [JetBrains Runtime](/basics/ide_development_instance.md#using-a-jetbrains-runtime-for-the-development-instance).
A different version of the runtime can be used by specifying the `runIde.jbrVersion` attribute, describing a version of the JetBrains Runtime that should be used by the IDE Development Instance.
The Gradle plugin will fetch the specified JetBrains Runtime as needed.

**CN:**  IntelliJ平台的每个版本都有一个相应的[JetBrains Runtime](/basics/ide_development_instance.md#using-a-jetbrains-runtime-for-the-development-instance)版本。
         可以通过指定`runIde.jbrVersion`属性来使用不同版本的运行时，该属性描述了应该由IDE开发实例使用的JetBrains运行时的版本。
         Gradle插件会根据需要获取指定的JetBrains运行时。

### Managing Directories used by the Gradle Plugin——管理Gradle插件使用的目录
There are several attributes to control where the Gradle plugin places directories for downloads and for use by the IDE Development Instance.

**CN:**  有几个属性可以控制Gradle插件放置目录的位置，供下载和IDE开发实例使用。

The location of the [sandbox home](/basics/ide_development_instance.md#sandbox-home-location-for-gradle-based-plugin-projects) directory and its subdirectories can be controlled with Gradle plugin attributes.
The `intellij.sandboxDirectory` attribute is used to set the path for the sandbox directory to be used while running the plugin in an IDE Development Instance. 
Locations of the sandbox [subdirectories](/basics/ide_development_instance.md#development-instance-settings-caches-logs-and-plugins) can be controlled using the `runIde.configDirectory`, `runIde.pluginsDirectory`, and `runIde.systemDirectory` attributes.
If the `intellij.sandboxDirectory` path is explicitly set, the subdirectory attributes default to the new sandbox directory.

**CN:**  [sandbox home](/basics/ide_development_instance.md#sandbox-home-location-for-gradle-based-plugin-projects)目录及其子目录的位置可以通过Gradle插件属性来控制。
         `intellij.sandboxDirectory`属性用于设置在IDE开发实例中运行插件时要使用的沙箱目录的路径。
         沙箱[subdirectories](/basics/ide_development_instance.md#development-instance-settings-caches-logs-and-plugins)的位置可以使用`runIde.configDirectory`、`runIde.pluginsDirectory`和`runIde.systemDirectory`属性进行控制。
         如果显式地设置了`intellij.sandboxDirectory`路径，则子目录属性默认为新沙箱目录。

The storage location of downloaded IDE versions and components defaults to the Gradle cache directory.
However, it can be controlled by setting the `intellij.ideaDependencyCachePath` attribute.

**CN:**  下载的IDE版本和组件的存储位置默认为Gradle缓存目录。
         但是，可以通过设置`intellij.ideaDependencyCachePath`属性来控制它。

### Controlling Downloads by the Gradle Plugin——控制下载的Gradle插件
As mentioned in the section about [configuring the intellij platform](#configuring-the-gradle-plugin-for-building-intellij-platform-plugin-projects) used for building plugin projects, the Gradle plugin will fetch the version of the IntelliJ Platform specified by the default or by the `intellij` attributes.
Standardizing the versions of the Gradle plugin and Gradle system across projects will minimize the time spent downloading versions.
There are controls for managing the IntelliJ IDEA Gradle plugin version, and the version of Gradle itself.
The Gradle plugin version is defined in the `plugins {}` section of a project's `build.gradle` file.
The Gradle version is in defined in a project's `gradle-wrapper.properties`.

**CN:**  如本节所述，用于构建插件项目的[configuring the intellij platform](#configuring-the-gradle-plugin-for-building-intellij-platform-plugin-projects), Gradle插件将获取默认或`intellij`属性指定的IntelliJ平台的版本。
         跨项目标准化Gradle插件和Gradle系统的版本可以减少下载版本的时间。
         有管理IntelliJ IDEA Gradle插件版本和Gradle本身版本的控件。
         Gradle插件版本是在项目的`build.gradle`文件的`plugins {}`部分定义的。
         Gradle版本是在一个项目的`gradle-wrapper.properties`中定义的。

### Patching the Plugin Configuration File——修补插件配置文件
A plugin project's `plugin.xml` file has element values that are "patched" at build time from the attributes of the `patchPluginXml` ([Patching DSL](https://github.com/JetBrains/gradle-intellij-plugin#patching-dsl)) task. 
As many as possible of the attributes in the Patching DSL will be substituted into the corresponding element values in a plugin project's `plugin.xml` file:

**CN:**  插件项目的`plugin.xml`文件的元素值是在构建时根据`patchPluginXml` ([Patching DSL](https://github.com/JetBrains/gradle-intellij-plugin#patching-dsl))任务的属性“patched”的。
         修补DSL中的尽可能多的属性将被替换到插件项目的`plugin.xml`文件中的相应元素值中:

* If a `patchPluginXml` attribute default value is defined, the attribute value will be patched in plugin.xml _regardless of whether the `patchPluginXml` task appears in the `build.gradle` file_.
  * For example, the default values for the attributes `patchPluginXml.sinceBuild` and `patchPluginXml.untilBuild` are defined based on the declared (or default) value of `intellij.version`.
    So by default `patchPluginXml.sinceBuild` and `patchPluginXml.untilBuild` are substituted into the `<idea-version>` element's `since-build` and `until-build` attributes in the `plugin.xml` file.

**CN:**  
* 如果定义了`patchPluginXml`属性默认值，属性值将在plugin.xml  _regardless of whether the `patchPluginXml` task appears in the `build.gradle` file_ 中打补丁。
  * 例如，属性`patchPluginXml.sinceBuild`和`patchPluginXml.untilBuild`的默认值是基于`intellij.version`的声明值(或默认值)定义的。
因此，在缺省情况下，将`patchPluginXml.sinceBuild`和`patchPluginXml.untilBuild`替换为`plugin.xml`文件中`<idea-version>`元素的`since-build`和`until-build`属性。

* If a `patchPluginXml` attribute value is explicitly defined, the attribute value will be substituted in `plugin.xml`.
  * If both `patchPluginXml.sinceBuild` and `patchPluginXml.untilBuild` attributes are explicitly set, both are substituted in `plugin.xml`. 
  * If one attribute is explicitly set (e.g. `patchPluginXml.sinceBuild`) and one is not (e.g. `patchPluginXml.untilBuild` has default value,) both attributes are patched at their respective (explicit and default) values.

**CN:**  
* 如果明确定义了`patchPluginXml`属性值，则属性值将被替换到`plugin.xml`中。
  * 如果显式地设置了`patchPluginXml.sinceBuild`和`patchPluginXml.untilBuild`属性，则在`plugin.xml`中替换它们。
  * 如果一个属性是显式设置的(例如`patchPluginXml.sinceBuild`)，而另一个属性不是(例如`patchPluginXml.untilBuild`有默认值)，则两个属性都按各自的值(显式和默认)打补丁。

* For **no substitution** of the `<idea-version>` element's `since-build` and `until-build` attributes, one of the following must appear in the `build.gradle` file:
  * Either set `intellij.updateSinceUntilBuild = false`, which will disable substituting both `since-build` and `until-build` attributes,
  * Or, for independent control, set `patchPluginXml.sinceBuild(null)` and `patchPluginXml.untilBuild(null)` depending on whether the intention is to disable one or both substitutions. 

**CN:**  
* 对于`<idea-version>`元素的`since-build`和`until-build`属性的 **no substitution** , `build.gradle`文件中必须出现以下内容之一:
  * 设置`intellij.updateSinceUntilBuild = false`，将禁用替换`since-build`和`until-build`属性，
  * 或对于独立控制，设置`patchPluginXml.sinceBuild(null)`和`patchPluginXml.untilBuild(null)`取决于意图是禁用一个或两个替换。


A best practice to avoid confusion is to replace the elements in `plugin.xml` that will be patched by the Gradle plugin with a comment.
That way the values for these parameters do not appear in two places in the source code.
The Gradle plugin will add the necessary elements as part of the patching process.
For those `patchPluginXml` attributes that contain descriptions such as `changeNotes` and `pluginDescription`, a `CDATA` block is not necessary when using HTML elements.

**CN:**  避免混淆的最佳实践是用注释替换将由Gradle插件修补的`plugin.xml`中的元素。
         这样，这些参数的值就不会出现在源代码的两个地方。
         Gradle插件将添加必要的元素作为补丁过程的一部分。
         对于那些包含`changeNotes`和`pluginDescription`等描述的`patchPluginXml`属性，在使用HTML元素时不需要`CDATA`块。


As discussed in [Components of a Wizard-Generated Gradle IntelliJ Platform Plugin](prerequisites.md#components-of-a-wizard-generated-gradle-intellij-platform-plugin), the Gradle properties `project.version`, `project.group`, and `rootProject.name` are all generated based on the input to the Wizard.
However, the IntelliJ IDEA Gradle plugin does not combine and substitute those Gradle properties for the default `<id>` and `<name>` elements in the `plugin.xml` file.

**CN:**  正如在[Components of a Wizard-Generated Gradle IntelliJ Platform Plugin](prerequisites.md#components-of-a-wizard-generated-gradle-intellij-platform-plugin)中讨论的，Gradle属性`project.version`、`project.group`和`rootProject.name`都是根据向导的输入生成的。
         然而，IntelliJ IDEA Gradle插件并没有将这些Gradle属性组合起来并替换为`plugin.xml`文件中默认的`<id>`和`<name>`元素。


The best practice is to keep `project.version` current. 
By default, if you modify `project.version` in `build.gradle`, the Gradle plugin will automatically update the `<version>` value in the `plugin.xml` file. 
This practice keeps all version declarations synchronized.

**CN:**  最佳实践是让`project.version`保持最新。
         默认情况下，如果你在`build.gradle`中修改`project.version`, Gradle插件会自动更新`plugin.xml`文件中的`<version>`值。
         这种做法使所有版本声明保持同步。


### Publishing with the Gradle Plugin——发布使用Gradle的插件
Please review the [Publishing Plugins with Gradle](deployment.md) page before using the [Publishing DSL](https://github.com/JetBrains/gradle-intellij-plugin#publishing-dsl) attributes.
That documentation explains three different ways to use Gradle for plugin uploads without exposing account credentials.

**CN:**  请在使用[Publishing DSL](https://github.com/JetBrains/gradle-intellij-plugin#publishing-dsl)属性之前查看[Publishing Plugins with Gradle](deployment.md)页面。
         该文档解释了使用Gradle进行插件上传的三种不同的方法，而不用暴露帐户凭证。


## Common Gradle Plugin Configurations for Development——用于开发的通用Gradle插件配置
Different combinations of Gradle plugin attributes are needed to create the desired build or IDE Development Instance environment.
This section reviews some of the more common configurations.

**CN:**  创建所需的构建或IDE开发实例环境需要不同的Gradle插件属性组合。
         本节将回顾一些更常见的配置。


### Plugins Targeting IntelliJ IDEA——以IntelliJ IDEA为目标的插件
IntelliJ Platform plugins targeting IntelliJ IDEA have the most straightforward Gradle plugin configuration.

**CN:**  IntelliJ平台上针对IntelliJ IDEA的插件有最直接的Gradle插件配置。

* Determine the version of [IntelliJ IDEA to use for building the plugin project](#configuring-the-gradle-plugin-for-building-intellij-platform-plugin-projects); this is the desired version of the IntelliJ Platform.
  This can be EAP (default) or determined from the [build number ranges](/basics/getting_started/build_number_ranges.md). 
  * If a production version of IntelliJ IDEA is the desired target, set the `intellij` [version attributes](#intellij-platform-configuration) accordingly. 
  * Set the necessary [plugin dependencies](#plugin-dependencies), if any. 

**CN:**  
* 确定[IntelliJ IDEA to use for building the plugin project](#configuring-the-gradle-plugin-for-building-intellij-platform-plugin-projects)的版本;这是IntelliJ平台的理想版本。
这可以是EAP(默认值)，也可以由[build number ranges](/basics/getting_started/build_number_ranges.md)决定。
  * 如果IntelliJ IDEA的一个产品版本是你想要的目标，那么相应的设置`intellij` [version attributes](#intellij-platform-configuration)。
  * 设置必要的[plugin dependencies](#plugin-dependencies)(如果有的话)。

* If the plugin project should be run or debugged in an IDE Development Instance based on the same IntelliJ IDEA version, no further attributes need to be set for the IDE Development Instance. 
  This is the default behavior and is the most common use case. 
  * If the plugin project should be run or debugged in an IDE Development Instance based on an alternate version of the IntelliJ Platform, set the [Running](#running-against-alternate-versions-and-types-of-intellij-platform-based-ides) DSL attribute accordingly.
  * If the plugin project should be run using a JetBrains Runtime other than the default for the IDE Development Instance, specify the [JetBrains Runtime version](#running-against-alternate-versions-of-the-jetbrains-runtime). 

**CN:**  
* 如果插件项目应该在基于相同IntelliJ IDEA版本的IDE开发实例中运行或调试，则不需要为IDE开发实例设置更多的属性。
这是默认的行为，也是最常见的用例。
  * 如果插件项目应该在基于IntelliJ平台另一个版本的IDE开发实例中运行或调试，那么相应地设置相应的[Running](#running-against-alternate-versions-and-types-of-intellij-platform-based-ides) DSL属性。
  * 如果插件项目应该使用JetBrains运行时而不是IDE开发实例的默认运行时来运行，指定[JetBrains Runtime version](#running-against-alternate-versions-of-the-jetbrains-runtime)。

* Set the appropriate attributes for [patching the `plugin.xml` file](#patching-the-plugin-configuration-file).

**CN:**  为[patching the `plugin.xml` file](#patching-the-plugin-configuration-file)设置适当的属性。

### Plugins Targeting Alternate IntelliJ Platform-Based IDEs——针对可替换的基于IntelliJ平台的IDE的插件
Gradle also supports developing plugins to run in IDEs that are based on the IntelliJ Platform, but are not IntelliJ IDEA.
For more information, see the [Developing for Multiple Products](/products/dev_alternate_products.md) page of this guide.

**CN:**  Gradle还支持开发运行在基于IntelliJ平台的ide上的插件，但这些插件并不是IntelliJ的理念。
         有关更多信息，请参见本指南的[Developing for Multiple Products](/products/dev_alternate_products.md)页面。