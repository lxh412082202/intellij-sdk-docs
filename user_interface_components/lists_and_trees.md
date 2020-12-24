---
title: List and Tree Controls——列表和树控件
---


### JBList and Tree——JBList和Tree

Whenever you would normally use a standard
[Swing `JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html)
component, it's recommended to use the
[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java)
class as drop-in replacement.
[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java)
supports the following additional features on top of
[`JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html):

**CN:**  当您通常使用标准的[Swing `JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html)组件时，建议使用[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java)类作为完全替代。[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java)支持[`JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html)之上的以下附加功能:

*  Drawing a tooltip with complete text of an item if the item doesn't fit into the list box width.

**CN:**  如果项目不适合列表框宽度，则使用项目的完整文本绘制工具提示。

*  Drawing a gray text message in the middle of the list box when it contains no items.
   The text can be customized by calling `getEmptyText().setText()`.

**CN:**  在不包含任何项的列表框中间绘制灰色文本消息。可以通过调用`getEmptyText().setText()`定制文本。

*  Drawing a busy icon in the top right corner of the list box to indicate that a background operation is being performed.
   This can be enabled by calling `setPaintBusy()`.

**CN:**  在列表框的右上角绘制一个忙碌图标，以指示正在执行后台操作。这可以通过调用`setPaintBusy()`来启用。

Similarly, the
[`Tree`](upsource:///platform/platform-api/src/com/intellij/ui/treeStructure/Tree.java)
class provides a replacement for the standard
[`JTree`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html)
class.
In addition to the features of
[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java),
it supports wide selection painting (Mac style) and auto-scroll on drag & drop.

**CN:**  类似地，[`Tree`](upsource:///platform/platform-api/src/com/intellij/ui/treeStructure/Tree.java)类提供了对标准[`JTree`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html)类的替换。除了[`JBList`](upsource:///platform/platform-api/src/com/intellij/ui/components/JBList.java)的功能，它还支持广泛的选择绘画(Mac风格)和自动滚动的拖放。

### ColoredListCellRenderer and ColoredTreeCellRenderer——ColoredListCellRenderer和ColoredTreeCellRenderer

When you need to customize the presentation of items in a list box or a tree, it's recommended to use the
[`ColoredListCellRenderer`](upsource:///platform/platform-api/src/com/intellij/ui/ColoredListCellRenderer.java)
or
[`ColoredTreeCellRenderer`](upsource:///platform/platform-api/src/com/intellij/ui/ColoredTreeCellRenderer.java)
classes as the cell renderer.
These classes allow you to compose the presentation out of multiple text fragments with different attributes by calling `append()` and to set an optional icon for the item by calling `setIcon`.
The renderer automatically takes care of setting the correct text color for selected items and of many other platform-specific rendering details.

**CN:**  当您需要自定义列表框或树中的项的表示时，建议使用[`ColoredListCellRenderer`](upsource:///platform/platform-api/src/com/intellij/ui/ColoredListCellRenderer.java)或[`ColoredTreeCellRenderer`](upsource:///platform/platform-api/src/com/intellij/ui/ColoredTreeCellRenderer.java)类作为单元格呈现程序。
         这些类允许您通过调用`append()`将具有不同属性的多个文本片段组成表示，并通过调用`setIcon`设置项的可选图标。
         渲染器自动负责为选择的项目和许多其他特定于平台的渲染细节设置正确的文本颜色。

### ListSpeedSearch and TreeSpeedSearch——ListSpeedSearch和TreeSpeedSearch

To facilitate keyboard-based selection of items in a list box or a tree, you can install a speed search handler on it using the
[`ListSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/ListSpeedSearch.java)
and
[`TreeSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/TreeSpeedSearch.java).
This can be done simply by calling `new ListSpeedSearch(list)` or `new TreeSpeedSearch(tree)`.
If you need to customize the text which is used to locate the element, you can override the `getElementText()` method.
Alternatively, you can pass a function to convert items to strings.
A function needs to be passed as `elementTextDelegate` to the
[`ListSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/ListSpeedSearch.java)
constructor or as `toString` to the
[`TreeSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/TreeSpeedSearch.java)
constructor.

**CN:**  为了方便在列表框或树中基于键盘选择项目，您可以使用[`ListSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/ListSpeedSearch.java)和[`TreeSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/TreeSpeedSearch.java)在其上安装一个快速搜索处理程序。
         这可以通过简单地调用`new ListSpeedSearch(list)`或`new TreeSpeedSearch(tree)`来实现。
         如果需要自定义用于定位元素的文本，可以覆盖`getElementText()`方法。
         或者，您可以传递一个函数来将项转换为字符串。
         一个函数需要作为`elementTextDelegate`传递给[`ListSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/ListSpeedSearch.java)构造函数，或者作为`toString`传递给[`TreeSpeedSearch`](upsource:///platform/platform-impl/src/com/intellij/ui/TreeSpeedSearch.java)构造函数。

### ToolbarDecorator

A very common task in plugin development is showing a list or a tree where the user is allowed to add, remove, edit or reorder the items.
The implementation of this task is greatly facilitated by the
[`ToolbarDecorator`](upsource:///platform/platform-api/src/com/intellij/ui/ToolbarDecorator.java)
class.
This class provides a toolbar with actions on items and automatically enables drag & drop reordering of items in list boxes if supported by the underlying list model.
The position of the toolbar above or below the list depends on the platform under which the IDE is running.

**CN:**  插件开发中一个非常常见的任务是显示一个列表或树，用户可以在其中添加、删除、编辑或重新排序项。
         [`ToolbarDecorator`](upsource:///platform/platform-api/src/com/intellij/ui/ToolbarDecorator.java)类极大地促进了该任务的实现。
         这个类提供一个工具栏，其中包含对项的操作，如果底层列表模型支持，则自动启用对列表框中项的拖放重新排序。
         工具栏在列表上方或下方的位置取决于IDE所运行的平台。

To use a toolbar decorator:

**CN:**  要使用工具栏装饰:

*  If you need to support removing and reordering of items in a list box, make sure the model of your list implements the
   [`EditableModel`](upsource:///platform/util/ui/src/com/intellij/util/ui/EditableModel.java)
   interface.
   [`CollectionListModel`](upsource:///platform/util/ui/src/com/intellij/ui/CollectionListModel.java)
   is a handy model class that implements this interface.

**CN:**  如果需要支持删除和重新排序列表框中的项，请确保列表的模型实现了[`EditableModel`](upsource:///platform/util/ui/src/com/intellij/util/ui/EditableModel.java)接口。
         [`CollectionListModel`](upsource:///platform/util/ui/src/com/intellij/ui/CollectionListModel.java)是一个方便的模型类，它实现了这个接口。

*  Call
   [`ToolbarDecorator.createDecorator()`](upsource:///platform/platform-api/src/com/intellij/ui/ToolbarDecorator.java)
   to create a decorator instance.

**CN:**  调用[`ToolbarDecorator.createDecorator()`](upsource:///platform/platform-api/src/com/intellij/ui/ToolbarDecorator.java)来创建装饰器实例。

*  If you need to support adding and/or removing items, call `setAddAction()` and/or `setRemoveAction()`.

**CN:**  如果您需要支持添加和/或删除项目，请调用`setAddAction()`和/或`setRemoveAction()`。

*  If you need other buttons in additional to the standard ones, call `addExtraAction()` or `setActionGroup()`.

**CN:**  如果您需要在标准按钮之外的其他按钮，请调用`addExtraAction()`或`setActionGroup()`。

*  Call `createPanel()` and add the component it returns to your panel.

**CN:**  调用`createPanel()`并将其返回的组件添加到您的面板中。

<!--
### AbstractTreeBuilder and AbstractTreeStructure
TODO link to tutorial
-->



