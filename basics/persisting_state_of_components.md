---
title: Persisting State of Components——持久化组件状态
---

The *IntelliJ Platform* provides an API that allows components or services to persist their state between restarts of the IDE. You can use either a simple API to persist a few values, or persist the state of more complicated components using the [`PersistentStateComponent`](upsource:///platform/core-api/src/com/intellij/openapi/components/PersistentStateComponent.java) interface.

**CN:**  *IntelliJ Platform*提供了一个API，它允许组件或服务在IDE重启之间保持它们的状态。您可以使用简单的API来持久化一些值，也可以使用[`PersistentStateComponent`](upsource:///platform/core-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口持久化更复杂组件的状态。

> **WARNING** If you need to persist sensitive data like passwords, please see [Persisting Sensitive Data](persisting_sensitive_data.md).
>
>**CN:**  警告：如果您需要保存密码等敏感数据，请参阅[Persisting Sensitive Data](persisting_sensitive_data.md)。

## Using PropertiesComponent for simple non-roamable persistence——使用PropertiesComponent实现简单的不可访问持久性

If the only thing that your plugin needs to persist is a few simple values, the easiest way to do so is to use the [`com.intellij.ide.util.PropertiesComponent`](upsource:///platform/core-api/src/com/intellij/ide/util/PropertiesComponent.java) service. It can be used for saving both application-level values and project-level values (stored in the workspace file). Roaming is disabled for `PropertiesComponent`, so use it only for temporary, non-roamable properties.

**CN:**  如果您的插件只需要持久化一些简单的值，那么最简单的方法就是使用[`com.intellij.ide.util.PropertiesComponent`](upsource:///platform/core-api/src/com/intellij/ide/util/PropertiesComponent.java)服务。它可以用于保存应用程序级的值和项目级的值(存储在工作空间文件中)。`PropertiesComponent`禁止漫游，因此仅对临时的、不可漫游的属性使用漫游。

Use the `PropertiesComponent.getInstance()` method for storing application-level values, and the `PropertiesComponent.getInstance(Project)` method for storing project-level values.

**CN:**  使用`PropertiesComponent.getInstance()`方法存储应用级值，使用`PropertiesComponent.getInstance(Project)`方法存储项目级值。

Since all plugins share the same namespace, it is highly recommended to prefix key names (e.g. using your plugin ID).

**CN:**  因为所有的插件都有相同的命名空间，所以强烈建议在键名前面加上前缀(例如使用你的插件ID)。

## Using PersistentStateComponent——使用PersistentStateComponent

The [`com.intellij.openapi.components.PersistentStateComponent`](upsource:///platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java) interface gives you the most flexibility for defining the values to be persisted, their format and storage location.

**CN:**  [`com.intellij.openapi.components.PersistentStateComponent`](upsource:///platform/projectModel-api/src/com/intellij/openapi/components/PersistentStateComponent.java)接口为您提供了定义要持久化的值、它们的格式和存储位置的最大灵活性。

To use it:

**CN:**  使用它:

- mark a [service](plugin_structure/plugin_services.md) or a [component](plugin_structure/plugin_components.md) as implementing the `PersistentStateComponent` interface

**CN:**  将[service](plugin_structure/plugin_services.md)或[component](plugin_structure/plugin_components.md)标记为实现`PersistentStateComponent`接口

- define the state class

**CN:**  定义状态类

- specify the storage location using `@com.intellij.openapi.components.State`

**CN:**  使用`@com.intellij.openapi.components.State`指定存储位置

Note that instances of extensions cannot persist their state by implementing `PersistentStateComponent`. If your extension needs to have a persistent state, you need to define a separate service responsible for managing that state.

**CN:**  注意，扩展的实例不能通过实现`PersistentStateComponent`来保持它们的状态。如果您的扩展需要具有持久状态，则需要定义一个负责管理该状态的独立服务。

### Implementing the PersistentStateComponent interface——实现PersistentStateComponent接口

The implementation of `PersistentStateComponent` needs to be parameterized with the type of the state class. The state class can either be a separate JavaBean class, or the class implementing `PersistentStateComponent` itself.

**CN:**  `PersistentStateComponent`的实现需要使用state类的类型进行参数化。状态类可以是一个单独的JavaBean类，也可以是实现`PersistentStateComponent`本身的类。

In the former case, the instance of the state class is typically stored as a field in the `PersistentStateComponent` class:

**CN:**  在前一种情况下，state类的实例通常存储为`PersistentStateComponent`类中的一个字段:

```java
@State(...)
class MyService implements PersistentStateComponent<MyService.State> {
  
  static class State {
    public String value;
  }

  State myState;

  public State getState() {
    return myState;
  }

  public void loadState(State state) {
    myState = state;
  }
}
```

In the latter case, you can use the following pattern to implement `getState()` and `loadState()` methods:

**CN:**  在后一种情况下，可以使用以下模式来实现`getState()`和`loadState()`方法:

```java
@State(...)
class MyService implements PersistentStateComponent<MyService> {
  public String stateValue;

  public MyService getState() {
    return this;
  }

  public void loadState(MyService state) {
    XmlSerializerUtil.copyBean(state, this);
  }
}
```

### Implementing the state class——实现state类

The implementation of `PersistentStateComponent` works by serializing public fields, [annotated](upsource:///platform/util/src/com/intellij/util/xmlb/annotations) private fields and bean properties into an XML format. 

**CN:**  

The following types of values can be persisted:

**CN:**  

* numbers (both primitive types, such as `int`, and boxed types, such as `Integer`)

**CN:**  

* booleans
* strings
* collections
* maps
* enums

To exclude a public field or bean property from serialization, annotate the field or getter with `@com.intellij.util.xmlb.annotations.Transient`.

**CN:**  

Note that the state class must have a default constructor. It should return the default state of the component (one used if there is nothing persisted in the XML files yet).

**CN:**  

State class should have an `equals` method, but if it is not implemented, state objects will be compared by fields. When using Kotlin, use [Data Classes](https://kotlinlang.org/docs/reference/data-classes.html).

**CN:**  

### Defining the storage location——定义存储位置

To specify where exactly the persisted values will be stored, you need to add a `@State` annotation to the `PersistentStateComponent` class. 

**CN:**  

It has the following fields:

**CN:**  

* `name` (required) — specifies the name of the state (name of the root tag in XML).

**CN:**  

* `storages` — one or more of `@com.intellij.openapi.components.Storage` annotations to specify the storage locations. Optional for project-level values — standard project file will be used in this case.

**CN:**  

* `reloadable` (optional) — if set to false, complete project (or application) reload is required when the XML file is changed externally and the state has changed.

**CN:**  

The simplest ways of specifying the `@Storage` annotation are as follows (since 2016.x; for previous versions, please see [old version](https://github.com/JetBrains/intellij-sdk-docs/blob/5dcb02991cf828a7d4680d125ce56b4c10234146/basics/persisting_state_of_components.md) of this document):

**CN:**  

* `@Storage("yourName.xml")` If component is project-level — for `.ipr` based projects standard project file will be used automatically; you don't need to specify anything.

**CN:**  

* `@Storage(StoragePathMacros.WORKSPACE_FILE)` for values stored in the workspace file.

**CN:**  

By specifying a different value for the `value` parameter (`file` before 2016.x), the state will be persisted in a different file. 

**CN:**  

> **NOTE** For application-level components it is strongly recommended to use a custom file, using of `other.xml` is deprecated.
>
>**CN:**  

The `roamingType` parameter of the `@Storage` annotation specifies the roaming type when the Settings Repository plugin is used.

**CN:**  

## Customizing the XML format of persisted values——自定义持久化值的XML格式

Please consider using annotation parameters only to achieve backward compatibility. Otherwise feel free to file issues about serialization cosmetics.

**CN:**  

If you want to use the default bean serialization but need to customize the storage format in XML (for example, for compatibility with previous versions of your plugin or externally defined XML formats), you can use the `@Tag`, `@Attribute`, `@Property`, `@MapAnnotation`, `@AbstractCollection` annotations. 

**CN:**  

Please see `com.intellij.util.xmlb.annotations`'s `package.html` for more information.

**CN:**  

If the state that you need to serialize doesn't map cleanly to a JavaBean, you can use `org.jdom.Element` as the state class. In that case, you can use the `getState()` method to build an XML element with an arbitrary structure, which will then be saved directly in the state XML file. In the `loadState()` method, you can deserialize the JDOM element tree using any custom logic. Please note this is not recommended and should be avoided whenever possible.

**CN:**  

## Persistent component lifecycle——持久性组件生命周期

The `loadState()` method is called after the component has been created (only if there is some non-default state persisted for the component), and after the XML file with the persisted state is changed externally (for example, if the project file was updated from the version control system). In the latter case, the component is responsible for updating the UI and other related components according to the changed state.

**CN:**  

The `getState()` method is called every time the settings are saved (for example, on frame deactivation or when closing the IDE). If the state returned from `getState()` is equal to the default state (obtained by creating the state class with a default constructor), nothing is persisted in the XML. Otherwise, the returned state is serialized in XML and stored.

**CN:**  

## Legacy API (JDOMExternalizable)——遗留API (JDOMExternalizable)

Older components use the [`JDOMExternalizable`](upsource:///platform/util/src/com/intellij/openapi/util/JDOMExternalizable.java) interface for persisting state. It uses the `readExternal()` method for reading the state from a JDOM element, and `writeExternal()` to write the state to it.

**CN:**  

Implementations can store the state in attributes and sub-elements manually, or use the [`DefaultJDOMExternalizer`](upsource:///platform/util/src/com/intellij/openapi/util/DefaultJDOMExternalizer.java) class to store the values of all public fields automatically.

**CN:**  

Components save their state in the following files:

**CN:**  

* Project-level: project (`.ipr`) file. However, if the workspace option in the `plugin.xml` file is set to `true`, then workspace (`.iws`) file will be used instead.

**CN:**  

* Module-level: module (`.iml`) file.

**CN:**  
