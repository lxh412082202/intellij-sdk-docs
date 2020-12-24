---
title: Tree Structure View——树状视图
---

This tutorial is meant to illustrate how the project tree structure view appearance can be modified programmatically. 
If you need to know more about basic concepts of a project view in IntelliJ-based IDEs, please refer to
[Exploring The Project Structure](https://www.jetbrains.com/idea/help/exploring-the-project-structure.html#d164891e120)
of 
[IntelliJ IDEA Web Help](https://www.jetbrains.com/idea/help/intellij-idea.html).

**CN：** 本教程旨在说明如何以编程方式修改项目树结构视图外观。如果您需要了解更多关于基于intellij的ide项目视图的基本概念，请参考
[IntelliJ IDEA Web Help](https://www.jetbrains.com/idea/help/intellij-idea.html)
的
[Exploring The Project Structure](https://www.jetbrains.com/idea/help/exploring-the-project-structure.html#d164891e120)。

Series of step below show how to filter out and keep visible only text files and directories in the Project View Panel. 

**CN：** 下面的一系列步骤展示了如何过滤掉项目视图面板中的文本文件和目录并保持可见。

## Pre-requirements——技能

Create an empty plugin project.
See 
[Creating a Plugin Project](/tutorials/build_system/prerequisites.md).

**CN：** 创建一个空的插件项目。祥见[Creating a Plugin Project](/tutorials/build_system/prerequisites.md).

## 1. Register Custom TreeStructure Provider——注册自定义树结构提供程序

Add new *treeStructureProvider* extension to the
[plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/tree_structure_provider/src/main/resources/META-INF/plugin.xml).

**CN：** 在
[plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/tree_structure_provider/src/main/resources/META-INF/plugin.xml)
中添加新的*treeStructureProvider*扩展。

```java
<extensions defaultExtensionNs="com.intellij">
  <treeStructureProvider implementation="org.jetbrains.tutorials.tree.structure.TextOnlyTreeStructureProvider"/>
</extensions>
```

## 2. Implement Custom TreeStructureProvider——实现自定义TreeStructureProvider

To provide custom Structure View behaviour you need to implement TreeStructureProvider interface.  

**CN：** 要提供自定义结构视图行为，需要实现TreeStructureProvider接口。

```java
public class TextOnlyTreeStructureProvider implements TreeStructureProvider {
    @NotNull
    @Override
    public Collection<AbstractTreeNode> modify(@NotNull AbstractTreeNode parent, @NotNull Collection<AbstractTreeNode> children, ViewSettings settings) {
        return null;
    }

    @Nullable
    @Override
    public Object getData(Collection<AbstractTreeNode> collection, String s) {
        return null;
    }
}
```

## 3. Override modify() method——覆盖modify()方法

To implement Tree Structure nodes filtering logic, override `modify()` method.
The example below shows how to filter out all the Project View nodes except those which correspond to text files and directories.

**CN：** 若要实现树结构节点过滤逻辑，请重写`modify()`方法。
下面的示例显示了如何过滤除与文本文件和目录对应的节点之外的所有项目视图节点。

```java
{% include /code_samples/tree_structure_provider/src/main/java/org/jetbrains/tutorials/tree/structure/TextOnlyTreeStructureProvider.java %}
```

## 4. Compile and Run the Plugin——编译并运行插件

Compile and run the code sample from this tutorial.
Refer to 
[Running and Debugging a Plugin](/basics/getting_started/running_and_debugging_a_plugin.md).

**CN：** 编译并运行本教程中的代码示例。参照
[Running and Debugging a Plugin](/basics/getting_started/running_and_debugging_a_plugin.md)。

After going through the steps described above you can see only text files and directories belonging to a project in the Project View.

**CN：** 完成上述步骤后，在项目视图中只能看到属于某个项目的文本文件和目录。

![Text Files](tree_structure_view/img/text_only.png)


Check out 
[plugin source code](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/tree_structure_provider)
and build the project to see how TreeStructureView provider works in practice.

**CN：** 检出[plugin source code](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/tree_structure_provider)
并且构建项目以查看TreeStructureView提供程序在实践中是如何工作的。