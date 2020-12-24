---
title: Structure View——结构视图
---

The Structure View implementation used for a specific file type can be customized on many levels.
If a custom language plugin provides an implementation of the
[`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java)
interface, it can completely replace the standard structure view implementation with a custom user interface component.
However, for most languages this is not necessary, and the standard
[`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java)
implementation provided by *IntelliJ Platform* can be reused.

**CN:**  用于特定文件类型的结构视图实现可以在多个级别上进行自定义。
         如果自定义语言插件提供了
         [`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java)
         接口，它可以完全用自定义用户接口组件替换标准结构视图实现。
         然而，对于大多数语言来说，这是不必要的，也是标准的
         [`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java)
         *IntelliJ Platform*提供的实现可以重用。

The starting point for the structure view is the
[`PsiStructureViewFactory`](upsource:///platform/editor-ui-api/src/com/intellij/lang/PsiStructureViewFactory.java)
interface, which is registered in the `com.intellij.lang.psiStructureViewFactory` extension point.

**CN:**  结构视图的起点是
         [`PsiStructureViewFactory`](upsource:///platform/editor-ui-api/src/com/intellij/lang/PsiStructureViewFactory.java)
         接口，注册在`com.intellij.lang.psiStructureViewFactory`扩展点。

**Examples:**

**CN:**  例如：

- [`PsiStructureViewFactory`](upsource:///plugins/properties/src/com/intellij/lang/properties/structureView/PropertiesStructureViewBuilderFactory.java)
for
[Properties language plugin(Properties语言插件)](upsource:///plugins/properties)

- [Custom Language Support Tutorial: Structure View(自定义语言支持教程:结构视图)](/tutorials/custom_language_support/structure_view_factory.md)

To reuse the *IntelliJ Platform* implementation of the
[`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java),
the plugin returns a
[`TreeBasedStructureViewBuilder`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/TreeBasedStructureViewBuilder.java)
from its
[`PsiStructureViewFactory.getStructureViewBuilder()`](upsource:///platform/editor-ui-api/src/com/intellij/lang/PsiStructureViewFactory.java)<!--#L35-->
method.
As the model for the builder, the plugin can specify a subclass of
[`TextEditorBasedStructureViewModel`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/TextEditorBasedStructureViewModel.java),
and by overriding methods of this subclass it customizes the structure view for a specific language.

**CN:**  重复使用[`StructureView`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureView.java)的*IntelliJ Platform*实现，
         插件返回一个[`TreeBasedStructureViewBuilder`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/TreeBasedStructureViewBuilder.java)
         从它的[`PsiStructureViewFactory.getStructureViewBuilder()`](upsource:///platform/editor-ui-api/src/com/intellij/lang/PsiStructureViewFactory.java)方法。
         作为构建器的模型，插件可以指定[`TextEditorBasedStructureViewModel`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/TextEditorBasedStructureViewModel.java)的子类，
         通过覆盖这个子类的方法，它可以定制特定语言的结构视图。

**Example**:

**CN:**  例如：

[`StructureViewModel`](upsource:///plugins/properties/properties-psi-impl/src/com/intellij/lang/properties/structureView/PropertiesFileStructureViewModel.java)
for
[Properties language plugin(Properties语言插件)](upsource:///plugins/properties)

The main method to override is `getRoot()`, which returns the instance of a class implementing the
[`StructureViewTreeElement`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureViewTreeElement.java)
interface.
There exists no standard implementation of this interface, so a plugin will need to implement it completely.

**CN:**  覆盖的主要方法是`getRoot()`，它返回实现[`StructureViewTreeElement`](upsource:///platform/editor-ui-api/src/com/intellij/ide/structureView/StructureViewTreeElement.java)接口的类的实例。
         这个接口没有标准的实现，所以插件需要完全实现它。

The structure view tree is usually built as a partial mirror of the PSI tree.
In the implementation of
`StructureViewTreeElement.getChildren()`,
the plugin can specify which of the child elements of a specific PSI tree node need to be represented as elements in the structure view.
Another important method is `getPresentation()`, which can be used to customize the text, attributes, and icon used to represent an element in the structure view.

**CN:**  结构视图树通常是作为PSI树的部分镜像构建的。
         在`StructureViewTreeElement.getChildren()`的实施过程中，
         插件可以指定特定PSI树节点的哪些子元素需要在结构视图中表示为元素。
         另一个重要的方法是`getPresentation()`，它可用于自定义用于表示结构视图中的元素的文本、属性和图标。

The implementation of `StructureViewTreeElement.getChildren()` needs to be matched by `TextEditorBasedStructureViewModel.getSuitableClasses()`.
The latter method returns an array of `PsiElement`\-derived classes which can be shown as structure view elements. It is used to select the Structure View item matching the cursor position when the structure view is first opened or when the _Autoscroll from source_ option is enabled.

**CN:**  `StructureViewTreeElement.getChildren()`的实施需要`TextEditorBasedStructureViewModel.getSuitableClasses()`配合。
         后一种方法返回一个`PsiElement`\-派生类数组，可以将其显示为结构视图元素。它用于在第一次打开结构视图或启用 _Autoscroll from source_ 选项时选择与光标位置匹配的结构视图项。

**Example:**
[`StructureViewTreeElement`](upsource:///plugins/properties/properties-psi-impl/src/com/intellij/lang/properties/editor/PropertyStructureViewElement.java)
for
[Properties language plugin(Properties语言插件)](upsource:///plugins/properties/)

