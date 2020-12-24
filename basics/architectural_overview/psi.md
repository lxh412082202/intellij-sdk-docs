---
title: Program Structure Interface (PSI) 程序结构接口(PSI)
---

The Program Structure Interface, commonly referred to as just PSI, is the layer in the _IntelliJ Platform_ that is responsible for parsing files and creating the syntactic and semantic code model that powers so many of the platform's features.
程序结构接口，通常简称为PSI，是 _IntelliJ Platform_ 中的一个层，负责解析文件，创建语法和语义代码模型，为平台的许多特性提供支持。

* [PSI Files](/basics/architectural_overview/psi_files.md)
* [File View Providers](/basics/architectural_overview/file_view_providers.md)
* [PSI Elements](/basics/architectural_overview/psi_elements.md)

> **TIP** A very helpful tool for debugging the PSI implementation is the [PsiViewer plugin](https://plugins.jetbrains.com/plugin/227-psiviewer). 
It can show you the structure of the PSI tree, the properties of every PSI element and highlight its text range.
调试PSI实现的一个非常有用的工具是[PsiViewer plugin(PsiViewer插件)](https://plugins.jetbrains.com/plugin/227-psiviewer)。它可以显示PSI树的结构，每个PSI元素的属性，并突出显示它的文本范围。