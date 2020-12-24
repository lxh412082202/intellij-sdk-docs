---
title: PSI References——PSI引用
---

A *reference* in a PSI tree is an object that represents a link from a *usage* of a certain element in the code
to the corresponding *declaration*. *Resolving* a reference means locating the declaration to which a specific usage
refers.

**CN:**  PSI树中的*reference*(引用)是一个对象，它表示代码中某个元素的*usage*(用法)的链接到相应的*declaration*(声明)。*Resolving*(解析)引用意味着查找特定用法所在的声明引用。

The most common type of references is defined by language semantics. For example, consider a simple Java method:

**CN:**  最常见的引用类型是由语言语义定义的。例如，考虑一个简单的Java方法:

```java
public void hello(String message) {
    System.out.println(message);
}
```

This simple code fragment contains five references. The references created by the identifiers `String`, `System`, `out` and
`println` can be resolved to the corresponding declarations in the JDK: the `String` and `System` classes, the `out` field and the
`println` method. The reference created by the second occurrence of the `message` identifier in `println(message)` can be resolved to the
`message` parameter, declared by `String message` in the method header.

**CN:**  这个简单的代码片段包含5个引用。标识符`String`、`System`、`out`和`println`创建的引用可以解析为JDK中相应的声明:`String`和`System`类、`out`字段和
`println`方法。`println(message)`中第二次出现`message`标识符时创建的引用可以解析为`message`参数，该参数由方法头中的`String message`声明。

Note that `String message` is not a reference, and cannot be resolved. Instead, it's a _declaration_. It does not
refer to any name defined elsewhere; instead, it defines a name by itself.

**CN:**  注意，`String message`不是引用，不能解析。相反，它是一个 _declaration_ (声明)。它不涉及在其他地方定义的任何名称;相反，它自己定义一个名称。

A reference is an instance of a class implementing the [`PsiReference`](upsource:///platform/core-api/src/com/intellij/psi/PsiReference.java) interface.
Note that references are distinct from PSI elements. You can obtain the references created by a PSI element by calling
`PsiElement.getReferences()`, and can go back from a reference to an element by calling `PsiReference.getElement()`.

**CN:**  

To *resolve* the reference - to locate the declaration being referenced - you call `PsiReference.resolve()`. It's very
important to understand the difference between `PsiReference.getElement()` and `PsiReference.resolve()`. The former method returns the _source_
of a reference, while the latter returns its _target_. In the example above, for the `message` reference, `getElement()`
will return the `message` identifier on the second line of the snippet, and `resolve()` will return the `message` identifier
on the first line (inside the parameter list).

**CN:**  

The process of resolving references is distinct from parsing, and is not performed at the same time. Moreover, it is
not always successful. If the code currently open in the IDE does not compile, or in other situations, it's normal
for `PsiReference.resolve()` to return `null`, and if you work with references, you need to be able to handle that in your code.

**CN:**  

> **TIP** Please see also _Cache results of heavy computations_ in [Working with PSI efficiently](/reference_guide/performance/performance.md#working-with-psi-efficiently).
>
>**CN:**  

## Contributed References——

In addition to references defined by the semantics of the programming language, IntelliJ IDEA recognizes many references
which are determined by the semantics of the APIs and frameworks used in your code. Consider the following example:

**CN:**  

```java
File f = new File("foo.txt");
```

Here, "foo.txt" has no special meaning from the point of view of the Java syntax - it's just a string literal. However,
if you open this example in IntelliJ IDEA, and if you have a file called "foo.txt" in the same directory, you'll notice
that you're able to Ctrl-click on "foo.txt" and navigate to the file. This works because IntelliJ IDEA recognizes the
semantics of `new File(...)` and _contributes a reference_ into the string literal passed as a parameter to the method.

**CN:**  

Normally, references can be contributed into elements which don't have their own references, such as string literals
and comments. References are also often contributed into non-code files, such as XML or JSON.

**CN:**  

Contributing references is one of the most common ways to extend an existing language. For example, your plugin can
contribute references to Java code, even though the Java PSI is part of the platform and not defined in your plugin.

**CN:**  

To contribute your own references, see the [reference contributor tutorial](/tutorials/custom_language_support/reference_contributor.md).

**CN:**  

## References with Optional or Multiple Resolve Results——

In the simplest case, a reference resolves to a single element, and if the resolve fails, this means that the
code is incorrect and the IDE needs to highlight it as an error. However, there are cases when the situation is different.

**CN:**  

The first case is *soft references*. Consider the `new File("foo.txt")` example above. If IntelliJ IDEA can't find
the file "foo.txt", it doesn't mean that an error needs to be highlighted - maybe the file is only available at runtime.
Such references return `true` from the `PsiReference.isSoft()` method.

**CN:**  

The second case is *polyvariant references*. Consider the case of a JavaScript program. JavaScript is a dynamically
typed language, so the IDE cannot always precisely determine which method is being called at a particular location.
To handle this, it provides a reference that can be resolved to multiple possible elements.
Such references implement the [`PsiPolyVariantReference`](upsource:///platform/core-api/src/com/intellij/psi/PsiPolyVariantReference.java) interface.

**CN:**  

For resolving a `PsiPolyVariantReference`, you call its `multiResolve()` method. The call returns an array of
[`ResolveResult`](upsource:///platform/core-api/src/com/intellij/psi/ResolveResult.java) objects. Each of the
objects identifies a PSI element and also specifies whether the result is valid. For example, if you have multiple
Java method overloads and a call with arguments not matching any of the overloads, you will get
back `ResolveResult` objects for all of the overloads, and `isValidResult()` will return false for all of them.

**CN:**  

## Searching for References——

As you already know, resolving a reference means going from a usage to the corresponding declaration. To perform the
navigation in the opposite direction - from a declaration to its usages - you need to perform a **references search**.

**CN:**  

To perform a references search, you use the 
[`ReferencesSearch`](upsource:///platform/indexing-api/src/com/intellij/psi/search/searches/ReferencesSearch.java) class.
To perform a search, you need to specify the *element* to search for, and optionally other parameters such as the
scope in which the reference needs to be searched. You get back a *query* object that allows you to get all results
as an array, or to iterate over the results one by one. If you don't need to collect all the results, it's more efficient
to use the iteration, because it allows you to stop the processing once you've found the element you need.

**CN:**  

## Implementing References——

The documentation above covers the key points of accessing references. If you need to create your own references
(if you're implementing a custom language or a reference contributor for an existing language),
please refer to the [guide](/reference_guide/custom_language_support/references_and_resolve.md) and
[tutorial](/tutorials/custom_language_support/reference_contributor.md) for implementing references.

**CN:**  