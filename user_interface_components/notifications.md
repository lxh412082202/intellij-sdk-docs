---
title: Notifications 通知
---


## Notifications 通知

One of the leading design principles is avoiding the use of modal message boxes for notifying the user about errors and other situations that may warrant the user's attention.
As a replacement, the *IntelliJ Platform* provides multiple non-modal notification UI options.

**CN:**  主要的设计原则之一是避免使用模式消息框来通知用户错误和其他可能引起用户注意的情况。作为替代，IntelliJ平台提供了多种非模态通知UI选项。

### Dialogs 对话框

When working in a modal dialog, instead of checking the validity of the input when the `OK` button is pressed and notifying the user about invalid data with a modal dialog, the recommended approach is to use
[`DialogBuilder.doValidate()`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogBuilder.java),
which was described previously.

**CN:**  在模态对话框中工作时，不需要在按下OK按钮时检查输入的有效性，也不需要在模态对话框中通知用户无效数据，推荐的方法是使用[`DialogBuilder.doValidate()`](upsource:///platform/platform-api/src/com/intellij/openapi/ui/DialogBuilder.java)，如前所述。

### Editor Hints 编辑提示

For actions invoked from the editor (such as refactorings, navigation actions and different code insight features), the best way to notify the user about the inability to perform an action is to use the
[`HintManager`](upsource:///platform/platform-api/src/com/intellij/codeInsight/hint/HintManager.java)
class.
Its method `showErrorHint()` displays a floating popup above the editor which is automatically hidden when the user starts performing another action in the editor.
Other
[`HintManager`](upsource:///platform/platform-api/src/com/intellij/codeInsight/hint/HintManager.java)
methods can be used for displaying other kinds of non-modal notification hints over an editor.

**CN:**  对于从编辑器调用的操作(例如重构、导航操作和不同的代码洞察力特性)，通知用户无法执行操作的最佳方法是使用[`HintManager`](upsource:///platform/platform-api/src/com/intellij/codeInsight/hint/HintManager.java)类。
它的方法`showErrorHint()`在编辑器上方显示一个浮动的弹出窗口，当用户开始在编辑器中执行另一个操作时，这个窗口会自动隐藏起来。其他
[`HintManager`](upsource:///platform/platform-api/src/com/intellij/codeInsight/hint/HintManager.java)方法可用于在编辑器上显示其他类型的非模态通知提示。


### Top-Level Notifications 顶级的通知

The most general way to display non-modal notifications is to use the
[`Notifications`](upsource:///platform/platform-api/src/com/intellij/notification/Notifications.java)
class.

**CN:**  显示非模式通知的最常见方式是使用[`Notifications`](upsource:///platform/platform-api/src/com/intellij/notification/Notifications.java)类。

It has two main advantages:

**CN:**  它有两个主要优势：

*  The user can control the way each notification type is displayed under `Settings | Appearance & Behavior | Notifications`

**CN:**  用户可以控制在`Settings | Appearance & Behavior | Notifications`下显示每种通知类型的方式

*  All displayed notifications are gathered in the Event Log tool window and can be reviewed later

**CN:**  所有显示的通知都收集在事件日志工具窗口中，稍后可以查看

The specific method used to display a notification is
[`Notifications.Bus.notify()`](upsource:///platform/platform-api/src/com/intellij/notification/Notifications.java).
If the current Project is known, please use overload with `Project` parameter, so the notification is shown in its associated frame.

**CN:**  用于显示通知的特定方法是[`Notifications.Bus.notify()`](upsource:///platform/platform-api/src/com/intellij/notification/Notifications.java)。如果当前项目已知，请使用带有`Project`参数的重载，这样通知将显示在其关联的框架中。

The text of the notification can include HTML tags.

**CN:**  通知的文本可以包含HTML标记。

Use `Notification#addAction(AnAction)` to add links below the content, use [`NotificationAction`](upsource:///platform/platform-api/src/com/intellij/notification/NotificationAction.java) for convenience. 

**CN:**  使用`Notification#addAction(AnAction)`添加内容下面的链接，使用[`NotificationAction`](upsource:///platform/platform-api/src/com/intellij/notification/NotificationAction.java)方便。

The `groupDisplayId` parameter of the
[`Notification`](upsource:///platform/platform-api/src/com/intellij/notification/Notification.java)
constructor specifies a notification type.
The user can choose the display type corresponding to each notification type under `Settings | Appearance and Behavior | Notifications`.
To specify the preferred display type, you need to call
[`Notifications.Bus.register()`](upsource:///platform/platform-api/src/com/intellij/notification/Notifications.java)
before displaying any notifications.

**CN:**  [`Notification`](upsource:///platform/platform-api/src/com/intellij/notification/Notification.java)构造函数的`groupDisplayId`参数指定通知类型。
用户可以选择与`Settings | Appearance and Behavior | Notifications`下的每个通知类型对应的显示类型。
要指定首选显示类型，需要在显示任何通知之前调用[`Notifications.Bus.register()`]。