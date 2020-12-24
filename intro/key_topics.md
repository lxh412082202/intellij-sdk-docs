---
title: Key Topics——关键主题
---

The _IntelliJ Platform_ is very large, and very capable, and its size and scope can initially be very daunting. This page is intended to list the key topics that a plugin author would be interested in, and provide quick links to the most common extension points.

**CN:**  IntelliJ平台非常大，功能也非常强大，它的规模和范围最初会让人望而生畏。这个页面的目的是列出插件作者感兴趣的关键主题，并提供最常见扩展点的快速链接。

## Essential concepts——基本概念

- [Getting Started](/basics/getting_started.md) with plugins.

**CN:**  插件[Getting Started(入门)](/basics/getting_started.md)

- [Testing plugins](/basics/testing_plugins/testing_plugins.md).

**CN:**  [Testing plugins(测试插件)](/basics/testing_plugins/testing_plugins.md).

- Component model - the _IntelliJ Platform_ is a component based application, and is responsible for creating components and injecting dependencies. Understanding this is necessary for building plugins.

**CN:**  组件模型——IntelliJ平台是一个基于组件的应用程序，负责创建组件和注入依赖关系。理解这一点对于构建插件是必要的。

- [Extension points](/basics/plugin_structure/plugin_extensions.md) - how to register components with extension points, and how to find out what extension points are available.

**CN:**  [Extension points(扩展点)](/basics/plugin_structure/plugin_extensions.md)——如何使用扩展点注册组件，以及如何查找哪些扩展点可用。

- [Virtual files](/basics/architectural_overview/virtual_file.md) - all file access should go through the Virtual File System which abstracts and caches the file system. This means you can work with files that are on the local file system, in a zip file or are old versions from version control.

**CN:**  [Virtual files(虚拟文件)](/basics/architectural_overview/virtual_file.md)——所有文件访问都要经过虚拟文件系统，虚拟文件系统对文件系统进行抽象和缓存。这意味着您可以处理本地文件系统中的文件、zip文件或版本控制中的旧版本。

## Code model——代码模型

The _IntelliJ Platform_'s code model is called the PSI - the [Program Structure Interface](/basics/architectural_overview/psi.md). The PSI parses code, builds indexes and creates a semantic model.

**CN:**  IntelliJ平台的代码模型被称为PSI——[Program Structure Interface(程序结构接口)](/basics/architectural_overview/psi.md)。PSI可以解析代码、构建索引并创建语义模型。

## Common extension points——常见的扩展点

The _IntelliJ Platform_ is extremely extensible, and most features and services can be extended. Some of the common extension points are:

**CN:**  IntelliJ平台具有极强的可扩展性，大部分功能和服务都可以扩展。一些常见的扩展点是:

* [Actions](/tutorials/action_system.md) - menu and toolbar items

**CN:**  [Actions(操作)](/tutorials/action_system.md)——菜单和工具栏项

* [Code inspections](/tutorials/code_inspections.md) - code analysis that looks at the syntax trees and semantic models and highlight issues in the editor.

**CN:**  [Code inspections(代码检查)](/tutorials/code_inspections.md)——代码分析关注语法树和语义模型，并在编辑器中突出显示问题。

* [Intentions](/tutorials/code_intentions.md) - context specific actions that are available in the <kbd>Alt</kbd>+<kbd>Enter</kbd> menu when the text caret is at a certain location.

**CN:**  [Intentions(意图)](/tutorials/code_intentions.md)——当文本插入符号位于某个位置时，在Alt+Enter菜单中可用的特定于上下文的操作。

* [Code completion](/reference_guide/custom_language_support/code_completion.md).

**CN:**  [Code completion(代码自动完成)](/reference_guide/custom_language_support/code_completion.md).