---
title: DialogWrapper
---


## DialogWrapper

The
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
is the base class which is supposed to be used for all modal dialogs (and some non-modal dialogs) shown in *IntelliJ Platform* plugins.

**CN:**  [`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
是一个基类，它应该用于IntelliJ平台插件中显示的所有模态对话框(以及一些非模态对话框)。

It provides the following features:

**CN:**  它提供了以下功能:

*  Button layout (platform-specific order of `OK/Cancel` buttons, macOS-specific `Help` button)

**CN:**  按钮的布局(平台的`OK/Cancel`按钮，mavcOs的`Help`按钮)

*  Context help 

**CN:**  上下文帮助

*  Remembering the size of the dialog 

**CN:**  记住对话框的大小

*  Non-modal validation (displaying an error message text when the data entered into the dialog is not valid)

**CN:**  非模态验证(当输入到对话框的数据无效时显示错误消息文本)

* Keyboard shortcuts:
  *  `Esc` for closing the dialog
  *  `Left/Right` for switching between buttons
  *  `Y/N` for `Yes/No` actions if they exist in the dialog

**CN:**  
* 键盘快捷键:
  * 关闭对话框
  * 用于按钮之间的切换
  * 如果对话框存在`Yes/No`操作，`Y/N`用于操作它们

*  Optional `Do not ask again` checkbox 

**CN:**  可选的`Do not ask again`复选框


When using the
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
class for your own dialog, you need to follow these steps:

**CN:**  当你为自己的对话框使用
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
类时，你需要遵循以下步骤:

*  Call the base class constructor and provide either a project in the frame of which the dialog will be displayed, or a parent component for the dialog.

**CN:**  调用基类构造函数，并提供对话框所在框架中的项目或对话框的父组件。

*  Call the `init()` method from the constructor of your dialog class

**CN:**  从您的dialog类的构造函数中调用`init()`方法

*  Call the `setTitle()` method to set the title for the dialog box

**CN:**  调用`setTitle()`方法来设置对话框的标题

*  Implement the `createCenterPanel()` method to return the component comprising the main contents of the dialog.

**CN:**  实现`createCenterPanel()`方法来返回包含对话框主要内容的组件。

*  *Optional*: Override the `getPreferredFocusedComponent()` method and return the component that should be focused when the dialog is first displayed.

**CN:**  可选:覆盖`getPreferredFocusedComponent()`方法返回在对话框第一次显示时应该被聚焦的组件。

*  *Optional*: Override the `getDimensionServiceKey()` method to return the identifier which will be used for persisting the dialog dimensions.

**CN:**  可选:覆盖`getDimensionServiceKey()`方法返回将用于持久化对话框维度的标识符。

*  *Optional*: Override the `getHelpId()` method to return the context help topic associated with the dialog.

**CN:**  可选:覆盖`getHelpId()`方法返回与对话框关联的上下文帮助主题。

The
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
class is often used together with UI Designer forms.
In this case, you bind a UI Designer form to your class extending
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java),
bind the top-level panel of the form to a field and return that field from the `createCenterPanel()` method.

**CN:**  [`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
类通常与UI设计器表单一起使用。
在本例中，将UI设计器表单绑定到扩展为
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
的类，将表单的顶级面板绑定到字段，并从`createCenterPanel()`方法返回该字段。

To display the dialog, you call the `show()` method and then use the `getExitCode()` method to check how the dialog was closed. The `showAndGet()` method can be used to combine these two calls.

**CN:**  要显示对话框，可以调用`show()`方法，然后使用`getExitCode()`方法检查对话框是如何关闭的。`showAndGet()`方法可用于组合这两个调用。

To customize the buttons displayed in the dialog (replacing the standard `OK/Cancel/Help` set of buttons), you can override either the `createActions()` or `createLeftActions()` methods.
Both of these methods return an array of Swing Action objects.
If the button that you're adding closes the dialog, you can use
[`DialogWrapperExitAction`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java),
as the base class for your action. 
Use `action.putValue(DialogWrapper.DEFAULT_ACTION, true)` to set the default button.

**CN:**  自定义对话框中显示的按钮(替换标准的`OK/Cancel/Help`按钮集)，您可以覆盖`createActions()`或`createLeftActions()`方法。
这两个方法都返回一个Swing动作对象数组。
如果要添加的按钮关闭了对话框，可以使用
[`DialogWrapperExitAction`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java)
作为操作的基类。
使用`action.putValue(DialogWrapper.DEFAULT_ACTION, true)`设置默认按钮。

To validate the data entered into the dialog, you can override the `doValidate()` method.
The method will be called automatically by timer.
If the currently entered data is valid, you need to return `null` from your implementation.
Otherwise, you need to return a
[`ValidationInfo`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/ValidationInfo.java)
object which encapsulates an error message and an optional component associated with the invalid data.
If you specify a component, an error icon will be displayed next to it, and it will be focused when the user tries to invoke the `OK` action.

**CN:**  要验证输入到对话框的数据，可以覆盖`doValidate()`方法。
该方法将被定时器自动调用。
如果当前输入的数据有效，则需要从实现中返回`null`。
否则，您需要返回一个
[`ValidationInfo`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/ValidationInfo.java)
对象，它封装了一个错误消息和一个与无效数据相关联的可选组件。
如果您指定了一个组件，一个错误图标将显示在它旁边，当用户试图调用`OK`操作时，它将被聚焦。

## Example

Simple definition of a
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java):

**CN:**  简单定义一个[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java):

```java
public class SampleDialogWrapper extends DialogWrapper {

    public SampleDialogWrapper() {
        super(true); // use current window as parent
        init();
        setTitle("Test DialogWrapper");
    }

    @Nullable
    @Override
    protected JComponent createCenterPanel() {
        JPanel dialogPanel = new JPanel(new BorderLayout());

        JLabel label = new JLabel("testing");
        label.setPreferredSize(new Dimension(100, 100));
        dialogPanel.add(label, BorderLayout.CENTER);

        return dialogPanel;
    }
}
```

Usage of 
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java):

**CN:**  使用
[`DialogWrapper`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogWrapper.java):

```java
JButton testButton = new JButton();
testButton.addActionListener(actionEvent -> {
  if(new SampleDialogWrapper().showAndGet()) {
    // user pressed ok
  }
});
```
