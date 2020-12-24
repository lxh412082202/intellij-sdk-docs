---
title: PSI Cookbook
---

This page gives a list of recipes for the most common operations for working with the PSI (Program Structure Interface). Unlike [Developing Custom Language Plugins](/reference_guide/custom_language_support.md), it talks about working with the PSI of existing languages (such as Java).

> **TIP** Please see also [Working with PSI efficiently](/reference_guide/performance/performance.md#working-with-psi-efficiently).

### How do I find a file if I know its name but don't know the path?——如果我知道一个文件的名称，但不知道它的路径，我如何找到它?

`FilenameIndex.getFilesByName()`

### How do I find where a particular PSI element is used?——我如何找到一个特定的元素在哪里被使用?

`ReferencesSearch.search()`

### How do I rename a PSI element?——如何重命名PSI元素?

`RefactoringFactory.createRename()`

### How can I cause the PSI for a virtual file to be rebuilt?——我怎样才能使一个虚拟文件的PSI被重建?

`FileContentUtil.reparseFiles()`

## Java Specific——Java特定的

### How do I find all inheritors of a class?——如何找到一个类的所有继承者?

`ClassInheritorsSearch.search()`

### How do I find a class by qualified name?——如何通过限定名查找类?

`JavaPsiFacade.findClass()`

### How do I find a class by short name?——如何通过短名称查找类?

`PsiShortNamesCache.getInstance().getClassesByName()`

### How do I find a superclass of a Java class?——如何查找Java类的超类?

`PsiClass.getSuperClass()`

### How do I get a reference to the containing package of a Java class?——如何获得对Java类的包含包的引用?

```java
PsiJavaFile javaFile = (PsiJavaFile) psiClass.getContainingFile();
PsiPackage pkg = JavaPsiFacade.getInstance(project).findPackage(javaFile.getPackageName());
```        
or

`com.intellij.psi.util.PsiUtil.getPackageName()`

### How do I find the methods overriding a specific method?——如何查找覆盖特定方法的方法?

`OverridingMethodsSearch.search()`
