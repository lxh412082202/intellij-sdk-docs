---
title: Navigating the PSI——PSI导航
---

There are three main ways to navigate the PSI: *top-down*, *bottom-up*, and using *references*. In the first scenario, 
you have a PSI file or another higher-level element (for example, a method), and you need to find all elements that match a
specified condition (for example, all variable declarations). In the second scenario, you have a specific point
in the PSI tree (for example, the element at caret) and need to find out something about its context (for example,
the element in which it has been declared). Finally, *references* allow you to navigate from the use of an element
(e.g., a method call) to the declaration (the method being called) and back. References are described in a
[separate topic](psi_references.md).

**CN:**  有三种主要的方法来导航PSI:自顶向下、自底向上和使用引用。在第一个场景中，您有一个PSI文件或另一个更高级别的元素(例如，一个方法)，您需要找到与指定条件匹配的所有元素(例如，所有变量声明)。在第二个场景中，您在PSI树中有一个特定的点(例如，插入符号处的元素)，需要查找关于它的上下文的一些信息(例如，声明它的元素)。最后，引用允许您从使用元素(例如，一个方法调用)导航到声明(被调用的方法)，然后返回。引用在[separate topic](psi_references.md)中描述。


## Top-down navigation——自顶向下导航

The most common way to perform top-down navigation is to use a *visitor*. To use a visitor, you create a class
(usually an anonymous inner class) that extends the base visitor class, override the methods that handle the elements
you're interested in, and pass the visitor instance it to `PsiElement.accept()`.

**CN:**  执行自顶向下导航的最常见方法是使用*visitor*。要使用visitor，您需要创建一个类(通常是一个匿名内部类)，该类扩展基本visitor类，覆盖处理您感兴趣的元素的方法，并将visitor实例传递给`PsiElement.accept()`。

The base classes for visitors are language-specific. For example, if you need to process elements in a Java file,
you extend `JavaRecursiveElementVisitor` and override the methods corresponding to the Java element types you're
interested in. 

**CN:**  访问者的基类是特定于语言的。例如，如果您需要处理Java文件中的元素，您可以扩展`JavaRecursiveElementVisitor`并覆盖与您感兴趣的Java元素类型对应的方法。

The following snippet shows the use of a visitor to find all Java local variable declarations:

**CN:**  下面的代码片段展示了如何使用visitor来查找所有Java局部变量声明:

```java
file.accept(new JavaRecursiveElementVisitor() {
  @Override
  public void visitLocalVariable(PsiLocalVariable variable) {
    super.visitLocalVariable(variable);
    System.out.println("Found a variable at offset " + variable.getTextRange().getStartOffset());
  }
});
```

In many cases, you can also use more specific APIs for top-down navigation. For example, if you need to get a list of
all methods in a Java class, you can do that using a visitor, but a much easier way to do that is to call `PsiClass.getMethods()`.

**CN:**  在许多情况下，您还可以为自顶向下导航使用更具体的api。例如，如果您需要获得Java类中所有方法的列表，您可以使用visitor来实现这一点，但是更简单的方法是调用`PsiClass.getMethods()`。

[`PsiTreeUtil`](upsource:///platform/core-api/src/com/intellij/psi/util/PsiTreeUtil.java) contains a number of
general-purpose, language-independent functions for PSI tree navigation, some of which (for example, `findChildrenOfType()`)
perform top-down navigation.

**CN:**  [`PsiTreeUtil`](upsource:///platform/core-api/src/com/intellij/psi/util/PsiTreeUtil.java)包含许多用于PSI树导航的通用的、独立于语言的函数，其中一些(例如，`findChildrenOfType()`)执行自顶向下的导航。

## Bottom-up navigation——自底向上的导航

The starting point for bottom-up navigation is either a specific element in the PSI tree (for example, the result of
resolving a reference), or an offset. If you have an offset, you can find the corresponding PSI element by calling
`PsiFile.findElementAt()`. This method returns the element at the lowest level of the tree (for example, an identifier),
and you need to navigate the tree up if you want to determine the broader context.

**CN:**  自底向上导航的起点可以是PSI树中的特定元素(例如，解析引用的结果)，也可以是偏移量。如果你有一个偏移量，你可以通过调用`PsiFile.findElementAt()`来找到相应的PSI元素。这个方法返回树的最低层次的元素(例如，一个标识符)，如果你想要确定更广泛的上下文，你需要向上导航树。

In most cases, bottom-up navigation is performed by calling `PsiTreeUtil.getParentOfType()`. This method goes up the
tree until it finds the element of the type you've specified. For example, to find the containing method, you call
`PsiTreeUtil.getParentOfType(element, PsiMethod.class)`.

**CN:**  在大多数情况下，自底向上的导航是通过调用`PsiTreeUtil.getParentOfType()`来执行的。例如，要查找包含的方法，可以调用`PsiTreeUtil.getParentOfType(element, PsiMethod.class)`。

In some cases, you can also use specific navigation methods. For example, to find the class where a method is contained,
you call `PsiMethod.getContainingClass()`.

**CN:**  在某些情况下，您还可以使用特定的导航方法。例如，要查找包含方法的类，可以调用`PsiMethod.getContainingClass()`。

The following snippet shows how these calls can be used together:

**CN:**  下面的代码片段展示了如何将这些调用一起使用:

```java
PsiFile psiFile = anActionEvent.getData(CommonDataKeys.PSI_FILE);
PsiElement element = psiFile.findElementAt(offset);
PsiMethod containingMethod = PsiTreeUtil.getParentOfType(element, PsiMethod.class);
PsiClass containingClass = containingMethod.getContainingClass();
```

To see how the navigation works in practice, please refer to the 
[code sample](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/psi_demo/src/com/intellij/tutorials/psi/PsiNavigationDemoAction.java).

**CN:**  要了解实际的导航工作原理，请参考[code sample](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/psi_demo/src/com/intellij/tutorials/psi/PsiNavigationDemoAction.java)。