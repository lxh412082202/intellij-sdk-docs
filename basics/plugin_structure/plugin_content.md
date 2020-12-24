---
title: Plugin Content——插件内容
---

The plugin `jar` file must contain:

**CN:**  插件`jar`文件必须包含:

- the configuration file (`META-INF/plugin.xml`) ([Plugin Configuration File](plugin_configuration_file.md))

**CN:**  配置文件(`META-INF/plugin.xml`) ([Plugin Configuration File](plugin_configuration_file.md))

- the classes that implement the plugin functionality 

**CN:**  实现插件功能的类

- recommended: plugin logo file(s) (`META-INF/pluginIcon*.svg`) ([Plugin Logo](plugin_icon_file.md)) 

**CN:**  推荐:插件logo文件(`META-INF/pluginIcon*.svg`) ([Plugin Logo](plugin_icon_file.md))


### Plugin Without Dependencies——无依赖插件
A plugin consisting of a single `.jar` file is placed in the `/plugins` directory.

**CN:**  一个由单个`.jar`文件组成的插件被放置在`/plugins`目录中。

```
.IntelliJIDEAx0/
└── plugins
    └── sample.jar
        ├── com/foo/...
        │   ...
        │   ...
        └── META-INF
            ├── plugin.xml
            ├── pluginIcon.svg
            └── pluginIcon_dark.svg
```


### Plugin With Dependencies——有依赖插件
The plugin `.jar` file is placed in the `/lib` folder under the plugin's "root" folder, together with all required bundled libraries.

**CN:**  插件的`.jar`文件放在插件"root"文件夹下的`/lib`文件夹中，以及所有需要的绑定库。

All jars from the `/lib` folder are automatically added to the classpath (see also [Plugin Class Loaders](plugin_class_loaders.md)).

**CN:**  所有来自`/lib`文件夹的jar都会自动添加到类路径中(参见[Plugin Class Loaders](plugin_class_loaders.md))。

```
   .IntelliJIDEAx0/
   └── plugins
       └── sample
           └── lib
               ├── lib_foo.jar
               ├── lib_bar.jar
               │   ...
               │   ...
               └── sample.jar
                   ├── com/foo/...
                   │   ...
                   │   ...
                   └── META-INF
                       ├── plugin.xml
                       ├── pluginIcon.svg
                       └── pluginIcon_dark.svg
```
