---
title: Creating Actions——创建操作
---

## Introduction——介绍
Plugins can add actions to existing IDE menus and toolbars, as well as add new menus and toolbars. 
The IntelliJ Platform calls the actions of plugins in response to user interactions with the IDE. 
However, the actions of a plugin must first be defined and registered with the IntelliJ Platform. 

**CN:**  插件可以向现有的IDE菜单和工具栏添加操作，也可以添加新的菜单和工具栏。
         IntelliJ平台调用插件的动作来响应用户与IDE的交互。
         然而，插件的行为必须首先在IntelliJ平台上定义和注册。

Using the SDK code sample `action_basics`, this tutorial illustrates the steps to create an action for a plugin. 

**CN:**  使用SDK代码示例`action_basics`，本教程演示了为插件创建操作的步骤。

* bullet list
{:toc}

## Creating a Custom Action——创建一个自定义的操作
Custom actions extend the abstract class [`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java). 
Classes that extend it should override `AnAction.update()`, and must override `AnAction.actionPerformed()`. 

**CN:**  自定义动作扩展了抽象类[`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)。
         扩展它的类应该覆盖`AnAction.update()`，并且必须覆盖`AnAction.actionPerformed()`。

* The `update()` method implements the code that enables or disables an action.

**CN:**  `update()`方法实现了启用或禁用操作的代码。

* The `actionPerformed()` method implements the code that executes when an action is invoked by the user.

**CN:**  `actionPerformed()`方法实现用户调用操作时执行的代码。


As an example, [`PopupDialogAction`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java) overrides `AnAction` for the `action_basics` code sample.

**CN:**  例如，对于`action_basics`代码示例，[`PopupDialogAction`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/PopupDialogAction.java)覆盖了`AnAction`。

```java
public class PopupDialogAction extends AnAction {
   
  @Override
  public void update(AnActionEvent e) {
    // Using the event, evaluate the context, and enable or disable the action.
    // 使用事件，评估上下文，并启用或禁用操作。
  }
  
  @Override
  public void actionPerformed(@NotNull AnActionEvent e) {
    // Using the event, implement an action. For example, create and show a dialog.
    // 使用事件，实现一个动作。例如，创建并显示一个对话框。
  }
  
}
```

> **Warning** `AnAction` classes do not have class fields of any kind. This restriction prevents memory leaks.
For more information about why, see [Action Implementation](/basics/action_system.md#action-implementation). 
>
>**CN:**  警告：`AnAction`类没有任何类型的类字段。这个限制可以防止内存泄漏。
             有关原因的更多信息，请参见[Action Implementation](/basics/action_system.md#action-implementation)。

At this stage, `update()` implicitly defaults to always enable this action.
The implementation of `actionPerformed()` does nothing.
These methods will be more fully implemented in [Developing the AnAction Methods](#developing-the-anaction-methods) below.

**CN:**  在此阶段，`update()`隐式默认总是启用此操作。
         `actionPerformed()`的实施毫无作用。
         这些方法将在下面的[Developing the AnAction Methods](#developing-the-anaction-methods)中得到更充分的实现。


Before fleshing out those methods, to complete this minimal implementation `PopupDialogAction` must be registered with the IntelliJ Platform.

**CN:**  在充实这些方法之前，要完成这个最小的实现`PopupDialogAction`必须在IntelliJ平台上注册。


## Registering a Custom Action——注册一个自定义的操作
Actions can be registered by declaring them in code or by declaring them in the `<actions>` section of a plugin configuration (`plugin.xml`) file. 
This section describes using IDE tooling - the New Action Form - to add a declaration to the `plugin.xml` file, and then tuning registration attributes manually.
A more comprehensive explanation of action registration is available in the [Action Registration](/basics/action_system.md#registering-actions) section of this guide.

**CN:**  动作可以通过在代码中声明或者在插件配置(`plugin.xml`)文件的`<actions>`部分声明来注册。
         本节介绍如何使用IDE工具(新的操作表单)将声明添加到`plugin.xml`文件中，然后手动调优注册属性。
         本指南的[Action Registration](/basics/action_system.md#registering-actions)部分提供了对操作注册的更全面的解释。

### Registering an Action with the New Action Form——使用新的操作表单注册一个操作
IntelliJ IDEA has an embedded inspection that spots unregistered actions. 
Verify the inspection is enabled at **Settings/Preferences \| Editor \| Inspections \| Plugin DevKit \| Code \| Component/Action not registered**. 
Here is an example for this stage of the `PopupDialogAction` class:

**CN:**  IntelliJ IDEA有一个嵌入式检查，可以发现未注册的操作。
         验证检查在 **Settings/Preferences \| Editor \| Inspections \| Plugin DevKit \| Code \| Component/Action not registered** 是启用的。
         这里是一个例子的`PopupDialogAction`类的这一阶段:

!["Action never used" inspection](img/action_never_used.png){:width="600px"}

To register `PopupDialogAction` and set up its basic attributes press **Alt + Shift + Enter**.
Fill out the **New Action** form to set up the parameters for `PopupDialogAction`: 

**CN:**  注册`PopupDialogAction`并设置其基本属性按 **Alt + Shift + Enter** 。
         填写 **New Action** 表格，设置`PopupDialogAction`参数:


![New Action](img/new_action.png){:width="800px"}

The fields of the form are:

**CN:**  表格的范围如下:

* _Action ID_ - Every action must have a unique ID.
  If the action class is used in only one place in the IDE UI, then the class FQN is a good default for the ID.
  Using the action class in multiple places requires mangling the ID, such as adding a suffix to the FQN, for each ID.

**CN:**  _Action ID_ - 每个操作必须有一个唯一的ID。
                       如果action类只在IDE UI的一个地方使用，那么类FQN是ID的一个很好的默认值。
                       在多个地方使用action类需要修改ID，比如为每个ID添加一个后缀到FQN。

* _Class Name_ - The FQN implementation class for the action.
  If the same action is used in multiple places in the IDE UI, the implementation FQN can be reused with a different _Action ID_.

**CN:**  动作的FQN实现类。
         如果在IDE UI的多个地方使用相同的操作，则可以使用不同的 _Action ID_ 重用实现FQN。

* _Name_ - The text to appear in the menu.

**CN:**  要显示在菜单中的文本。

* _Description_ - Hint text to be displayed.

**CN:**  要显示的提示文本。

* _Add to Group_ - The action group - menu or toolbar - to which the action is added.
  Clicking in the list of groups and typing will invoke a search, such as "ToolsMenu." 

**CN:**  添加操作的操作组(菜单或工具栏)。
         在组列表中单击并键入将调用一个搜索，如“ToolsMenu”。

* _Anchor_ - Where the menu action should be placed in the **Tools** menu relative to the other actions in that menu.  

**CN:**  相对于该菜单中的其他操作，菜单操作应该放在 **Tools** 菜单中的什么位置。 


In this case, `PopupDialogAction` would be available in the **Tools** menu, it would be placed at the top, and would have no shortcuts.

**CN:**  在这种情况下，`PopupDialogAction`将在 **Tools** 菜单中可用，它将放在顶部，并且没有快捷方式。


After finishing the **New Action** form and applying the changes, the `<actions>` section of the plugin's `plugins.xml` file
would contain:

**CN:**  完成 **New Action** 表单并应用更改后，插件的`plugins.xml`文件的`<actions>`部分
         将包含:

```xml
  <actions>
    <action id="org.intellij.sdk.action.PopupDialogAction" class="org.intellij.sdk.action.PopupDialogAction" 
          text="Pop Dialog Action" description="SDK action example">
      <add-to-group group-id="ToolsMenu" anchor="first"/>
    </action>
  </actions>
```

The `<action>` element declares the _Action ID_ (`id`,) _Class Name_ (`class`,) _Name_ (`text`,) and _Description_ from the **New Action** form.
The `<add-to-group>` element declares where the action will appear and mirrors the names of entries from the form. 

**CN:**  `<action>`元素从 **New Action** 表单中声明 _Action ID_  (`id`，)  _Class Name_  (`class`，)  _Name_  (`text`，)和 _Description_ 。
         `<add-to-group>`元素声明操作将出现在何处，并从表单中镜像条目的名称。


This declaration is adequate, but adding more attributes is discussed in the next section.

**CN:**  这个声明已经足够了，但是下一节将讨论添加更多属性。


### Setting Registration Attributes Manually——手动设置注册属性
An action declaration can be added manually to the `plugin.xml` file.
An exhaustive list of declaration elements and attributes is presented in [Registering Actions in plugin.xml](/basics/action_system.md#registering-actions-in-pluginxml).
Attributes are added by selecting them from the **New Action** form, or by editing the registration declaration directly in the plugin.xml file.

**CN:**  可以手动将操作声明添加到`plugin.xml`文件中。
         在[Registering Actions in plugin.xml](/basics/action_system.md#registering-actions-in-pluginxml)中给出了声明元素和属性的详尽列表。
         属性是通过从 **New Action** 表单中选择它们来添加的，或者通过直接在plugin.xml文件中编辑注册声明来添加的。

The `<action>` declaration for `PopupDialogAction` in the `action_basics` [plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml) file.
It also contains an attribute for an [`Icon`](/reference_guide/work_with_icons_and_images.md), and encloses elements declaring keyboard and mouse shortcuts. 
The full declaration is:

**CN:**  `action_basics` [plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml)文件中`PopupDialogAction`的`<action>`声明。
         它还包含一个[`Icon`](/reference_guide/work_with_icons_and_images.md)属性，并包含声明键盘和鼠标快捷键的元素。
         声明全文如下:

```xml
    <action id="org.intellij.sdk.action.PopupDialogAction" class="org.intellij.sdk.action.PopupDialogAction"
            text="Pop Dialog Action" description="SDK action example" icon="ActionBasicsIcons.Sdk_default_icon">
      <keyboard-shortcut first-keystroke="control alt A" second-keystroke="C" keymap="$default"/>
      <mouse-shortcut keystroke="control button3 doubleClick" keymap="$default"/>
      <add-to-group group-id="ToolsMenu" anchor="first"/>
    </action>
```


## Testing the Minimal Custom Action Implementation——测试最小的自定义操作实现
After performing the steps described above, compile and run the plugin to see the newly created action available as a Tools Menu item:

**CN:**  执行上述步骤后，编译并运行插件，以查看新创建的操作作为一个工具菜单项:


!["Register action" quick fix](img/tools_menu_item_action.png){:width="350px"}

Selecting the action from the menu or using the keyboard shortcuts won't do anything because the implementations are empty.
However, it confirms the new entry appears at **Tools \| Pop Dialog Action**

**CN:**  从菜单中选择操作或使用键盘快捷键不会做任何事情，因为实现是空的。
         然而，它证实了新条目出现在 **Tools \| Pop Dialog Action** 


## Developing the AnAction Methods——开发AnAction方法
At this point, the new action `PopupDialogAction` is registered with the IntelliJ Platform and functions in the sense that  `update()` and `actionPerformed()` are called in response to user interaction with the IDE Tools menu.
However, neither method implements any code to perform useful work.

**CN:**  此时，新的动作`PopupDialogAction`被IntelliJ平台和功能注册了，因为用户通过IDE工具菜单进行交互时，需要调用`update()`和`actionPerformed()`。
         但是，这两种方法都没有实现任何代码来执行有用的工作。

This section describes adding useful code to these methods.
The `update()` method defaults to always enable the action, which is satisfactory for intermediate testing.
So `actionPerformed()` will be developed first.

**CN:**  本节描述如何向这些方法添加有用的代码。
         `update()`方法默认总是启用操作，这对于中间测试来说是令人满意的。
         所以`actionPerformed()`将首先被开发。

### Extending the actionPerformed Method——扩展actionPerformed方法
Adding code to the `PopupDialogAction.actionPerformed()` method makes the action do something useful. 
The code below gets information from the `anActionEvent` input parameter and constructs a message dialog.
A generic icon, and the `dlgMsg` and `dlgTitle` attributes from the invoking menu action are displayed.
However, code in this method could manipulate a project, invoke an inspection, change the contents of a file, etc.

**CN:**  向`PopupDialogAction.actionPerformed()`方法添加代码可以使操作做一些有用的事情。
         下面的代码从`anActionEvent`输入参数获取信息并构造一个消息对话框。
         将显示一个通用图标，以及调用菜单操作的`dlgMsg`和`dlgTitle`属性。
         但是，此方法中的代码可以操作项目、调用检查、更改文件内容等。


For demonstration purposes the `AnActionEvent.getData()` method tests if a [`Navigatable`](upsource:///platform/core-api/src/com/intellij/pom/Navigatable.java) object is available. 
If so, information about the selected element is added to the dialog. 

**CN:**  为了演示目的，`AnActionEvent.getData()`方法测试[`Navigatable`](upsource:///platform/core-api/src/com/intellij/pom/Navigatable.java)对象是否可用。
         如果是，则将有关所选元素的信息添加到对话框中。

See [Determining the Action Context](/basics/action_system.md#determining-the-action-context) for more information about accessing information from the `AnActionEvent` input parameter. 

**CN:**  有关从`AnActionEvent`输入参数访问信息的更多信息，请参见[Determining the Action Context](/basics/action_system.md#determining-the-action-context)。

```java
  @Override
  public void actionPerformed(@NotNull AnActionEvent event) {
    // Using the event, create and show a dialog
    Project currentProject = event.getProject();
    StringBuffer dlgMsg = new StringBuffer(event.getPresentation().getText() + " Selected!");
    String dlgTitle = event.getPresentation().getDescription();
    // If an element is selected in the editor, add info about it.
    Navigatable nav = event.getData(CommonDataKeys.NAVIGATABLE);
    if (nav != null) {
      dlgMsg.append(String.format("\nSelected Element: %s", nav.toString()));
    }
    Messages.showMessageDialog(currentProject, dlgMsg.toString(), dlgTitle, Messages.getInformationIcon());
  }
```

### Extending the update Method——扩展update方法
Adding code to `PopupDialogAction.update()` gives finer control of the action's visibility and availability.
The action's state and(or) presentation can be dynamically changed depending on the context. 

**CN:**  向`PopupDialogAction.update()`添加代码可以更好地控制操作的可见性和可用性。
         可以根据上下文动态更改操作的状态和(或)表示。


> **Warning** This method needs to _execute very quickly_. For more information about this constraint, see the warning in [Overriding the AnAction.update Method](/basics/action_system.md#overriding-the-anactionupdate-method).
>
>**CN:**  警告：这个方法需要 _execute very quickly_ 。有关此约束的更多信息，请参见[Overriding the AnAction.update Method](/basics/action_system.md#overriding-the-anactionupdate-method)中的警告。

In this example, the `update()` method relies on a `Project` object being available. 
This requirement means the user must have at least one project open in the IDE for the `PopupDialogAction` to be available.
So the `update()` method disables the action for contexts where a `Project` object isn't defined.

**CN:**  在本例中，`update()`方法依赖于一个可用的`Project`对象。
         这个需求意味着用户必须在IDE中打开至少一个项目，以便`PopupDialogAction`可用。
         因此，`update()`方法在没有定义`Project`对象的上下文中禁用操作。


The availability (enabled and visible) is set on the `Presentation` object.
Setting both the enabled state and visibility produces consistent behavior despite possible host menu settings, as discussed in [Grouping Actions](/basics/action_system.md#grouping-actions).

**CN:**  在`Presentation`对象上设置可用性(启用和可见)。
         设置启用状态和可见性将产生一致的行为，尽管可能存在主机菜单设置，如[Grouping Actions](/basics/action_system.md#grouping-actions)中所述。

```java
  @Override
  public void update(AnActionEvent e) {
    // Set the availability based on whether a project is open
    Project project = e.getProject();
    e.getPresentation().setEnabledAndVisible(project != null);
  }
```

The `update()` method does not check to see if a `Navigatable` object is available before enabling `PopupDialogAction`. 
This check is unnecessary because using the `Navigatable` object is opportunistic in `actionPerformed()`. 
See [Determining the Action Context](/basics/action_system.md#determining-the-action-context) for more information about accessing information from the `AnActionEvent` input parameter. 

**CN:**  在启用`PopupDialogAction`之前，`update()`方法不会检查`Navigatable`对象是否可用。
         这种检查是不必要的，因为在`actionPerformed()`中使用`Navigatable`对象是有机会的。
         有关从`AnActionEvent`输入参数访问信息的更多信息，请参见[Determining the Action Context](/basics/action_system.md#determining-the-action-context)。

### Other Method Overrides——其他方法覆盖
A constructor is overridden in `PopupDialogAction`, but this is an artifact of reusing this class for a dynamically created menu action.
Otherwise, overriding constructors for `AnAction` is not required.

**CN:**  构造函数在`PopupDialogAction`中被覆盖，但这是为动态创建的菜单操作重用该类的结果。
         否则，不需要覆盖`AnAction`的构造函数。

## Testing the Custom Action——测试自定义动作
After compiling and running the plugin project and invoking the action, the dialog will pop up:

**CN:**  编译并运行插件项目并调用操作后，对话框将弹出:

![Action performed](img/action_performed.png){:width="800px"}
