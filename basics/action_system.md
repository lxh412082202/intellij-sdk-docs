---
title: Action System——操作系统
---

## Introduction——介绍
The actions system is an extension point that allows plugins to add their items to IntelliJ Platform-based IDE menus and toolbars. 
For example, one of the action classes is responsible for the **File \| Open File...** menu item and for the **Open File** toolbar button.

**CN:**  操作系统是一个扩展点，允许插件将其项添加到基于IntelliJ平台的IDE菜单和工具栏中。
         例如，其中一个action类负责 **File | Open File...** 菜单项和 **Open File** 工具栏按钮。

Actions in the IntelliJ Platform require a [code implementation](#action-implementation) and must be [registered](#registering-actions) with the IntelliJ Platform. 
The action implementation determines the contexts in which an action is available, and its functionality when it is selected in the UI. 
Registration determines where an action appears in the IDE UI.
Once implemented and registered, an action receives callbacks from the IntelliJ Platform in response to user gestures.

**CN:**  IntelliJ平台上的动作需要一个[code implementation](#action-implementation)，并且必须是与IntelliJ平台的[registered](#registering-actions)。
         操作实现决定了一个操作可用的上下文，以及它在UI中被选中时的功能。
         注册决定了一个动作在IDE UI中出现的位置。
         一旦实现和注册，动作就会收到响应用户手势的IntelliJ平台的回调。

The [Creating Actions](/tutorials/action_system/working_with_custom_actions.md) tutorial describes the process of adding a custom action to a plugin.
The [Grouping Actions](/tutorials/action_system/grouping_action.md) tutorial demonstrates three types of groups that can contain actions.
The rest of this page is an overview of actions as an extension point.

**CN:**  [Creating Actions](/tutorials/action_system/working_with_custom_actions.md)教程描述了向插件添加自定义操作的过程。
         [Grouping Actions](/tutorials/action_system/grouping_action.md)教程演示了可以包含操作的三种类型的组。
         该页的其余部分是作为扩展点的操作的概述。

* bullet list
{:toc}

## Action Implementation——操作实现
An action is a class derived from the abstract class [`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java).
The IntelliJ Platform calls methods of an action when a user interacts with a menu item or toolbar button. 

**CN:**  action是一个派生自抽象类[`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)的类。
         当用户与菜单项或工具栏按钮交互时，IntelliJ平台会调用操作的方法。


> **Warning** Classes based on `AnAction` do not have class fields of any kind. This is because an instance of `AnAction` class
exists for the entire lifetime of the application. If `AnAction` class uses a field to store data that has a shorter 
lifetime, and doesn't clear this data promptly, the data will be leaked. For example, any `AnAction` data that exists 
only within the context of a `Project` will cause the `Project` to be kept in memory after the user has closed it.
>
>**CN:**  警告：基于`AnAction`的类没有任何类字段。这是因为`AnAction`类的一个实例
             在应用程序的整个生存期中存在。如果`AnAction`类使用一个字段来存储短的数据
             生存期，如果不及时清除这些数据，数据就会泄露。例如，存在的任何`AnAction`数据
             只有在`Project`的上下文中，才会导致在用户关闭`Project`之后将它保存在内存中。

### Principal Implementation Overrides——主要实现覆盖
Every IntelliJ Platform action should override `AnAction.update()` and must override `AnAction.actionPerformed()`.

**CN:**  IntelliJ平台的每一次行动都应该覆盖`AnAction.update()`，也必须覆盖`AnAction.actionPerformed()`。

* An action's method `AnAction.update()` is called by the IntelliJ Platform framework to update the state of an action.
  The state (enabled, visible) of an action determines whether the action is available in the UI of an IDE.
  An object of type [`AnActionEvent`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnActionEvent.java) is passed to this method, and it contains the information about the current context for the action.
  Actions are made available by changing state in the [Presentation](upsource:///platform/platform-api/src/com/intellij/ide/presentation/Presentation.java) object associated with the event context.
  As explained in [Overriding the `AnAction.update()`  Method](#overriding-the-anactionupdate-method), it is vital `update()` methods _execute quickly_ and return execution to the IntelliJ Platform. 

**CN:**  一个动作的方法`AnAction.update()`被IntelliJ平台框架调用来更新动作的状态。
         操作的状态(启用的、可见的)决定了该操作在IDE的UI中是否可用。
         将类型为[`AnActionEvent`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnActionEvent.java)的对象传递给此方法，该对象包含有关操作的当前上下文的信息。
         可以通过更改与事件上下文关联的[Presentation](upsource:///platform/platform-api/src/com/intellij/ide/presentation/Presentation.java)对象中的状态来提供操作。
         正如在[Overriding the `AnAction.update()`  Method](#overriding-the-anactionupdate-method)中所解释的，`update()`方法 _execute quickly_ 和返回执行到IntelliJ平台是至关重要的。

* An action's method `AnAction.actionPerformed()` is called by the IntelliJ Platform if it is available and selected by the user.
  This method does the heavy lifting for the action - it contains the code to be executed when the action is invoked.
  The `actionPerformed()` method also receives `AnActionEvent` as a parameter, which can be used to access projects, files, selection, etc. 
  See [Overriding the `AnAction.actionPerformed()` Method](#overriding-the-anactionactionperformed-method) for more information.

**CN:**  如果一个动作的方法`AnAction.actionPerformed()`可用并由用户选择，IntelliJ平台就会调用它。
         此方法为操作执行繁重的工作—它包含调用操作时要执行的代码。
         `actionPerformed()`方法还接收`AnActionEvent`作为参数，用于访问项目、文件、选择等。
         更多信息请参见[Overriding the `AnAction.actionPerformed()` Method](#overriding-the-anactionactionperformed-method)。

There are other methods to override in the `AnAction` class, such as for changing the default `Presentation` object for the action.
There is also a use case for overriding action constructors when registering them with dynamic action groups, which is demonstrated in the [Grouping Actions](/tutorials/action_system/grouping_action.md#adding-child-actions-to-the-dynamic-group) tutorial. 
However, the `update()` and `actionPerformed()` methods are essential to basic operation.

**CN:**  在`AnAction`类中还有其他方法可以覆盖，比如更改动作的默认`Presentation`对象。
         在向动态动作组注册动作构造函数时，还有一个覆盖它们的用例，[Grouping Actions](/tutorials/action_system/grouping_action.md#adding-child-actions-to-the-dynamic-group)教程中对此进行了演示。
         然而，`update()`和`actionPerformed()`方法对于基本操作是必不可少的。


### Overriding the AnAction.update Method——覆盖AnAction.update方法
The method `AnAction.update()` is periodically called by the IntelliJ Platform in response to user gestures.
The `update()` method gives an action to evaluate the current context and enable or disable its functionality.

**CN:**  为了响应用户的手势，IntelliJ平台定期调用`AnAction.update()`方法。
         `update()`方法提供一个动作来评估当前上下文并启用或禁用其功能。


> **Warning** The `AnAction.update()` method can be called frequently, and on a UI thread.
This method needs to _execute very quickly_; no real work should be performed in this method.
For example, checking selection in a tree or a list is considered valid, but working with the file system is not.
>
>**CN:**  警告：为了响应用户的手势,IntelliJ平台定期调用`AnAction.update()`方法。
              _execute very quickly_ 方法提供一个动作来评估当前上下文并启用或禁用其功能。


> **Tip** If the new state of an action cannot be determined quickly, then evaluation should be performed in the `AnAction.actionPerformed()` method, and notify the user that the action cannot be executed if the context isn't suitable. 
>
>**CN:**  提示：如果不能快速确定操作的新状态，那么应该在`AnAction.actionPerformed()`方法中执行评估，并通知用户如果上下文不合适，操作将无法执行。


#### Determining the Action Context——确定操作上下文
The `AnActionEvent` object passed to `update()` carries information about the current context for the action.
Context information is available from the methods of `AnActionEvent`, providing information such as the Presentation, and whether the action is triggered from a Toolbar.
Additional context information is available using the method `AnActionEvent.getData()`. 
Keys defined in [`CommonDataKeys`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/CommonDataKeys.java) are passed to the `getData()` method to retrieve objects such as `Project`, `Editor`, `PsiFile`, and other information.
Accessing this information is relatively light-weight and is suited for `AnAction.update()`.

**CN:**  传递给`update()`的`AnActionEvent`对象携带有关操作的当前上下文的信息。
         上下文信息可以从`AnActionEvent`的方法中获得，提供诸如表示、操作是否从工具栏触发等信息。
         可以使用`AnActionEvent.getData()`方法获得其他上下文信息。
         [`CommonDataKeys`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/CommonDataKeys.java)中定义的键被传递给`getData()`方法来检索对象，如`Project`、`Editor`、`PsiFile`和其他信息。
         访问这一信息相对较轻，适合`AnAction.update()`。


#### Enabling and Setting Visibility for an Action——启用和设置操作的可见性
Based on information about the action context, the `AnAction.update()` method can enable, disable, or hide an action.
An action's enable/disable state and visibility are set using methods of the `Presentation` object, which is accessed using `AnActionEvent.getPresentation()`.

**CN:**  基于有关操作上下文的信息，`AnAction.update()`方法可以启用、禁用或隐藏操作。
         动作的启用/禁用状态和可见性是使用`Presentation`对象的方法设置的，使用`AnActionEvent.getPresentation()`访问该对象。

The default `Presentation` object is a set of descriptive information about a menu or toolbar action.
Every context for an action - it might appear in multiple menu or toolbar locations - has a unique presentation. 
Attributes such as an action's text, description, and icons, as well as visibility and enable/disable state, are stored in the presentation.
The attributes in a presentation get initialized from the [action registration](#registering-actions).
However, some can be changed at runtime using the methods of the `Presentation` object associated with an action.

**CN:**  默认的`Presentation`对象是一组关于菜单或工具栏操作的描述性信息。
         一个动作的每个上下文——它可能出现在多个菜单或工具栏位置——都有一个独特的表示。
         诸如操作的文本、描述和图标等属性以及可见性和启用/禁用状态都存储在表示中。
         表示中的属性从[action registration](#registering-actions)初始化。
         但是，可以在运行时使用与操作关联的`Presentation`对象的方法来更改一些操作。

The enabled/disabled state of an action is set using `Presentation.setEnabled()`.
The visibility state of an action is set using `Presentation.setVisible()`
If an action is enabled, the `AnAction.actionPerformed()` can be called if an action is selected in the IDE by a user.
A menu action will be shown in the UI location specified in the registration.
A toolbar action will display its enabled (or selected) icon, depending on the user interaction.

**CN:**  操作的启用/禁用状态是使用`Presentation.setEnabled()`设置的。
         动作的可见性状态使用`Presentation.setVisible()`设置
         如果一个操作是启用的，那么如果一个操作是由用户在IDE中选择的，则可以调用`AnAction.actionPerformed()`。
         在注册中指定的UI位置将显示一个菜单动作。
         根据用户交互，工具栏动作将显示其启用(或选中)图标。


When an action is disabled `AnAction.actionPerformed()` will not be called.
Toolbar actions will display their respective icons for the disabled state.
The visibility of a disabled action in a menu depends on whether the host menu (e.g. "ToolsMenu") containing the action has the `compact` attribute set.
See [Grouping Actions](#grouping-actions) for more information about the `compact` attribute, and the visibility of menu actions.

**CN:**  当一个动作被禁用时，将不会调用`AnAction.actionPerformed()`。
         工具栏动作将显示它们各自的图标，以显示禁用状态。
         菜单中禁用操作的可见性取决于主机菜单(例如，主菜单)。包含动作的“ToolsMenu”)有`compact`属性设置。
         有关`compact`属性和菜单操作可见性的更多信息，请参见[Grouping Actions](#grouping-actions)。


> **Note** If an action is added to a toolbar, its `update()` can be called if there was any user activity or focus transfer. 
If the action's availability changes in the absence of these events, then call [`ActivityTracker.getInstance().inc()`](upsource:///platform/platform-api/src/com/intellij/ide/ActivityTracker.java) to notify the action subsystem to update all toolbar actions. 
>
>**CN:**  注意：如果一个操作被添加到工具栏，如果有任何用户活动或焦点转移，可以调用它的`update()`。
             如果在没有这些事件的情况下操作的可用性发生变化，则调用[`ActivityTracker.getInstance().inc()`](upsource:///platform/platform-api/src/com/intellij/ide/ActivityTracker.java)通知操作子系统更新所有工具栏操作。

An example of enabling a menu action based on whether a project is open is demonstrated in [`PopupDialogAction.update()`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java) method. 

**CN:**  在[`PopupDialogAction.update()`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java)方法中演示了根据项目是否打开来启用菜单操作的示例。

### Overriding the AnAction.actionPerformed Method——覆盖AnAction.actionPerformed方法
When the user selects an enabled action, be it from a menu or toolbar, the action's `AnAction.actionPerformed()` method is called. 
This method contains the code to be executed to perform the action, and it is here that the real work gets done.

**CN:**  当用户从菜单或工具栏中选择启用的操作时，将调用该操作的`AnAction.actionPerformed()`方法。
         此方法包含执行操作所需的代码，真正的工作就是在这里完成的。

Using the `AnActionEvent` methods and `CommonDataKeys`, objects such as the `Project`, `Editor`, `PsiFile`, and other information is available.
For example, the `actionPerformed()` method can modify, remove, or add PSI elements to a file open in the editor.

**CN:**  使用`AnActionEvent`方法和`CommonDataKeys`，可以获得`Project`、`Editor`、`PsiFile`等对象的信息。
         例如，`actionPerformed()`方法可以修改、删除或向编辑器中打开的文件中添加PSI元素。

The code that executes in the `AnAction.actionPerformed()` method should execute efficiently, but it does not have to meet the same stringent requirements as the `update()` method.

**CN:**  在`AnAction.actionPerformed()`方法中执行的代码应该有效执行，但它不必满足与`update()`方法相同的严格要求。

<!-- TODO: does this all happen inside a transaction? Does that ensure the undo step? -->
<!-- TODO:这些都发生在事务内部吗?这样可以确保撤消步骤吗? -->

An example of inspecting PSI elements is demonstrated in the SDK code sample `action_basics` [`PopupDialogAction.actionPerformed()`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java) method. 

**CN:**  在SDK代码示例`action_basics` [`PopupDialogAction.actionPerformed()`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java)方法中给出了检测PSI元件的示例。

### Action IDs——操作ID
Every action and action group has a unique identifier.
Basing the identifier for a custom action on the FQN of the implementation is the best practice, assuming the package incorporates the `<id>` of the plugin.
An action must have a unique identifier for each place it is used in the IDE UI, even though the FQN of the implementation is the same.
Definitions of identifiers for the standard IntelliJ Platform actions are in [`IdeActions`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/IdeActions.java).

**CN:**  每个操作和操作组都有唯一的标识符。
         假设包包含了插件的`<id>`，那么基于实现的FQN的自定义操作的标识符是最佳实践。
         即使实现的FQN是相同的，操作在IDE UI中使用的每个位置都必须有唯一的标识符。
         标准IntelliJ平台行动的标识符定义在[`IdeActions`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/IdeActions.java)中。

### Grouping Actions——分组操作
Groups organize actions into logical UI structures, which in turn can contain other groups. 
A group of actions can form a toolbar or a menu.
Subgroups of a group can form submenus of a menu.

**CN:**  组将操作组织到逻辑UI结构中，而逻辑UI结构又可以包含其他组。
         一组操作可以形成工具栏或菜单。
         组的子组可以形成菜单的子菜单。

Actions can be included in multiple groups, and thus appear in multiple places within the IDE UI. 
An action must have a unique identifier for each place it appears in the IDE UI.
The places where actions can appear are defined by constants in [`ActionPlaces`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/ActionPlaces.java). 
Group IDs for the IntelliJ Platform are defined in [PlatformActions](upsource:///platform/platform-resources/src/idea/PlatformActions.xml). 

**CN:**  操作可以包含在多个组中，因此可以出现在IDE UI中的多个位置。
         操作在IDE UI中出现的每个位置都必须有唯一的标识符。
         可以出现操作的位置由[`ActionPlaces`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/ActionPlaces.java)中的常量定义。
         IntelliJ平台的组id在[PlatformActions](upsource:///platform/platform-resources/src/idea/PlatformActions.xml)中定义。

<!-- TODO: Reconcile ActionPlaces vs. PlatformActions -->
<!-- TODO: 协调ActionPlaces和PlatformActions -->

For every place where the action appears, a new [`Presentation`](upsource:///platform/platform-api/src/com/intellij/ide/presentation/Presentation.java) is created. 
Therefore the same action can have different text or icons when it appears in different places of the user interface. 
Different presentations for the action are created by copying the Presentation returned by the `AnAction.getTemplatePresentation()` method.

**CN:**  每出现一个动作，就会创建一个新的[`Presentation`](upsource:///platform/platform-api/src/com/intellij/ide/presentation/Presentation.java)。
         因此，同一操作在用户界面的不同位置出现时，可能具有不同的文本或图标。
         通过复制`AnAction.getTemplatePresentation()`方法返回的表示来创建动作的不同表示。

A group's "compact" attribute specifies whether an action within that group is visible when disabled.
See [Registering Actions in plugin.xml](#registering-actions-in-pluginxml) for an explanation of how the `compact` attribute is set for a group.
If the `compact` attribute is `true` for a menu group, an action in the menu will only appear if its state is both enabled and visible.
In contrast, if the `compact` attribute is `false`, an action in the menu will appear if its state is disabled but visible.
Some menus like **Tools** have the `compact` attribute set, so there isn't a way to show an action on the tools menu if it is not enabled.

**CN:**  组的“compact”属性指定禁用该组中的操作时是否可见。
         有关如何为组设置`compact`属性的说明，请参阅[Registering Actions in plugin.xml](#registering-actions-in-pluginxml)。
         如果`compact`属性是菜单组的`true`，则只有在其状态为启用且可见时，菜单中的操作才会出现。
         相反，如果`compact`属性是`false`，则如果菜单中的操作的状态是禁用的但却是可见的，则该操作将出现在菜单中。
         有些菜单，比如 **Tools** ，有`compact`属性设置，所以如果工具菜单没有启用，就没有办法显示操作。


| Host Menu<br>`compact` Setting | Action Enabled | Visibility Enabled | Menu Item Visible? | Menu Item Appears Gray? |
| :-----: | :------------: | :----------------: | :----------------: | :---------------------: |
| 主机菜单<br>`compact`设置 |      动作启用      |    可见度启用       | 菜单项可见?         |  菜单项显示灰色? |
|    F    |       T        |         T          |        T           |         F            |
|    F    |       T        |         F          |        F           |         x            |
| ***F*** |       F        |         T          |     ***T***        |      ***T***         |
|    F    |       F        |         F          |        F           |         x            |
|    T    |       T        |         T          |        T           |         F            |
|    T    |       T        |         F          |        F           |         x            |
| ***T*** |       F        |         T          |     ***F***        |         x            |
|    T    |       F        |         F          |        F           |         x            |

See the [Grouping Actions](/tutorials/action_system/grouping_action.md) tutorial for examples of creating action groups.

**CN:**  有关创建操作组的示例，请参阅[Grouping Actions](/tutorials/action_system/grouping_action.md)教程。

## Registering Actions——注册操作
There are two main ways to register an action: either by listing it in the `<actions>` section of a plugin's `plugin.xml` file, or through code.

**CN:**  注册一个动作有两种主要的方法:要么在插件的`plugin.xml`文件的`<actions>`部分列出它，要么通过代码。

### Registering Actions in plugin.xml——在plugin.xml文件中操作
Registering actions in `plugin.xml` is demonstrated in the following example, which documents all elements and attributes that can be used in the `<actions>` section, and describes the meaning of each element.
This information can also be found by using the [Code Completion](https://www.jetbrains.com/help/idea/auto-completing-code.html#invoke-basic-completion), [Quick Definition](https://www.jetbrains.com/help/idea/viewing-reference-information.html#view-definition-symbols) and [Quick Documentation](https://www.jetbrains.com/help/idea/viewing-reference-information.html#inline-quick-documentation) features.
 
 **CN:**  下面的示例演示了在`plugin.xml`中注册操作，它记录了可以在`<actions>`部分中使用的所有元素和属性，并描述了每个元素的含义。
          还可以使用[Code Completion](https://www.jetbrains.com/help/idea/auto-completing-code.html#invoke-basic-completion)、[Quick Definition](https://www.jetbrains.com/help/idea/viewing-reference-information.html#view-definition-symbols)和[Quick Documentation](https://www.jetbrains.com/help/idea/viewing-reference-information.html#inline-quick-documentation)特性找到这些信息。
 
 
```xml
<!-- Actions -->
<actions>
  <!-- The <action> element defines an action to register.
       The mandatory "id" attribute specifies a unique 
       identifier for the action.
       The mandatory "class" attribute specifies the
       FQN of the class implementing the action.
       The mandatory "text" attribute specifies the text of the
       action (tooltip for toolbar button or text for menu item).
       The optional "use-shortcut-of" attribute specifies the ID
       of the action whose keyboard shortcut this action will use.
       The optional "description" attribute specifies the text
       which is displayed in the status bar when the action is focused.
       The optional "icon" attribute specifies the icon which is
       displayed on the toolbar button or next to the menu item. 
        
        CN: <action>元素定义了一个要注册的操作。
        强制的“id”属性指定操作的唯一标识符。
        强制的“class”属性指定实现操作的类的FQN。
        强制的“text”属性指定操作的文本(工具栏按钮的工具提示或菜单项的文本)。
        可选的“use-shortcut-of”属性指定该操作将使用的键盘快捷方式的操作ID。
        可选的“description”属性指定当操作被聚焦时状态栏中显示的文本。
        可选的“图标”属性指定显示在工具栏按钮上或菜单项旁边的图标。 -->
  
<action id="VssIntegration.GarbageCollection" class="com.foo.impl.CollectGarbage" text="Collect _Garbage" description="Run garbage collector" icon="icons/garbage.png">
    <!-- The <add-to-group> node specifies that the action should be added
         to an existing group. An action can be added to several groups.
         The mandatory "group-id" attribute specifies the ID of the group
         to which the action is added.
         The group must be implemented by an instance of the DefaultActionGroup class.
         The mandatory "anchor" attribute specifies the position of the
         action in the group relative to other actions. It can have the values
         "first", "last", "before" and "after".
         The "relative-to-action" attribute is mandatory if the anchor
         is set to "before" and "after", and specifies the action before or after which
         the current action is inserted. 
         
         CN: <add-to-group>节点指定应该将操作添加到现有组。一个动作可以添加到多个组中。
             强制的“group-id”属性指定添加操作的组的ID。
             组必须由DefaultActionGroup类的实例实现。
             必选的“anchor”属性指定操作在组中相对于其他操作的位置。它可以有值“first”、“last”、“before”和“after”。
             如果锚点设置为“before”和“after”，并指定当前操作之前或之后插入的操作，则“相对于操作”属性是必需的。-->
    <add-to-group group-id="ToolsMenu" relative-to-action="GenerateJavadoc" anchor="after"/>
      <!-- The <keyboard-shortcut> node specifies the keyboard shortcut
           for the action. An action can have several keyboard shortcuts.
           The mandatory "first-keystroke" attribute specifies the first
           keystroke of the action. The keystrokes are specified according
           to the regular Swing rules.
           The optional "second-keystroke" attribute specifies the second
           keystroke of the action.
           The mandatory "keymap" attribute specifies the keymap for which
           the action is active. IDs of the standard keymaps are defined as
           constants in the com.intellij.openapi.keymap.KeymapManager class. 
           The optional "remove" attribute in the second <keyboard-shortcut>
           element below means the specified shortcut should be removed from 
           the specified action.
           The optional "replace-all" attribute in the third <keyboard-shortcut>
           element below means remove all keyboard and mouse shortcuts from the specified 
           action before adding the specified shortcut.  
            
           <keyboard-shortcut>节点指定操作的键盘快捷键。一个动作可以有几个键盘快捷键。
           强制的"first-keystroke"属性指定操作的第一个击键。击键是根据常规Swing规则指定的。
           可选的"second-keystroke"属性指定动作的第二个击键。
           强制的 "keymap"属性指定活动所在的键映射。标准密钥映射的id被定义为com.intellij.openapi.keymap.KeymapManager类中的常量。
           下面第二个<keyboard-shortcut>元素中的可选"remove"属性表示应该从指定的操作中删除指定的快捷键。
           下面第三个<keyboard-shortcut>元素中的可选"replace-all"属性表示在添加指定的快捷键之前，从指定的操作中删除所有的键盘和鼠标快捷键。
-->
    <!-- Add the first and second keystrokes to all keymaps  -->
    <keyboard-shortcut keymap="$default" first-keystroke="control alt G" second-keystroke="C"/>
    <!-- Except to the "Mac OS X" keymap and its children -->
    <keyboard-shortcut keymap="Mac OS X" first-keystroke="control alt G" second-keystroke="C" remove="true"/>
    <!-- The "Mac OS X 10.5+" keymap and its children will have only this keyboard shortcut for this action.  -->
    <keyboard-shortcut keymap="Mac OS X 10.5+" first-keystroke="control alt G" second-keystroke="C" replace-all="true"/>
    <!-- The <mouse-shortcut> node specifies the mouse shortcut for the
           action. An action can have several mouse shortcuts.
           The mandatory "keystroke" attribute specifies the clicks and
           modifiers for the action. It is defined as a sequence of words
           separated by spaces: 
           "button1", "button2", "button3" for the mouse buttons;
           "shift", "control", "meta", "alt", "altGraph" for the modifier keys;
           "doubleClick" if the action is activated by a double-click of the button.
           The mandatory "keymap" attribute specifies the keymap for which
           the action is active. IDs of the standard keymaps are defined as
           constants in the com.intellij.openapi.keymap.KeymapManager class.
           The "remove" and "replace-all" attributes can also be used in
           a <mouse-shortcut> element. See <keyboard-shortcut> for documentation.  -->
    <mouse-shortcut keymap="$default" keystroke="control button3 doubleClick"/>
  </action>
  <!-- The <group> element defines an action group. <action>, <group> and 
       <separator> elements defined within it are automatically included in the group.
       The mandatory "id" attribute specifies a unique identifier for the action.
       The optional "class" attribute specifies the FQN of
       the class implementing the group. If not specified,
       com.intellij.openapi.actionSystem.DefaultActionGroup is used.
       The optional "text" attribute specifies the text of the group (text
       for the menu item showing the submenu).
       The optional "description" attribute specifies the text which is displayed
       in the status bar when the group has focus.
       The optional "icon" attribute specifies the icon which is displayed on
       the toolbar button or next to the menu group.
       The optional "popup" attribute specifies how the group is presented in
       the menu. If a group has popup="true", actions in it are placed in a
       submenu; for popup="false", actions are displayed as a section of the
       same menu delimited by separators. 
       The optional "compact" attribute specifies whether an action within that group is visible when disabled.
       Setting compact="true" specifies an action in the group isn't visible unless the action is enabled.        -->
  <group class="com.foo.impl.MyActionGroup" id="TestActionGroup" text="Test Group" description="Group with test actions" icon="icons/testgroup.png" popup="true" compact="true">
    <action id="VssIntegration.TestAction" class="com.foo.impl.TestAction" text="My Test Action" description="My test action"/>
    <!-- The <separator> element defines a separator between actions.
         It can also have an <add-to-group> child element. -->
    <separator/>
    <group id="TestActionSubGroup"/>
    <!-- The <reference> element allows to add an existing action to the group.
         The mandatory "ref" attribute specifies the ID of the action to add. -->
    <reference ref="EditorCopy"/>
    <add-to-group group-id="MainMenu" relative-to-action="HelpMenu" anchor="before"/>
  </group>
</actions>
```

### Registering Actions from Code——代码中注册操作
Two steps are required to register an action from code:

**CN:** 注册一个动作需要从代码的步骤: 

* First, an instance of the class derived from `AnAction` must be passed to the `registerAction()` method of [`ActionManager`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionManager.java), to associate the action with an ID.   

**CN:**  首先，派生自`AnAction`的类的实例必须传递给[`ActionManager`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionManager.java)的`registerAction()`方法，以将操作与ID关联起来。

* Second, the action needs to be added to one or more groups. 
  To get an instance of an action group by ID, it is necessary to call `ActionManager.getAction()` and cast the returned value to [`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java).

**CN:**  其次，需要将操作添加到一个或多个组中。
         要通过ID获取操作组的实例，需要调用`ActionManager.getAction()`并将返回值强制转换为[`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java)。

## Building UI from Actions——从操作中创建UI
If a plugin needs to include a toolbar or popup menu built from a group of actions in its user interface, that is accomplished through [`ActionPopupMenu`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionPopupMenu.java) and [`ActionToolbar`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionToolbar.java). 
These objects can be created through calls to the `ActionManager.createActionPopupMenu()` and `createActionToolbar()` methods. 
To get a Swing component from such an object, call the respective `getComponent()` method.

**CN:**  如果一个插件需要在其用户界面中包含一个工具栏或弹出菜单，这可以通过[`ActionPopupMenu`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionPopupMenu.java)和[`ActionToolbar`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionToolbar.java)来完成。
         可以通过调用`ActionManager.createActionPopupMenu()`和`createActionToolbar()`方法来创建这些对象。
         要从这样的对象获取Swing组件，请调用相应的`getComponent()`方法。

If an action toolbar is attached to a specific component (for example, a panel in a tool window), call `ActionToolbar.setTargetComponent()` and pass the instance of the related component as a parameter. 
Setting the target ensures that the state of the toolbar buttons depends on the state of the related component, and not on the current focus location within the IDE frame.

**CN:**  如果操作工具栏附加到特定组件(例如，工具窗口中的面板)，则调用`ActionToolbar.setTargetComponent()`并将相关组件的实例作为参数传递。
         设置目标可以确保工具栏按钮的状态依赖于相关组件的状态，而不依赖于IDE框架内的当前焦点位置。