---
title: About this Guide——关于本指南
---

This guide is split into several parts, similar to a text book. Each part builds on the content of the previous part, but it is not necessary to read the guide in order. The [Key Topics](key_topics.md) page aims to link to the pages that are necessary to be able to understand the architecture and get started building plugins.

**CN:**  本指南分为几个部分，类似于教科书。每个部分都以前一部分的内容为基础，但是没有必要按顺序阅读指南。[Key Topics](key_topics.md)页面的目的是链接到那些需要理解架构并开始构建插件的页面。

> **NOTE** While browsing this guide, you will notice that there are topics that are greyed out. Unfortunately, the guide is not complete and contains placeholders for certain topics. We are working on increasing the coverage, but if you get stuck due to missing content, please see the [Getting Help](getting_help.md) section for details on how to get moving again.
>
>**CN:**  注意：浏览本指南时，您会注意到有些主题是灰色的。遗憾的是，该指南并不完整，并且包含某些主题的占位符。我们正在努力增加覆盖面，但如果你因缺少内容而陷入困境，请参阅[Getting Help(获取帮助)](getting_help.md)部分，了解如何再次移动的详细信息。
>
> The guide is also [Open Source on GitHub](https://github.com/JetBrains/intellij-sdk-docs), and Pull Requests for new content or updates are always gratefully received. A Pull Request does not need to be fully comprehensive - if a little update would help you, it will help other developers too! All pull requests will be reviewed before being accepted, so don't worry about inaccuracies. Please see the [Contributing](/CONTRIBUTING.md) page for details on building the guide locally and contributing.
>
>**CN:**  该指南[Open Source on GitHub(在GitHub上也是开源的)](https://github.com/JetBrains/intellij-sdk-docs)，对新内容或更新的拉取请求总是会被欣然接受。拉请求不需要是全面的-如果一个小的更新会帮助你，它也会帮助其他开发人员!所有的拉请求在被接受之前都会被审查，所以不要担心不准确。有关在本地建立指南和投稿的详细信息，请参阅[Contributing(投稿)](/CONTRIBUTING.md)页面。

#### [Part I - Plugins](/basics.md)——第一部分-插件

Describes how to create a plugin that can extend the _IntelliJ Platform_. Includes details on how to set up the project, register extension points, target specific versions of the _IntelliJ Platform_, and how to package, deploy and test your plugins.

**CN:**  描述如何创建可以扩展IntelliJ平台的插件。包括如何设置项目、注册扩展点、目标IntelliJ平台的特定版本，以及如何打包、部署和测试你的插件等细节。

#### [Part II - Base Platform](/platform/fundamentals.md)——第二部分-基础平台

Describes the foundational layer of the architecture, which provides many features and utilities, such as the component model, the user interface, documents and editors, the virtual file system, settings and threading and background tasks. The Base Platform layer essentially comprises the functionality of the _IntelliJ Platform_ that does not target language features or parsing.

**CN:**  描述体系结构的基本层，它提供许多特性和实用程序，如组件模型、用户界面、文档和编辑器、虚拟文件系统、设置和线程以及后台任务。基本平台层本质上包含IntelliJ平台的功能，它不针对语言特性或解析。

#### [Part III - Project Model](/basics/project_structure.md)——第三部分-项目模块

Documents the Project Model, which represents the files and configuration of the currently loaded project, as well as the build system used to build the project.

**CN:**  记录项目模型，它表示当前加载的项目的文件和配置，以及用于构建项目的构建系统。

#### [Part IV - PSI](/basics/architectural_overview/psi.md)——第四部分-PSI

The Program Structure Interface builds the syntactic and semantic models for lots of different file types. This section describes how to work with the PSI, navigating and manipulating the syntax trees, and also looks at the powerful references system, which allows a syntax tree node to reference an item in the semantic model. It also details how the PSI creates and uses indexes.

**CN:**  程序结构接口为许多不同的文件类型建立语法和语义模型。本节介绍如何使用PSI，导航和操作语法树，并介绍强大的引用系统，该系统允许语法树节点引用语义模型中的一个项。它还详细说明了PSI如何创建和使用索引。

#### Part V - Features——第五部分-特性

Describes how to extend and interact with various features that use the PSI layer, such as code completion, navigation, <kbd>Alt</kbd>+<kbd>Enter</kbd> items, intentions, refactorings and more. See also the section on Custom Languages below for language specific features that are only applicable when adding support for a new language.

**CN:**  描述如何扩展和交互使用PSI层的各种功能，如代码完成、导航、Alt+输入项、意图、重构等。请参阅下面有关自定义语言的部分，了解仅在添加对新语言的支持时才适用的语言特定功能。

#### [Part VI - Testing](/basics/testing_plugins/testing_plugins.md)——第六部分-测试

Describes the available infrastructure for writing automated tests covering the functionality of plugins.

**CN:**  描述用于编写覆盖插件功能的自动化测试的可用基础结构。

#### [Part VII - Custom Languages](/reference_guide/custom_language_support.md)——第七部分-自定义语言

Plugins frequently extend support for existing languages, such as adding inspections to Java files. This section describes how to add support to the _IntelliJ Platform_ for a new language, that isn't supported by default, creating parsers, syntactic and semantic models and all the features that build on top.

**CN:**  插件经常扩展对现有语言的支持，例如将检查添加到Java文件中。本节描述如何为一种默认不支持的新语言添加IntelliJ平台支持，创建解析器、语法和语义模型以及所有在其上构建的特性。

#### [Part VIII - Product Specific](/products/idea.md)——第八部分-特定产品

A lot of the functionality in the _IntelliJ Platform_ is language and product agnostic. For example, code inspections work the same in Java as they do in Ruby, it is just the syntax trees and semantic information that is different. This section describes product specific features, such as specific project model differences and how to target them in a plugin.

**CN:**  IntelliJ平台的很多功能都与语言和产品无关。例如，代码检查在Java中与在Ruby中工作是一样的，只是语法树和语义信息有所不同。本节描述产品特定的特性，例如特定的项目模型差异，以及如何在插件中针对它们。

#### Part IX - Custom IDEs——第九部分-自定义IDE

Documents how to use the _IntelliJ Platform_ to create a new, custom IDE, rather than plugins to an existing product, e.g. like WebStorm, or Android Studio.

**CN:**  文档说明了如何使用IntelliJ平台来创建一个新的、自定义的IDE，而不是像WebStorm或Android Studio这样的现有产品的插件。

#### [Part X - Plugin Repository](/plugin_repository/index.md)——第十部分-插件仓库

This part has been moved to [JetBrains Marketplace documentation](https://plugins.jetbrains.com/docs/marketplace/about-marketplace.html).

**CN:**  这部分已经移到[JetBrains Marketplace documentation](https://plugins.jetbrains.com/docs/marketplace/about-marketplace.html)。

#### [Appendix I - Resources](/appendix/resources/useful_links.md)——附录I -资源

Links to useful resources, such as the IntelliJ Community Edition source code, the Plugin Development forum and the Jetbrains Platform Slack.

**CN:**  链接到有用的资源，如IntelliJ社区版源代码、插件开发论坛和Jetbrains平台Slack。

#### [Appendix II - Changes](/reference_guide/api_changes_list.md)——附录II -变更

Provides a list of [backwards-incompatible](/reference_guide/api_changes_list.md) API changes as well as [notable changes and new features](/reference_guide/api_notable/api_notable.md) in each major release of the IntelliJ Platform.

**CN:**  提供了[backwards-incompatible(向后不兼容的)](/reference_guide/api_changes_list.md)向后不兼容的API更改列表，以及IntelliJ平台每个主要版本中[notable changes and new features(值得注意的更改和新特性)](/reference_guide/api_notable/api_notable.md)。