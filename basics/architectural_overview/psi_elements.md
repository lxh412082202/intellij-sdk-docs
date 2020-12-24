---
title: PSI Elements——PSI的元素
---

A PSI (Program Structure Interface) file represents a hierarchy of PSI elements (so-called _PSI trees_). A single PSI file (itself being a PSI element) may contain several PSI trees in specific programming languages. A PSI element, in its turn, can have child PSI elements.

**CN:**  一个PSI(程序结构接口)文件表示PSI元素的层次结构(即所谓的PSI树)。在特定的编程语言中，单个PSI文件(本身是PSI元素)可能包含多个PSI树。一个PSI元素，也可以有子PSI元素。

PSI elements and operations at the level of individual PSI elements are used to explore the internal structure of source code as it is interpreted by the **IntelliJ Platform**. For example, you can use PSI elements to perform code analysis, such as [code inspections](https://www.jetbrains.com/help/idea/code-inspection.html) or [intention actions](https://www.jetbrains.com/idea/help/intention-actions.html).

**CN:**  PSI元素和单个PSI元素层次上的操作被用于探索源代码的内部结构，因为它是由**IntelliJ Platform**解释的。例如，您可以使用PSI元素来执行代码分析，例如[code inspections(代码检查)](https://www.jetbrains.com/help/idea/code-inspection.html)或[intention actions(意图操作)](https://www.jetbrains.com/idea/help/intention-actions.html)。

The [`PsiElement`](upsource:///platform/core-api/src/com/intellij/psi/PsiElement.java) class is the common base class for PSI elements.

**CN:**  [`PsiElement`](upsource:///platform/core-api/src/com/intellij/psi/PsiElement.java)类是PSI元素的公共基类。

## How do I get a PSI element?——如何得到元素?

* From an action: `e.getData(LangDataKeys.PSI_ELEMENT)`. Note: if an editor is currently open and the element under caret is a reference, this will return the result of resolving the reference. This may or may not be what you need.

**CN:**  从一个操作:`e.getData(LangDataKeys.PSI_ELEMENT)`。注意：如果编辑器当前处于打开状态，并且插入符号下的元素是引用，则将返回解析引用的结果。这可能是你需要的，也可能不是。

* From a file by offset: `PsiFile.findElementAt()`. Note: this returns the lowest level element  ("leaf") at the specified offset, which is normally a lexer token.
Most likely you should use `PsiTreeUtil.getParentOfType()` to find the element you really need.

**CN:**  按偏移量从文件：`PsiFile.findElementAt()`。注意：这将返回指定偏移量处的最低级别元素("leaf")，该偏移量通常是lexer令牌。很可能您应该使用`PsiTreeUtil.getParentOfType()`来找到您真正需要的元素。

* By iterating through a PSI file: using a [`PsiRecursiveElementWalkingVisitor`](upsource:///platform/core-api/src/com/intellij/psi/PsiRecursiveElementWalkingVisitor.java).

**CN:**  通过遍历PSI文件:使用[`PsiRecursiveElementWalkingVisitor`](upsource:///platform/core-api/src/com/intellij/psi/PsiRecursiveElementWalkingVisitor.java)。

* By resolving a reference: `PsiReference.resolve()`

**CN:**  通过解析引用：`PsiReference.resolve()`。

## What can I do with PSI elements?——我该怎么处理PSI元素呢?

See [PSI Cook Book](/basics/psi_cookbook.md).

**CN:**  参阅[PSI Cook Book](/basics/psi_cookbook.md)。
