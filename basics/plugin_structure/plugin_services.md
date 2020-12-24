---
title: Plugin Services——插件服务
---

A _service_ is a plugin component loaded on demand when your plugin calls the `getService()` method of the [`ServiceManager`](upsource:///platform/core-api/src/com/intellij/openapi/components/ServiceManager.java) class.

**CN:**  当你的插件调用[`ServiceManager`](upsource:///platform/core-api/src/com/intellij/openapi/components/ServiceManager.java)类的`getService()`方法时， _service_ 是一个按需加载的插件组件。

The *IntelliJ Platform* ensures that only one instance of a service is loaded even though the service is called several times.

**CN:**  即使服务被多次调用，*IntelliJ Platform*也确保只加载一个服务实例。

A service must have an implementation class which is used for service instantiation. 
A service may also have an interface class which is used to obtain the service instance and provides API of the service. The interface and implementation classes are specified in the `plugin.xml` file.

**CN:**  服务必须有一个用于服务实例化的实现类。
         服务也可以有一个接口类，用于获取服务实例并提供服务的API。接口和实现类在`plugin.xml`文件中指定。

The *IntelliJ Platform* offers three types of services: _application level_ services, _project level_ services, and _module level_ services.

**CN:**  *IntelliJ Platform*提供三种服务: _application level_ 服务、 _project level_ 服务和 _module level_ 服务。

Please consider not using module level services because it can lead to increased memory usage for projects with many modules.

**CN:**  请考虑不使用模块级服务，因为它可能会导致有许多模块的项目的内存使用量增加。

## Light Services——Light服务

> **NOTE** Light Services are available since IntelliJ Platform 2019.3.
>
>**CN:**  注意，Light服务从IntelliJ平台2019.3开始提供。

A service not going to be overridden does not need to be registered in `plugin.xml` (see [How To Declare a Service](#how-to-declare-a-service)).

**CN:**  不被覆盖的服务不需要在`plugin.xml`中注册(见[How To Declare a Service](#how-to-declare-a-service))。

Instead, annotate service class with [@Service](upsource:///platform/core-api/src/com/intellij/openapi/components/Service.java). If service is written in Java and not Kotlin, mark class as `final`.
 
**CN:**  相反，使用[@Service](upsource:///platform/core-api/src/com/intellij/openapi/components/Service.java)注释服务类。如果服务是用Java而不是Kotlin编写的，则将类标记为`final`。

Restrictions:

**CN:** 限制: 

* Constructor injection is not supported (since it is deprecated), but project level service can define a constructor that accepts `Project`, and module level service `Module` respectively.

**CN:**  不支持构造函数注入(因为它已被弃用)，但是项目级服务可以定义一个分别接受`Project`和模块级服务`Module`的构造函数。

* If service is a [PersistentStateComponent](/basics/persisting_state_of_components.md), roaming must be disabled (`roamingType` is set to `RoamingType.DISABLED`).

**CN:**  如果服务是[PersistentStateComponent](/basics/persisting_state_of_components.md)，则必须禁用漫游(`roamingType`设置为`RoamingType.DISABLED`)。

* Service class must be `final`.

**CN:**  服务类必须是`final`。

## How to Declare a Service?——如何声明服务?

To declare a service, you can use the following extension points in the IntelliJ Platform:

**CN:**  要声明一个服务，你可以在IntelliJ平台上使用以下扩展点:

* `com.intellij.applicationService`: designed to declare an application level service.

**CN:**  `com.intellij.applicationService`:用于声明应用程序级服务。

* `com.intellij.projectService`: designed to declare a project level service.

**CN:**  `com.intellij.projectService`:用于声明项目级服务。

* `com.intellij.moduleService`: designed to declare a module level service.

**CN:**  `com.intellij.moduleService`: 用于声明模块级服务。

**To declare a service:**

**CN:**  **声明服务:**

1. In your project, open the context menu of the destination package and click *New* (or press <kbd>Alt</kbd>+<kbd>Insert</kbd>).
**CN:**  在你的项目中，打开目标包的上下文菜单，点击*New*(或者按<kbd>Alt</kbd>+<kbd>Insert</kbd>)。

2. In the *New* menu, choose *Plugin DevKit* and click *Application Service*, *Project Service* or *Module Service* depending on the type of service you need to use.
**CN:**  在*New*菜单中，选择*Plugin DevKit*并点击*Application Service*、*Project Service*或*Module Service*，这取决于您需要使用的服务类型。

3. In the dialog box that opens, you can specify service interface and implementation, or just a service class if you uncheck *Separate interface from implementation* checkbox.
**CN:**  在打开的对话框中，您可以指定服务接口和实现，或者如果不选中*Separate interface from implementation*复选框，则仅指定一个服务类。

The IDE will generate new Java interface and class (or just a class if you unchecked *Separate interface from implementation* checkbox) and register the new service in `plugin.xml` file.

**CN:**  IDE将生成新的Java接口和类(如果您未选中*Separate interface from implementation*复选框，则仅生成一个类)，并将新服务注册到`plugin.xml`文件中。

> **Note** Declaring a service via *New* context menu is available since version **2017.3**.
>
>**CN:**  注意，通过*New*上下文菜单声明服务从版本 **2017.3** 开始可用。


To clarify the service declaration procedure, consider the following fragment of the `plugin.xml` file:

**CN:**  要阐明服务声明过程，请考虑以下`plugin.xml`文件片段:

```xml
<extensions defaultExtensionNs="com.intellij">
  <!-- Declare the application level service -->
  <applicationService serviceInterface="mypackage.MyApplicationService" 
                      serviceImplementation="mypackage.MyApplicationServiceImpl" />

  <!-- Declare the project level service -->
  <projectService serviceInterface="mypackage.MyProjectService" 
                  serviceImplementation="mypackage.MyProjectServiceImpl" />
</extensions>
```

If `serviceInterface` isn't specified, it's supposed to have the same value as `serviceImplementation`.

**CN:**  如果没有指定`serviceInterface`，它应该与`serviceImplementation`值相同。

## Retrieving a service——检索服务

Getting service doesn't need read action and can be performed from any thread. If service is requested from several threads, it will be initialized in the first thread, and other threads will be blocked until service is fully initialized. 

**CN:**  获取服务不需要读操作，可以在任何线程中执行。如果服务被多个线程请求，它将在第一个线程中初始化，其他线程将被阻塞，直到服务完全初始化。

To instantiate your service in Java code:

**CN:**  用Java代码实例化你的服务:

```java
MyApplicationService applicationService = ServiceManager.getService(MyApplicationService.class);

MyProjectService projectService = project.getService(MyProjectService.class)
```

In Kotlin code, you can use convenience methods:

**CN:**  在Kotlin代码中，您可以使用方便的方法:

```kotlin
MyApplicationService applicationService = service<MyApplicationService>()

MyProjectService projectService = project.service<MyProjectService>()
```

### Sample Plugin——插件示例

This section allows you to download and install a sample plugin illustrating how to create and use a plugin service. This plugin has a project component implementing a service that counts the number of currently opened projects in the IDE. If this number exceeds the maximum allowed number of simultaneously opened projects, the plugin returns an error message and closes the most recently opened project.

**CN:**  本节允许您下载并安装一个演示如何创建和使用插件服务的示例插件。这个插件有一个项目组件，它实现了一个计算IDE中当前打开的项目数量的服务。如果这个数字超过了同时打开的项目的最大允许数量，插件将返回一个错误消息并关闭最近打开的项目。

<!-- TODO Replace with other plugin URL when available-->
<!-- TODO 替换为其他可用的插件URL -->

**To install and run the sample plugin**

**CN:**  **安装并运行示例插件**

* Download the included sample plugin project located [here](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/max_opened_projects).

**CN:**  下载所包含的位于[here](https://github.com/JetBrains/intellij-sdk-docs/tree/master/code_samples/max_opened_projects)的示例插件项目。

* Start *IntelliJ IDEA*, on the starting page, click *Open Project*, and then use the *Open Project* dialog box to open the project *max_opened_projects*.

**CN:**  启动*IntelliJ IDEA*，在起始页面，点击*Open Project*，然后使用*Open Project*对话框打开项目*max_opened_projects*。

* On the main menu, choose *Run \| Run* or press <kbd>Shift</kbd>+<kbd>F10</kbd>.

**CN:**  在主菜单中，选择*Run \| Run*或按下<kbd>Shift</kbd>+<kbd>F10</kbd>。

* If necessary, change the [Run/Debug Configurations](https://www.jetbrains.com/help/idea/run-debug-configuration-plugin.html).

**CN:**  如有必要，更换[Run/Debug Configurations](https://www.jetbrains.com/help/idea/run-debug-configuration-plugin.html)。
