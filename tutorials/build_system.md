---
title: Building Plugins with Gradle——用Gradle建立插件
---

The [gradle-intellij-plugin](https://github.com/JetBrains/gradle-intellij-plugin) Gradle plugin is the recommended solution for building IntelliJ plugins. 
The plugin takes care of the dependencies of your plugin project - both the base IDE and other plugin dependencies.

**CN:**  Gradle插件是构建IntelliJ插件的推荐解决方案。插件负责处理你的插件项目的依赖关系——包括基本的IDE和其他的插件依赖关系。

If a new plugin will be Scala-based, a plugin development workflow [sbt-idea-plugin](https://github.com/JetBrains/sbt-idea-plugin), is available.
The workflow is analogous to the Gradle workflow but tailored to developing IntelliJ Platform plugins in Scala.
JetBrains does not officially support this Scala workflow, and at this time the workflow has only minimal documentation.

**CN:**  如果一个新的插件是基于scala的，那么可以使用一个插件开发工作流sbt-idea-plugin。该工作流程类似于Gradle工作流程，但是是为在Scala中开发IntelliJ平台插件而量身定做的。JetBrains并没有正式支持这个Scala工作流，而且目前这个工作流只有很少的文档。

The gradle-intellij-plugin provides tasks to run the IDE with your plugin and to publish your plugin to the [JetBrains plugins repository](/plugin_repository/index.md). 
To make sure that your plugin is not affected by [API changes](/reference_guide/api_changes_list.md) which may happen between major releases of the platform, you can easily build your plugin against many versions of the base IDE.

**CN:**  gradle-intellij-plugin提供了与您的插件一起运行IDE并将您的插件发布到JetBrains插件库的任务。为了确保你的插件不受API变化的影响，这些API变化可能发生在平台的主要版本之间，你可以很容易地构建你的插件来对抗基本IDE的许多版本。

> **WARNING** When adding additional repositories to your Gradle build script, make sure to always use HTTPS protocol.
>
>**CN:**  警告：当向Gradle构建脚本添加额外的存储库时，一定要始终使用HTTPS协议。

> **Note** Please make sure to always upgrade to the latest version of `gradle-intellij-plugin`.
Follow releases on [GitHub](https://github.com/JetBrains/gradle-intellij-plugin/releases). 
>
>**CN:**  注意：请务必经常升级到最新版本的gradle-intellij-plugin。关注GitHub上的发布。

The following tutorial refers to materials that can be found in the included [gradle_plugin_demo](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/gradle_plugin_demo) project. 
Below are a series of guides to developing and deploying Gradle-based IntelliJ Platform Plugins:  

**CN:**  下面的教程参考了可以在包含的gradle_plugin_demo项目中找到的材料。下面是一系列开发和部署基于级别的IntelliJ平台插件的指南:

*  [1. Getting Started with Gradle-Based Plugins](build_system/prerequisites.md)

**CN:**  基于Gradle插件入门

*  [2. Configuring Gradle-Based Plugins](build_system/gradle_guide.md)

**CN:**  基于Gradle插件配置

*  [3. Deploying a Plugin with Gradle](build_system/deployment.md)

**CN:**  使用Gradel开发一个插件