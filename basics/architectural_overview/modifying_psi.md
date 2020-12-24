---
title: Modifying the PSI
---

The PSI is a read-write representation of the source code as a tree of elements corresponding to the structure of a source
file. You can modify the PSI by *adding*, *replacing*, and *deleting* PSI elements.

**CN:**  PSI是源代码的读写表示形式，它是与源文件结构相对应的元素树。您可以通过添加、替换和删除PSI元素来修改PSI。

To perform these operations, you use methods such as `PsiElement.add()`, `PsiElement.delete()`, and `PsiElement.replace()`,
as well as other methods defined in the `PsiElement` interface that let you process multiple elements in a single
operation, or to specify the exact location in the tree where an element needs to be added.

**CN:**  要执行这些操作,您使用方法如`PsiElement.add()`, `PsiElement.delete()`,和`PsiElement.replace()`,以及其他`PsiElement`接口中定义的方法,让你在单个操作过程中多个元素,或指定一个元素树中的确切位置,需要添加。

Just as document operations, PSI modifications need to be wrapped in a write action and in a command (and therefore
can only be performed in the event dispatch thread). See [the Documents article](documents.md#what-are-the-rules-of-working-with-documents)
for more information on commands and write actions.

**CN:**  与文档操作一样，PSI修改也需要包装在写操作和命令中(因此只能在事件调度线程中执行)。有关命令和写操作的更多信息，请参见[the Documents article](documents.md#what-are-the-rules-of-working-with-documents)。


## Creating the New PSI 创建新的PSI

The PSI elements to add to the tree, or to replace existing PSI elements, are normally *created from text*.
In the most general case, you use the `createFileFromText()` method of [`PsiFileFactory`](upsource:///platform/core-api/src/com/intellij/psi/PsiFileFactory.java)
to create a new file that contains the code construct which you need to add to the tree or to use as a replacement
for an existing element, traverse the resulting tree to locate the specific element that you need, and then pass that
element to `add()` or `replace()`.

**CN:**  要添加到树中的PSI元素，或要替换现有的PSI元素，通常是从文本创建的。在最一般的情况下,您使用[`PsiFileFactory`](upsource:///platform/core-api/src/com/intellij/psi/PsiFileFactory.java)的`createFileFromText()`方法来创建一个新文件,其中包含您需要添加的代码构造树或使用替代现有的元素,遍历结果树定位所需要的特定元素,然后通过元素`add()`或`replace()`。

Most languages provide factory methods which let you create specific code constructs more easily. For example,
the [`PsiJavaParserFacade`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiJavaParserFacade.java) class
contains methods such as `createMethodFromText()`, which creates a Java method from the given text.

**CN:**  大多数语言都提供了工厂方法，让您更容易地创建特定的代码构造。例如，[`PsiJavaParserFacade`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiJavaParserFacade.java)类包含`createMethodFromText()`等方法，它根据给定的文本创建Java方法。

When you're implementing refactorings, intentions or inspection quickfixes that work with existing code, the text that
you pass to the various `createFromText()` methods will combine hard-coded fragments and fragments of code taken from
the existing file. For small code fragments (individual identifiers), you can simply append the text from the existing
code to the text of the code fragment you're building. In that case, you need to make sure that the resulting text is 
syntactically correct, otherwise the `createFromText()` method will throw an exception. 

**CN:**  当您实现与现有代码一起工作的重构、意图或检查快速修复时，传递给各种`createFromText()`方法的文本将结合硬编码的片段和从现有文件中提取的代码片段。对于小的代码片段(单个标识符)，您可以简单地将现有代码中的文本附加到正在构建的代码片段的文本中。在这种情况下，您需要确保结果文本在语法上是正确的，否则createFromText()方法将抛出异常。

For larger code fragments, it's best to perform the modification in several steps: 

**CN:**  对于较大的代码片段，最好通过以下几个步骤进行修改:

 * create a replacement tree fragment from text, leaving placeholders for the user code fragments;
 
 **CN:**  从文本中创建一个替换的树片段，为用户代码片段留下占位符;
 
 * replace the placeholders with the user code fragments;
 
 **CN:**  用用户代码片段替换占位符;
 
 * replace the element in the original source file with the replacement tree.

**CN:**  用替换树替换原始源文件中的元素。

This ensures that the formatting of the user code is preserved and that the modification doesn't introduce any unwanted
whitespace changes.  

**CN:**  这可以确保保留用户代码的格式，并且修改不会引入任何不必要的空白更改。 

As an example of this approach, see the quickfix in the `ComparingReferencesInspection` example:

**CN:**  作为这种方法的一个例子，参见`ComparingReferencesInspection`例子中的quickfix:

```java
// binaryExpression holds a PSI expression of the form "x == y", which needs to be replaced with "x.equals(y)"
// binaryExpression包含“x == y”形式的PSI表达式，需要用“x.equals(y)”替换
PsiBinaryExpression binaryExpression = (PsiBinaryExpression) descriptor.getPsiElement();
IElementType opSign = binaryExpression.getOperationTokenType();
PsiExpression lExpr = binaryExpression.getLOperand();
PsiExpression rExpr = binaryExpression.getROperand();

// Step 1: Create a replacement fragment from text, with "a" and "b" as placeholders
// 步骤1:用“a”和“b”作为占位符，从文本中创建一个替换片段
PsiElementFactory factory = JavaPsiFacade.getInstance(project).getElementFactory();
PsiMethodCallExpression equalsCall =
    (PsiMethodCallExpression) factory.createExpressionFromText("a.equals(b)", null);

// Step 2: replace "a" and "b" with elements from the original file
// 步骤2:用原始文件中的元素替换“a”和“b”
equalsCall.getMethodExpression().getQualifierExpression().replace(lExpr);
equalsCall.getArgumentList().getExpressions()[0].replace(rExpr);

// Step 3: replace a larger element in the original file with the replacement tree
// 步骤3:用替换树替换原始文件中的较大元素
PsiExpression result = (PsiExpression) binaryExpression.replace(equalsCall);
```

Just as everywhere else in the IntelliJ Platform API, the text passed to `createFileFromText()` and other `createFromText()`
methods must use only `\n` as line separators.

**CN:**  与IntelliJ平台API中的其他地方一样，传递给`createFileFromText()`和其他`createFromText()`方法的文本必须只使用`\n`作为行分隔符。

## Maintaining Tree Structure Consistency——保持树状结构的一致性

The PSI modification methods do not restrict you in the way you can build the resulting tree structure. For example,
when working with a Java class, you can add a `for` statement as a direct child of a `PsiMethod` element, even though
the Java parser will never produce such a structure (the `for` statement will always be a child of the `PsiCodeBlock`)
representing the method body). Modifications that produce incorrect tree structures may appear to work, but they will
lead to problems and exceptions later. Therefore, you always need to ensure that the structure you built with PSI
modification operations is the same as what the parser would produce when parsing the code that you've built.

**CN:**  PSI修改方法不会限制您构建结果树结构的方式。例如，当使用Java类时，您可以添加一个`for`语句作为`PsiMethod`元素的直接子元素，即使Java解析器永远不会生成这样的结构(`for`语句始终是表示方法体的`PsiCodeBlock`的子语句)。产生不正确的树结构的修改可能看起来有效，但是它们将在以后导致问题和异常。因此，您总是需要确保使用PSI修改操作构建的结构与解析器在解析您构建的代码时生成的结构相同。

To make sure you're not introducing inconsistencies, you can call `PsiTestUtil.checkFileStructure()` in the tests for
your action that modifies the PSI. This method ensures that the structure you've built is the same as what the parser produces.

**CN:**  为了确保没有引入不一致，可以在修改PSI的操作的测试中调用`PsiTestUtil.checkFileStructure()`。该方法确保您构建的结构与解析器生成的结构相同。

## Whitespaces and Imports 空格和引入

When working with PSI modification functions, you should never create individual whitespace nodes (spaces or line breaks)
from text. Instead, all whitespace modifications are performed by the formatter, which follows the code style settings
selected by the user. Formatting is automatically performed at the end of every command, and if you need, you can
also perform it manually using the `reformat(PsiElement)` method in the
[`CodeStyleManager`](upsource:///platform/core-api/src/com/intellij/psi/codeStyle/CodeStyleManager.java) class.

**CN:**  在使用PSI修改函数时，不应该从文本中创建单独的空白节点(空格或换行)。相反，所有空白修改都由格式化程序执行，它遵循用户选择的代码样式设置。格式化是在每个命令结束时自动执行的，如果需要，还可以使用[`CodeStyleManager`](upsource:///platform/core-api/src/com/intellij/psi/codeStyle/CodeStyleManager.java)类中的`reformat(PsiElement)`方法手动执行。

Also, when working with Java code (or with code in other languages with a similar import mechanism such as Groovy or Python),
you should never create imports manually. Instead, you should insert fully-qualified names into the code you're
generating, and then call the `shortenClassReferences()` method in the 
[`JavaCodeStyleManager`](upsource:///java/java-psi-api/src/com/intellij/psi/codeStyle/JavaCodeStyleManager.java)
(or the equivalent API for the language you're working with). This ensures that the imports are created according to
the user's code style settings and inserted into the correct place of the file.

**CN:**  此外，在使用Java代码(或使用其他语言中类似导入机制(如Groovy或Python)的代码)时，不应该手动创建导入。相反，应该在生成的代码中插入完全限定的名称，然后在[`JavaCodeStyleManager`](upsource:///java/java-psi-api/src/com/intellij/psi/codeStyle/JavaCodeStyleManager.java)中调用`shortenClassReferences()`方法(或使用的语言的等效API)。这将确保根据用户的代码样式设置创建导入，并将其插入文件的正确位置。

## Combining PSI and Document Modifications —— 结合PSI和文档修改

In some cases, you need to perform a PSI modification and then to perform an operation on the document you've just
modified through the PSI (for example, start a live template). In this case, you need to call a special method that
completes the PSI-based post-processing (such as formatting) and commits the changes to the document. The method
you need to call is called `doPostponedOperationsAndUnblockDocument()`, and it's defined in the
[`PsiDocumentManager`](upsource:///platform/core-api/src/com/intellij/psi/PsiDocumentManager.java) class.

**CN:**  在某些情况下，您需要执行PSI修改，然后对刚刚通过PSI修改的文档执行操作(例如，启动活动模板)。在这种情况下，您需要调用一个特殊的方法来完成基于psi的后处理(例如格式化)并将更改提交到文档。您需要调用的方法称为`doPostponedOperationsAndUnblockDocument()`，它是在[`PsiDocumentManager`](upsource:///platform/core-api/src/com/intellij/psi/PsiDocumentManager.java)类中定义的。

