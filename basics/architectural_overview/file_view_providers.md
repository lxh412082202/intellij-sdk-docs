---
title: File View Providers 文件视图提供者
---

A file view provider ([`FileViewProvider`](upsource:///platform/core-api/src/com/intellij/psi/FileViewProvider.java)) manages access to multiple PSI trees within a single file.

**CN:**  文件视图提供程序([`FileViewProvider`](upsource:///platform/core-api/src/com/intellij/psi/FileViewProvider.java))管理对单个文件中的多个PSI树的访问。

For example, a JSPX page has a separate PSI tree for the Java code in it (`PsiJavaFile`), a separate tree for the XML code (`XmlFile`), and a separate tree for JSP as a whole ([`JspFile`](upsource:///java/jsp-openapi/src/com/intellij/psi/jsp/JspFile.java)).

**CN:**  例如，JSPX页面中有一个用于Java代码的单独的PSI树(`PsiJavaFile`)，一个用于XML代码的单独树(`XmlFile`)，还有一个用于整个JSP的单独树([`JspFile`](upsource:///java/jsp-openapi/src/com/intellij/psi/jsp/JspFile.java))。

Each of the PSI trees covers the entire contents of the file, and contains special "outer language elements" in the places where contents in a different language can be found.

**CN:**  每个PSI树覆盖文件的全部内容，并在可以找到不同语言内容的地方包含特殊的“外部语言元素”。

A `FileViewProvider` instance corresponds to a single `VirtualFile`, a single `Document`, and can be used to retrieve multiple `PsiFile` instances.

**CN:**  一个`FileViewProvider`实例对应于一个单独的`VirtualFile`，一个单独的`Document`，可以用来检索多个`PsiFile`实例。

## How do I get a FileViewProvider? 如何获得FileViewProvider?

* From a `VirtualFile`: `PsiManager.getInstance(project).findViewProvider()`
* From a `PsiFile`: `psiFile.getViewProvider()`

## What can I do with a FileViewProvider? 我可以用FileViewProvider做什么?

* To get the set of all languages for which PSI trees exist in a file: `fileViewProvider.getLanguages()`

**CN:**  获取PSI树存在于文件中的所有语言的集合:`fileViewProvider.getLanguages()`

* To get the PSI tree for a particular language: `fileViewProvider.getPsi(language)`. For example, to get the PSI tree for XML, use `fileViewProvider.getPsi(XMLLanguage.INSTANCE)`.

**CN:**  得到特定语言的PSI树:`fileViewProvider.getPsi(language)`。例如，使用`fileViewProvider.getPsi(XMLLanguage.INSTANCE)`获取XML的PSI树。

* To find an element of a particular language at the specified offset in the file: `fileViewProvider.findElementAt(offset, language)`

**CN:**  查找文件中指定偏移位置的特定语言元素:`fileViewProvider.findElementAt(offset, language)`

## How do I extend the FileViewProvider? 如何扩展FileViewProvider?

To create a file type that has multiple interspersing trees for different languages, a plugin must contain an extension to the `com.intellij.fileType.fileViewProviderFactory` extension point.

**CN:**  要创建一个包含多种不同语言的树的文件类型，插件必须包含`com.intellij.fileType.fileViewProviderFactory`扩展点的扩展。

Implement [`FileViewProviderFactory`](upsource:///platform/core-api/src/com/intellij/psi/FileViewProviderFactory.java) and return your `FileViewProvider` implementation from `createFileViewProvider()` method.

**CN:**  实现[`FileViewProviderFactory`](upsource:///platform/core-api/src/com/intellij/psi/FileViewProviderFactory.java)并且在`createFileViewProvider()`方法中返回你的`FileViewProvider`实现。

Register as follows in `plugin.xml`:

**CN:**  在`plugin.xml`中注册：

```xml
<extensions defaultExtensionNs="com.intellij">
  <fileType.fileViewProviderFactory filetype="%file_type%" implementationClass="com.plugin.MyFileViewProviderFactory" />
</extensions>
```

Where `%file_type%` refers to the type of the file being created (for example, "JFS").

**CN:**  其中`%file_type%`指的是正在创建的文件的类型(例如，“JFS”)。