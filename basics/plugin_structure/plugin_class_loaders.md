---
title: Plugin Class Loaders——插件的类装载器
---

A separate class loader is used to load the classes of each plugin. This allows each plugin to use a different version of a library, even if the same library is used by the IDE itself or by another plugin.

**CN:**  单独的类装载器用于装载每个插件的类。这允许每个插件使用不同版本的库，即使IDE本身或其他插件使用相同的库。

By default, the main IDE class loader loads classes that were not found in the plugin class loader. However, in the `plugin.xml` file, you may use the `<depends>` element to specify that a [plugin depends](plugin_class_loaders.md) on one or more other plugins. In this case the class loaders of those plugins will be used for classes not found in the current plugin. This allows a plugin to reference classes from other plugins.

**CN:**  默认情况下，主IDE类装载器装入在插件类装载器中没有找到的类。但是，在`plugin.xml`文件中，您可以使用`<depends>`元素来指定一个[plugin depends](plugin_class_loaders.md)于一个或多个其他插件。