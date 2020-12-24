---
title: PSI Files PSI——文件
---

A PSI (Program Structure Interface) file is the root of a structure representing the contents of a file as a hierarchy of elements in a particular programming language.

**CN:**  PSI(程序结构接口)文件是表示文件内容的结构的根，它是特定编程语言中元素的层次结构。

The [`PsiFile`](upsource:///platform/core-api/src/com/intellij/psi/PsiFile.java) class is the common base class for all PSI files, while files in a specific language are usually represented by its subclasses.  For example, the [`PsiJavaFile`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiJavaFile.java) class represents a Java file, and the [`XmlFile`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlFile.java) class represents an XML file.
[`PsiFile`](upsource:///platform/core-api/src/com/intellij/psi/PsiFile.java)

**CN:**  类是所有PSI文件的公共基类，而特定语言中的文件通常由它的子类表示。例如，
[`PsiJavaFile`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiJavaFile.java)
类表示一个Java文件，而
[`XmlFile`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlFile.java)
类表示一个XML文件。

Unlike `VirtualFile` and `Document`, which have application scope (even if multiple projects are open, each file is represented by the same `VirtualFile` instance), PSI has project scope (the same file is represented by multiple `PsiFile` instances if the file belongs to multiple projects open at the same time).

**CN:**  与具有应用范围的`VirtualFile`和`Document`不同(即使打开了多个项目，每个文件也由相同的`VirtualFile`实例表示)，PSI具有项目范围(如果文件属于同时打开的多个项目，则相同的文件由多个`PsiFile`实例表示)。

## How do I get a PSI file?——如何获得PSI文件

* From an action: `e.getData(LangDataKeys.PSI_FILE)`. 

**CN:**  从一个操作中：`e.getData(LangDataKeys.PSI_FILE)`。

* From a VirtualFile: `PsiManager.getInstance(project).findFile()` 

**CN:**  从一个VirtualFile中：`PsiManager.getInstance(project).findFile()`。

* From a Document: `PsiDocumentManager.getInstance(project).getPsiFile()` 

**CN:**  从一个Document中：`PsiDocumentManager.getInstance(project).getPsiFile()`。

* From an element inside the file: `psiElement.getContainingFile()` 

**CN:**  来自文件内的元素：`psiElement.getContainingFile()`。

* To find files with a specific name anywhere in the project, use `FilenameIndex.getFilesByName(project, name, scope)` 

**CN:**  要查找项目中任何地方具有特定名称的文件，请使用`FilenameIndex.getFilesByName(project, name, scope)`。

## What can I do with a PSI file?——我能用PSI文件做什么?

Most interesting modification operations are performed on the level of individual PSI elements, not files as a whole.

**CN:**  大多数有趣的修改操作是在单个PSI元素的级别上执行的，而不是在整个文件上执行的。

To iterate over the elements in a file, use `psiFile.accept(new PsiRecursiveElementWalkingVisitor()...);`

**CN:**  若要遍历文件中的元素，请使用`psiFile.accept(new PsiRecursiveElementWalkingVisitor()...);`

## Where does a PSI file come from?——PSI文件从何而来?

As PSI is language-dependent, PSI files are created through the [`Language`](upsource:///platform/core-api/src/com/intellij/lang/Language.java) object, by using the `LanguageParserDefinitions.INSTANCE.forLanguage(language).createFile(fileViewProvider)` method.

**CN:**  由于PSI依赖于语言，所以通过使用`LanguageParserDefinitions.INSTANCE.forLanguage(language).createFile(fileViewProvider)`方法通过
[`Language`](upsource:///platform/core-api/src/com/intellij/lang/Language.java)
对象创建PSI文件。

Like documents, PSI files are created on demand when the PSI is accessed for a particular file.

**CN:**  与文档一样，PSI文件是在访问特定文件时根据需要创建的。

## How long do PSI files persist?——PSI文件能存在多久?

Like documents, PSI files are weakly referenced from the corresponding `VirtualFile` instances and can be garbage-collected if not referenced by anyone.

**CN:**  与文档一样，PSI文件也只能从相应的VirtualFile实例中得到弱引用，如果没有任何人引用，就会被垃圾收集。

## How do I create a PSI file?——如何创建PSI文件?

The [`PsiFileFactory`](upsource:///platform/core-api/src/com/intellij/psi/PsiFileFactory.java) `createFileFromText()` method creates an in-memory PSI file with the specified contents.

**CN:**  [`PsiFileFactory`](upsource:///platform/core-api/src/com/intellij/psi/PsiFileFactory.java)的`createFileFromText()`方法创建一个具有指定内容的内存中的PSI文件。

To save the PSI file to disk, use the [`PsiDirectory`](upsource:///platform/core-api/src/com/intellij/psi/PsiDirectory.java) `add()` method.

**CN:**  要将PSI文件保存到磁盘，请使用[`PsiDirectory`](upsource:///platform/core-api/src/com/intellij/psi/PsiDirectory.java)的`add()`方法。

## How do I get notified when PSI files change?——当PSI文件改变时，我如何得到通知?

`PsiManager.getInstance(project).addPsiTreeChangeListener()` allows you to receive notifications about all changes to the PSI tree of a project.

**CN:**  `PsiManager.getInstance(project).addPsiTreeChangeListener()`允许您接收关于项目的PSI树的所有更改的通知。

## How do I extend PSI?——如何扩展PSI？

PSI can be extended to support additional languages through custom language plugins. For more details on developing custom language plugins, see the [Custom Language Support](/reference_guide/custom_language_support.md) reference guide.

**CN:**  可以通过自定义语言插件扩展PSI以支持其他语言。有关开发自定义语言插件的详细信息，请参阅[Custom Language Support (自定义语言支持)](/reference_guide/custom_language_support.md)。参考指南

## What are the rules for working with PSI?——使用PSI的规则是什么?

Any changes done to the content of PSI files are reflected in documents, so all [rules for working with documents](documents.md#what-are-the-rules-of-working-with-documents) (read/write actions, commands, read-only status handling) are in effect.

**CN:**  对PSI文件内容所做的任何更改都反映在文档中，因此所有[rules for working with documents(处理文档的规则)](documents.md#what-are-the-rules-of-working-with-documents)(读/写操作、命令、只读状态处理)都有效。