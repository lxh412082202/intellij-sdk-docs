---
title: Plugin Listeners——插件侦听器
---

> **NOTE** Defining listeners in `plugin.xml` is supported starting with the version 2019.3 of the platform.
>
>**CN:**  注意：从平台的2019.3版本开始支持在`plugin.xml`中定义监听器。


_Listeners_ allow plugins to declaratively subscribe to events delivered through the
[message bus](/reference_guide/messaging_infrastructure.md). You can define both application- and project-level
listeners.

**CN:**   _Listeners_ 允许插件声明订阅通过[message bus](/reference_guide/messaging_infrastructure.md)发布的事件。您可以定义应用程序级侦听器和项目级侦听器。

Declarative registration of listeners allows you to achieve better performance compared to registering listeners
from code, because listener instances are created lazily (the first time an event is sent to the topic), and not
during application startup or project opening.

**CN:**  声明式侦听器注册允许您获得比从代码中注册侦听器更好的性能，因为侦听器实例是延迟创建的(事件第一次发送到主题时)，而不是在应用程序启动或项目打开期间创建的。

## Defining Application-Level Listeners——定义应用程序级的侦听器

To define an application-level listener, add the following section to your `plugin.xml`:

**CN:**  要定义应用程序级监听器，请将以下部分添加到您的`plugin.xml`:

```xml
<applicationListeners>
  <listener class="myPlugin.MyListenerClass" topic="BaseListenerInterface"/>
</applicationListeners>
```

The `topic` attribute specifies the listener interface corresponding to the type of events you want to receive.
Normally, this is the interface used as the type parameter of the `Topic` instance for the type of events.
The `class` attribute specifies the class in your plugin that implements the listener interface and receives
the events. 

**CN:**  `topic`属性指定与您希望接收的事件类型相对应的侦听器接口。
         通常，这是用作事件类型的`Topic`实例的类型参数的接口。
         `class`属性指定插件中实现侦听器接口并接收事件的类。

As a specific example, if you want to receive events about all changes in the virtual file system, you need
to implement the `BulkFileListener` interface, corresponding to the topic `VirtualFileManager.VFS_CHANGES`.
To subscribe to this topic from code, you could use something like the following snippet:

**CN:**  作为一个具体的例子，如果您想要接收关于虚拟文件系统中所有更改的事件，您需要实现与主题`VirtualFileManager.VFS_CHANGES`对应的`BulkFileListener`接口。
         要从代码中订阅这个主题，您可以使用如下代码片段:

```java
messageBus.connect().subscribe(VirtualFileManager.VFS_CHANGES, new BulkFileListener() {
    @Override
    public void after(@NotNull List<? extends VFileEvent> events) {
        // handle the events
    }
});
```

To use declarative registration, you no longer need to reference the `Topic` instance. Instead, you refer directly
to the listener interface class:

**CN:**  要使用声明式注册，您不再需要引用`Topic`实例。相反，你可以直接引用listener接口类:

```xml
<applicationListeners>
  <listener class="myPlugin.MyVfsListener" topic="com.intellij.openapi.vfs.newvfs.BulkFileListener"/>
</applicationListeners>
```

Then you provide the listener implementation as a top-level class:

**CN:**  然后，您将侦听器实现作为顶级类提供:

```java
public class MyVfsListener implements BulkFileListener {
    @Override
    public void after(@NotNull List<? extends VFileEvent> events) {
        // handle the events
    }
}
```

## Defining Project-level Listeners——定义项目级侦听器

Project-level listeners are registered in the same way, except that the top-level tag is 
`<projectListeners>`. They can be used to listen to project-level events, for example, tool window operations:

**CN:**  项目级侦听器以相同的方式注册，不同之处在于顶级标记是`<projectListeners>`。它们可以用来监听项目级的事件，例如，工具窗口操作:

```xml
<projectListeners>
    <listener class="MyToolwindowListener" topic="com.intellij.openapi.wm.ex.ToolWindowManagerListener" />
</projectListeners>
```

The class implementing the listener interface can define a one-argument constructor accepting a `Project`,
and it will receive the instance of the project for which the listener is created:

**CN:**  实现侦听器接口的类可以定义一个接受`Project`的单参数构造函数，它将接收为其创建侦听器的项目实例:

```java
public class MyToolwindowListener implements ToolWindowManagerListener {
    private final Project project;

    public MyToolwindowListener(Project project) {
        this.project = project;
    }

    @Override
    public void stateChanged() {
        // handle the state change
    }
}
```