---
title: Plugin Actions——插件操作
---

The *IntelliJ Platform* provides the concept of _actions_. An action is a class, derived from the [`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java) class, whose `actionPerformed()` method is called when the menu item or toolbar button is selected.

**CN:**  *IntelliJ Platform*提供了 _actions_ 的概念。action是一个类，派生自[`AnAction`](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)类，当菜单项或工具栏按钮被选中时，将调用该类的`actionPerformed()`方法。

Actions are the most common way for a user to invoke the functionality of your plugin. An action can be invoked from
a menu or a toolbar, using a keyboard shortcut, or from the Find Action interface.

**CN:**  动作是用户调用插件功能的最常见方式。可以从中调用操作
         菜单或工具栏，使用键盘快捷键，或从查找操作界面。

Actions are organized into groups, which, in turn, can contain other groups. A group of actions can form a toolbar or a menu. Subgroups of the group can form submenus of a menu. You can find detailed information on how to create and register your actions in the [IntelliJ Platform Action System](/basics/action_system.md).

**CN:**  动作被组织成组，组又可以包含其他组。一组操作可以形成工具栏或菜单。组的子组可以形成菜单的子菜单。您可以在[IntelliJ Platform Action System](/basics/action_system.md)中找到关于如何创建和注册操作的详细信息。