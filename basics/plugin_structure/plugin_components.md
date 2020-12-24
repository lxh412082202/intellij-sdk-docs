---
title: Plugin Components——插件组件
---

> **WARNING** When writing new plugins, you should avoid creating components, and you should migrate existing components in your plugins to services, extensions or listeners (see below).
>
>**CN:**  警告：在编写新插件时，您应该避免创建组件，并且应该将插件中的现有组件迁移到服务、扩展或侦听器(参见下面的内容)。

Plugin components are a legacy feature supported for compatibility with plugins created for older versions of the
IntelliJ Platform. Plugins using components do not support [dynamic loading](dynamic_plugins.md) (the ability to install, update and
uninstall plugins without restarting the IDE). 

**CN:**  插件组件是一个受支持的遗留特性，与为旧版本的
        IntelliJ平台。使用组件的插件不支持[dynamic loading](dynamic_plugins.md)(安装、更新和
        卸载插件而不重新启动IDE)。

Plugin components are defined using `<application-components>`, `<project-components>` and `<module-components>`
tags in a `plugin.xml` file. 

**CN:**  插件组件使用`<application-components>`、`<project-components>`和`<module-components>`定义
        `plugin.xml`文件中的标记。

To migrate your code from components to more modern APIs, please use the following guidelines:

**CN:**  要将您的代码从组件迁移到更现代的api，请使用以下指南:

  * To manage some state or logic that is only needed when the user performs a specific operation,
    use a [Service](plugin_services.md).
    
**CN:**  要管理仅在用户执行特定操作时才需要的某些状态或逻辑，请使用[Service](plugin_services.md)。

  * To store the state of your plugin at the application or project level, use a [Service](plugin_services.md)
    and implement the `PersistentStateComponent` interface. See [Persisting State of Components](/basics/persisting_state_of_components.md) for details.
    
**CN:**  要在应用程序或项目级别存储插件的状态，请使用[Service](plugin_services.md)并实现`PersistentStateComponent`接口。详见[Persisting State of Components](/basics/persisting_state_of_components.md)。

  * To subscribe to events, use a [listener](plugin_listeners.md) or create an [extension](plugin_extensions.md) for a dedicated extension point (for example, `com.intellij.editorFactoryListener`) if one exists for the event you need to subscribe to.
  
**CN:**  若要订阅事件，请使用[listener](plugin_listeners.md)或为专用扩展点(例如`com.intellij.editorFactoryListener`)创建[extension](plugin_extensions.md)(如果存在用于您需要订阅的事件的[extension](plugin_extensions.md))。

  * Executing code on application startup should be avoided whenever possible because it slows down startup.
    Plugin code should only be executed when projects are opened or when the user invokes an action of your plugin. If you can't avoid this, add a [listener](plugin_listeners.md) subscribing to the [AppLifecycleListener](upsource:///platform/platform-impl/src/com/intellij/ide/AppLifecycleListener.java) topic.
    
**CN:**  应该尽可能避免在应用程序启动时执行代码，因为这样会减慢启动速度。插件代码应该只在项目打开或用户调用插件的操作时执行。如果你不能避免这一点，添加一个[listener](plugin_listeners.md)订阅的[AppLifecycleListener](upsource:///platform/platform-impl/src/com/intellij/ide/AppLifecycleListener.java)主题。

  * To execute code when a project is being opened, provide [StartupActivity](upsource:///platform/core-api/src/com/intellij/openapi/startup/StartupActivity.java) implementation and register an [extension](plugin_extensions.md) for the `com.intellij.postStartupActivity` or `com.intellij.backgroundPostStartupActivity` extension point (the latter is supported starting with version 2019.3 of the platform).
  
**CN:**  要在项目打开时执行代码，请提供[StartupActivity](upsource:///platform/core-api/src/com/intellij/openapi/startup/StartupActivity.java)实现并为`com.intellij.postStartupActivity`或`com.intellij.backgroundPostStartupActivity`扩展点注册一个[extension](plugin_extensions.md)(后者从平台的2019.3版本开始支持)。

  * To execute code on project closing or application shutdown, implement the `Disposable` interface in a [Service](plugin_services.md)
    and place the code in the `dispose()` method, or use `Disposer.register()` passing a `Project` or `Application` instance
    as the `parent` argument.
    
**CN:**  要在项目关闭或应用程序关闭时执行代码，请在[Service](plugin_services.md)中实现`Disposable`接口并将代码放在`dispose()`方法中，或使用`Disposer.register()`传递`Project`或`Application`实例作为`parent`参数。

