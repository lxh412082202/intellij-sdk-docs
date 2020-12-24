---
title: Grouping Actions——操作分组
---

If an implementation requires several actions, or there are simply too many actions that overload the menu, the actions can be placed into groups.
This tutorial demonstrates adding an action to an existing group, creating a new action group, and action groups with a variable number of actions.
The sample code discussed in this tutorial is from the code sample [`action_basics`].

**CN:**  如果实现需要多个操作，或者有太多的操作使菜单超载，可以将这些操作放到组中。
      本教程演示如何将操作添加到现有组、创建新操作组和具有可变数量操作的操作组。
      本教程中讨论的示例代码来自代码示例[`action_basics`]。

Some content in this tutorial assumes the reader is familiar with the tutorial for [Creating Actions](working_with_custom_actions.md).

**CN:**  本教程中的一些内容假设读者熟悉[Creating Actions](working_with_custom_actions.md)教程。

* bullet list
{:toc}

## Simple Action Groups——简单操作分组
In this first example, the action group will be available as a top-level menu item, and actions are represented as drop-down menu items.
The group is based on a default IntelliJ Platform implementation.

**CN:**  在第一个示例中，操作组将作为顶级菜单项可用，操作将表示为下拉菜单项。
         这个组基于默认的IntelliJ平台实现。

### Creating Simple Groups——创建简单分组
Grouping can be registered by adding a `<group>` element to the `<actions>` section of a plugin's `plugin.xml` file.
This example has no `class` attribute in the `<group>` element because the IntelliJ Platform framework will supply a default implementation class for the group. 
This default implementation is used if a set of actions belonging to the group is static, i.e. does not change at runtime, which is the majority of cases.
The `id` attribute must be unique, so incorporating the plugin ID or package name is the best practice.

**CN:**  可以通过将一个`<group>`元素添加到插件的`plugin.xml`文件的`<actions>`部分来注册分组。
         这个例子在`<group>`元素中没有`class`属性，因为IntelliJ平台框架将为这个组提供一个默认的实现类。
         如果属于组的一组操作是静态的，即在运行时不改变，则使用此默认实现，这是大多数情况。
         `id`属性必须是惟一的，因此合并插件ID或包名是最佳实践。

The `popup` attribute determines whether actions in the group are placed in a submenu.
The `icon` attribute specifies the FQN of an [`Icon`](/reference_guide/work_with_icons_and_images.md) object to be displayed.
No `compact` attribute is specified, which means this group will support submenus.
See [Registering Actions in plugin.xml](/basics/action_system.md#registering-actions-in-pluginxml) for more information about these attributes.

**CN:**  `popup`属性确定组中的操作是否放在子菜单中。
         `icon`属性指定要显示的[`Icon`](/reference_guide/work_with_icons_and_images.md)对象的FQN。
         没有指定`compact`属性，这意味着该组将支持子菜单。
         有关这些属性的更多信息，请参见[Registering Actions in plugin.xml](/basics/action_system.md#registering-actions-in-pluginxml)。

```xml
    <group id="org.intellij.sdk.action.GroupedActions" text="Static Grouped Actions" popup="true" icon="ActionBasicsIcons.Sdk_default_icon">
    </group>
```

### Binding Action Groups to UI Components——在UI组件绑定操作分组
The following sample shows how to use an `<add-to-group>` element to place a custom action group relative to an entry in the **Tools** menu. 
The attribute `relative-to-action` references the action `id` for `PopupDialogAction`, which is not a native IntelliJ menu entry. 
Rather `PopupDialogAction` is defined in the same [plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml) file.
This group is placed after the single entry for the action `PopupDialogAction`, as defined in the tutorial [Creating Actions](working_with_custom_actions.md#registering-an-action-with-the-new-action-form).

**CN:**  下面的示例展示了如何使用`<add-to-group>`元素将自定义动作组相对于 **Tools** 菜单中的一个条目放置。
         属性`relative-to-action`引用`PopupDialogAction`的动作`id`，它不是一个本地IntelliJ菜单项。
         相反，`PopupDialogAction`是在同一个[plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml)文件中定义的。
         这个组位于action `PopupDialogAction`的单个条目之后，如教程[Creating Actions](working_with_custom_actions.md#registering-an-action-with-the-new-action-form)中定义的那样。

```xml
    <group id="org.intellij.sdk.action.GroupedActions" text="Static Grouped Actions" popup="true" icon="ActionBasicsIcons.Sdk_default_icon">
      <add-to-group group-id="ToolsMenu" anchor="after" relative-to-action="org.intellij.sdk.action.PopupDialogAction"/>
    </group>
```

### Adding a New Action to the Static Grouped Actions——向静态分组操作添加新操作
The `PopupDialogAction` implementation will be reused and registered in the newly created static group. 
The `id` attribute for the reused `PopupDialogAction` implementation is set to a unique value, `org.intellij.sdk.action.GroupPopDialogAction`.
This value differentiates this new `<action>` entry from the `id` previously used to register this action implementation in the [Creating Actions](working_with_custom_actions.md#registering-an-action-with-the-new-action-form) tutorial. 
A unique `id` supports reuse of action classes in more than one menu or group.
The action in this group will display the menu text "A Group Action".

**CN:**  将在新创建的静态组中重用和注册`PopupDialogAction`实现。
         重用`PopupDialogAction`实现的`id`属性被设置为一个惟一的值`org.intellij.sdk.action.GroupPopDialogAction`。
         这个值将这个新的`<action>`条目与以前用于在[Creating Actions](working_with_custom_actions.md#registering-an-action-with-the-new-action-form)教程中注册这个操作实现的`id`区别开来。
         唯一的`id`支持在多个菜单或组中重用action类。
         该组中的操作将显示菜单文本"A Group Action"。

```xml
    <group id="org.intellij.sdk.action.GroupedActions" text="Static Grouped Actions" popup="true" icon="ActionBasicsIcons.Sdk_default_icon">
      <add-to-group group-id="ToolsMenu" anchor="after" relative-to-action="org.intellij.sdk.action.PopupDialogAction"/>
      <action class="org.intellij.sdk.action.PopupDialogAction" id="org.intellij.sdk.action.GroupPopDialogAction"
              text="A Group Action" description="SDK static grouped action example" icon="ActionBasicsIcons.Sdk_default_icon">
      </action>
    </group>
```

After performing the steps described above the action group and its content will be available in the **Tools** menu.
The underlying `PopupDialogAction` implementation is reused for two entries in the **Tools** menu:

**CN:**  执行上述步骤后，行动组及其内容将在 **Tools** 菜单中可用。
         底层的`PopupDialogAction`实现在 **Tools** 菜单中的两个条目中被重用:

* Once for the top menu entry **Tools \| Pop Dialog Action** with the action `id` equal to `org.intellij.sdk.action.PopupDialogAction` as set in the [Creating Actions](/tutorials/action_system/working_with_custom_actions.md#registering-an-action-with-the-new-action-form) tutorial.

**CN:**  一次为顶部菜单项 **Tools \| Pop Dialog Action** 与行动`id`等于`org.intellij.sdk.action.PopupDialogAction`在[Creating Actions](/tutorials/action_system/working_with_custom_actions.md#registering-an-action-with-the-new-action-form)教程中设置。

* A section time for the menu entry **Tools \| Static Grouped Actions \| A Group Action** with the action `id` equal to `org.intellij.sdk.action.GroupPopDialogAction`.

**CN:**  动作`id`等于`org.intellij.sdk.action.GroupPopDialogAction`的菜单条目 **Tools \| Static Grouped Actions \| A Group Action** 的一段时间。

![Simple Action Group](img/grouped_action.png){:width="550px"}
    
  
## Implementing Custom Action Group Classes——实现自定义操作组类
In some cases, the specific behavior of a group of actions needs to depend on the context.
The solution is analagous to making a [single action entry dependent on context](working_with_custom_actions.md#extending-the-update-method). 

**CN:**  在某些情况下，一组操作的特定行为需要依赖于上下文。
         这个解决方案不利于制造[single action entry dependent on context](working_with_custom_actions.md#extending-the-update-method)。

The steps below show how to make a group of actions available and visible if certain conditions are met.
In this case, the condition is having an instance of an editor is available. 
This condition is needed because the custom action group will be added to an IntelliJ menu that is only enabled for editing.

**CN:**  下面的步骤展示了如何在满足某些条件时使一组操作可用并可见。
         在本例中，条件是有一个可用的编辑器实例。
         这个条件是必须的，因为自定义动作组将被添加到一个IntelliJ菜单中，该菜单只允许编辑。

### Extending DefaultActionGroup——扩展DefaultActionGroup
The [`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java) is an implementation of [`ActionGroup`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionGroup.java).
The `DefaultActionGroup` class is used to add child actions and separators between them to a group.
This class is used if a set of actions belonging to the group does not change at runtime, which is the majority of cases.

**CN:**  [`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java)是[`ActionGroup`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionGroup.java)的一个实现。
         `DefaultActionGroup`类用于将子操作和它们之间的分隔符添加到组中。
         如果属于组的一组操作在运行时不更改，则使用该类，这是大多数情况。

As an example, extend [`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java) 
to create the `CustomDefaultActionGroup` class in the `action_basics` code sample:

**CN:**  例如，扩展[`DefaultActionGroup`](upsource:///platform/platform-api/src/com/intellij/openapi/actionSystem/DefaultActionGroup.java)
         在`action_basics`代码样本中创建`CustomDefaultActionGroup`类:

```java
  public class CustomDefaultActionGroup extends DefaultActionGroup {
    @Override
    public void update(AnActionEvent event) {
      // Enable/disable depending on whether user is editing...
    }
  }
```

### Registering the Custom Action Group——注册自定义操作组
As in the case with the static action group, the action `<group>` should be declared in the `<actions>` section of the`plugin.xml` file, for example, the [action_basics](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml) plugin. 
The declaration below shows:

**CN:**  在静态动作组的情况下，动作`<group>`应该在`plugin.xml`文件的`<actions>`部分声明，例如，[action_basics](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml)插件。
         声明如下:

* The presence of the `class` attribute in the `<group>` element, which tells the IntelliJ Platform framework to use `CustomDefaultActionGroup` rather than the default implementation.

**CN:**  `<group>`元素中存在的`class`属性，它告诉IntelliJ平台框架使用`CustomDefaultActionGroup`而不是默认实现。

* Setting the group's `popup` attribute to allow submenus. 

**CN:**  设置组的`popup`属性以允许子菜单。

* The `<add-to-group>` element specifies adding the group in the first position of the existing `EditorPopupMenu`.

**CN:**  `<add-to-group>`元素指定在现有`EditorPopupMenu`的第一个位置添加组。

```xml
   <group id="org.jetbrains.tutorials.actions.ExampleCustomDefaultActionGroup" 
        class="org.jetbrains.tutorials.actions.CustomDefaultActionGroup" popup="true"
        text="Example Custom DefaultActionGroup" description="Custom DefaultActionGroup Demo">
      <add-to-group group-id="EditorPopupMenu" anchor="first"/>
    </group>
```

### Adding Actions to the Custom Group——在自定义组中添加操作
As in [Static Grouped Actions](#adding-a-new-action-to-the-static-grouped-actions), the `PopupDialogAction` action is added as an `<action>` element in the `<group>` element. 
In the declaration below, note:

**CN:**  在[Static Grouped Actions](#adding-a-new-action-to-the-static-grouped-actions)中，`PopupDialogAction`动作作为`<action>`元素添加到`<group>`元素中。
         在下面的声明中，请注意:

  * The `class` attribute in the `<action>` element has the same FQN to reuse this action implementation.

**CN:**  `<action>`元素中的`class`属性具有相同的FQN来重用此操作实现。

  * The `id` attribute is unique to distinguish it from other uses of the implementation in the Action System.
  
**CN:**  `id`属性是惟一的，以区别于操作系统中实现的其他用途。
   
```xml
    <group id="org.intellij.sdk.action.CustomDefaultActionGroup" class="org.intellij.sdk.action.CustomDefaultActionGroup" popup="true"
            text="Popup Grouped Actions" description="Custom defaultActionGroup demo" icon="ActionBasicsIcons.Sdk_default_icon">
      <add-to-group group-id="EditorPopupMenu" anchor="first"/>
      <action class="org.intellij.sdk.action.PopupDialogAction" id="org.intellij.sdk.action.CustomGroupedAction"
              text="A Popup Action" description="SDK popup grouped action example" icon="ActionBasicsIcons.Sdk_default_icon"/>
    </group>
```

### Providing Specific Behavior for the Custom Group——为自定义组提供特定的行为
Override the `CustomDefaultActionGroup.update()` method to make the group visible only if there's an instance of the editor available. 
Also, a custom icon is added to demonstrate that icons can be changed depending on the action context:

**CN:**  重写`CustomDefaultActionGroup.update()`方法，使组仅在有编辑器实例可用时才可见。
         此外，还添加了一个自定义图标，以演示可以根据动作上下文更改图标:

```java
public class CustomDefaultActionGroup extends DefaultActionGroup {
  @Override
  public void update(AnActionEvent event) {
    // Enable/disable depending on whether user is editing
    Editor editor = event.getData(CommonDataKeys.EDITOR);
    event.getPresentation().setEnabled(editor != null);
    // Take this opportunity to set an icon for the menu entry.
    event.getPresentation().setIcon(ActionBasicsIcons.Sdk_default_icon);
  }
}
```

After compiling and running the code sample above and opening a file in the editor and right-clicking, the **Editing** menu will pop up containing a new group of actions in the first position. 
The new group will also have an icon:

**CN:**  在编译和运行上面的代码示例并在编辑器中打开一个文件并右击之后， **Editing** 菜单将弹出，其中在第一个位置包含一组新的操作。
         新组也将有一个图标:

![Custom Action Group](img/editor_popup_menu.png){:width="600px"}
  

## Action Groups with Variable Actions Sets——具有可变操作集的操作组
If a set of actions belonging to a custom group will vary depending on the context, the group must extend [`ActionGroup`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionGroup.java).
The set of actions in the `ActionGroup` is dynamically defined.

**CN:**  如果属于自定义组的一组操作将根据上下文而变化，则该组必须扩展[`ActionGroup`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/ActionGroup.java)。
         `ActionGroup`中的操作集是动态定义的。

### Creating Variable Action Group——创建可变操作组
To create a group of actions with a variable number of actions, extend `ActionGroup`.
For example, as in the `action_basics` class [`DynamicActionGroup`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/DynamicActionGroup.java) code:

**CN:**  若要创建具有可变数量操作的操作组，请扩展`ActionGroup`。
         例如，在`action_basics`类[`DynamicActionGroup`](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/java/org/intellij/sdk/action/DynamicActionGroup.java)代码中:

```java
public class DynamicActionGroup extends ActionGroup {
}
```

### Registering a Variable Action Group——注册可变操作组
To register the dynamic menu group, a `<group>` attribute needs to be placed in the `<actions>` section of [plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml).
When enabled, this group will appear at the entry just below the [Static Grouped Actions](#binding-action-groups-to-ui-components) in the **Tools** menu:

**CN:**  为了注册动态菜单组，需要在[plugin.xml](https://github.com/JetBrains/intellij-sdk-docs/blob/master/code_samples/action_basics/src/main/resources/META-INF/plugin.xml)的`<actions>`部分放置一个`<group>`属性。
         当启用时，这个组将出现在 **Tools** 菜单[Static Grouped Actions](#binding-action-groups-to-ui-components)下面的入口:

```xml
    <group id="org.intellij.sdk.action.DynamicActionGroup" class="org.intellij.sdk.action.DynamicActionGroup" popup="true"
            text="Dynamically Grouped Actions" description="SDK dynamically grouped action example" icon="ActionBasicsIcons.Sdk_default_icon">
      <add-to-group group-id="ToolsMenu" anchor="after" relative-to-action="org.intellij.sdk.action.GroupedActions"/>
    </group>
```

> **Warning** If a`<group>` element's `class` attribute names a class derived from `ActionGroup`, then any static `<action>` declarations in that group will throw an exception. 
For a statically defined group, use `DefaultActionGroup`.
>
>**CN:**  警告：如果`<group>`元素的`class`属性命名了一个派生自`ActionGroup`的类，那么该组中的任何静态`<action>`声明都将抛出异常。
             对于静态定义的组，使用`DefaultActionGroup`。

### Adding Child Actions to the Dynamic Group——在动态组中添加子操作
To add actions to the `DynamicActionGroup`, a non-empty array of `AnAction` instances should be returned from the `DynamicActionGroup.getChildren()` method. 
Here again, reuse the `PopupDialogAction` implementation.
This use case is why `PopupDialogAction` overrides a constructor:

**CN:**  要向`DynamicActionGroup`添加操作，应该从`DynamicActionGroup.getChildren()`方法返回一个非空的`AnAction`实例数组。
         在这里，再次重用`PopupDialogAction`实现。
         这个用例是为什么`PopupDialogAction`覆盖了一个构造函数:

```java
public class DynamicActionGroup extends ActionGroup {
  @NotNull
  @Override
  public AnAction[] getChildren(AnActionEvent e) {
    return new AnAction[]{ new PopupDialogAction("Action Added at Runtime",
                                                 "Dynamic Action Demo",
                                                 ActionBasicsIcons.Sdk_default_icon) };
  }
}
```

After providing the implementation of `DynamicActionGroup` and making it return a non-empty array of actions, the third position in the **Tools** Menu will contain a new group of actions:

**CN:**  在提供了`DynamicActionGroup`的实现并使其返回一个非空的动作数组后， **Tools** 菜单中的第三个位置将包含一组新的动作:

![Dynamic Action Group](img/dynamic_action_group.png){:width="600px"}
