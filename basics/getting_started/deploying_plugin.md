---
title: Deploying a Plugin——部署插件
---

Before your custom plugin can be used, it must be deployed: built, installed, and then enabled using Plugin Manager.

**CN:**  在使用自定义插件之前，必须对其进行部署:构建、安装，然后使用插件管理器启用它。

To deploy a plugin:

**CN:**  部署插件:

* Make your project by invoking **Build \| Build Project** or **Build \| Build Module \<module name\>**.

**CN:**  通过调用 **Build \| Build Project** 或 **Build \| Build Module \<module name\>** 生成您的项目

* Prepare your plugin for deployment. In the main menu, select **Build \| Prepare Plugin Module \<module name\> for Deployment**.

**CN:**  为部署插件做好准备。在主菜单中，选择**Build \| Prepare Plugin Module \<module name\> for Deployment**

  ![Prepare Plugin for Deployment](deploying_plugin/img/prepare_plugin_for_deployment.png)

* If the plugin module does not depend on any libraries, a `.jar` archive will be created. Otherwise, a `.zip` archive will be created including all the plugin libraries specified in the project settings.

**CN:**  如果插件模块不依赖于任何库，将创建一个`.jar`归档。否则，将创建一个`.zip`档案，包括项目设置中指定的所有插件库。

  ![Jar Saved Notification](deploying_plugin/img/jar_saved_notification.png)

* [Install](https://www.jetbrains.com/help/idea/managing-plugins.html#installing-plugins-from-disk)
  the newly created archive/jar file from disk. The `editor_basics` code sample builds the plugin archive/jar into the `editor_basics` project folder:

**CN:**  [Install](https://www.jetbrains.com/help/idea/managing-plugins.html#installing-plugins-from-disk)从磁盘上新建的archive/jar文件。`editor_basics`代码示例将插件archive/jar构建到`editor_basics`项目文件夹中:

  ![Jar File Location](deploying_plugin/img/jar_location.png)

* Restart your IDE so the changes will take effect.

**CN:**  重新启动IDE，这样更改才会生效。

