---
title: Publishing Plugins with Gradle——使用Gradle发布插件
---

Once you have configured Gradle support, you can automatically build and deploy your plugin to the [JetBrains Plugin Repository](https://plugins.jetbrains.com). 
To automatically deploy a plugin, you need to have _already published the plugin to the plugin repository at least once._ 
Please see the guide page for manually [publishing a plugin](../../basics/getting_started/publishing_plugin.md) for the first time.

**CN:**  一旦你配置了Gradle支持，你就可以自动构建和部署你的插件到[JetBrains Plugin Repository](https://plugins.jetbrains.com)。
         要自动部署插件，您需要有 _already published the plugin to the plugin repository at least once._(已经至少一次将插件发布到插件库中) 
         第一次手动[publishing a plugin](../../basics/getting_started/publishing_plugin.md)请查看指导页面。


> **WARNING** When adding additional repositories to your Gradle build script, make sure to always use HTTPS protocol.
>
>**CN:**  警告：当向Gradle构建脚本添加额外的存储库时，确保始终使用HTTPS协议。


* bullet list
{:toc}

## Providing Your Hub Permanent Token to Gradle——提供您的Hub永久令牌给Gradle
To deploy a plugin to the plugin repository, you need to supply your [JetBrains Hub Permanent Token](https://plugins.jetbrains.com/docs/marketplace/plugin-upload.html). 
This page describes three options to supply your _Hub Permanent Token_ via Gradle using: 

**CN:**  要将插件部署到插件存储库，您需要提供您的[JetBrains Hub Permanent Token](https://plugins.jetbrains.com/docs/marketplace/plugin-upload.html)。
         本页描述了三个选项，以供应您的 _Hub Permanent Token_ 通过级配使用:

* Gradle properties, 
* Environment variables,
* Parameters to the Gradle task.

**CN:**  
* Gradle属性,
* 环境变量,
* Gradle任务的参数。

### Using Gradle Properties——使用Gradle属性
You can store the Hub Token in [Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties). 

**CN:**  您可以将集线器令牌存储在[Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties)中。


> **WARNING** You must not check the Gradle properties file containing the Hub Token into source control.
>
>**CN:**  警告：不能将包含集线器令牌的Gradle属性文件签入源代码控制。


If you place a `gradle.properties` file containing your Hub Permanent Token in your project's root directory, please ensure your version control tool ignores this file. 
For example in Git, you can add the following line to your `.gitignore` file:

**CN:**  如果您将包含集线器永久令牌的`gradle.properties`文件放在项目的根目录中，请确保您的版本控制工具忽略该文件。
         例如在Git中，你可以在你的`.gitignore`文件中添加如下代码:

```
gradle.properties
```

To add the Hub Token to a Gradle properties file, place the following information in:

**CN:**  要将Hub令牌添加到Gradle属性文件中，请将以下信息放入:

* The file `GRADLE_HOME/gradle.properties`,

**CN:**  `GRADLE_HOME/gradle.properties`文件。

* Or inside a file called `gradle.properties` under your project's root directory.

**CN:**  或者在项目根目录下名为`gradle.properties`的文件中。


```text
intellijPublishToken=YOUR_HUB_TOKEN_HERE
```

Then refer to these values in `publishPlugin` task in your `build.gradle` file:

**CN:**  然后参考你的`build.gradle`文件中的`publishPlugin`任务中的这些值:

```groovy
publishPlugin {
    token intellijPublishToken
}
```

### Using Environment Variables——使用环境变量
Alternatively, and possibly slightly safer because you cannot accidentally commit your token to version control, you can provide your token via an environment variable. 
For example, start by defining an environment variable such as:

**CN:**  或者，也可能稍微安全一点，因为您不能意外地将令牌提交到版本控制，您可以通过环境变量提供令牌。
         例如，首先定义一个环境变量，如:

```bash
export ORG_GRADLE_PROJECT_intellijPublishToken='YOUR_HUB_TOKEN_HERE'
```

> **Note** On macOS systems environment variables defined in `.bash_profile` are only visible to processes you run from bash. 
Environment variables visible to all processes need to be defined in [Environment.plist](https://developer.apple.com/library/archive/qa/qa1067/_index.html)
>
>**CN:**  注意：在macOS系统上，在`.bash_profile`中定义的环境变量只对从bash运行的进程可见。
             所有过程可见的环境变量需要在[Environment.plist](https://developer.apple.com/library/archive/qa/qa1067/_index.html)中定义


Now provide the environment variable in the run configuration with which you run the `publishPlugin` task locally. 
To do so, create a Gradle run configuration (if not already done), choose your Gradle project, specify the `publishPlugin` task, and then add the environment variable. 

**CN:**  现在在运行配置中提供环境变量，您可以使用该环境变量在本地运行`publishPlugin`任务。
         为此，创建一个Gradle运行配置(如果还没有完成)，选择你的Gradle项目，指定`publishPlugin`任务，然后添加环境变量。

```groovy
publishPlugin {
  token = System.getenv("ORG_GRADLE_PROJECT_intellijPublishToken")
}
```

Note that you still need to put some default values (can be empty) in the Gradle properties because otherwise, you will get a compilation error.

**CN:**  请注意，您仍然需要在Gradle属性中放入一些默认值(可以是空的)，否则，您将得到一个编译错误。


### Using Parameters for the Gradle Task——使用参数的Gradle任务
Similar to using environment variables, you can also pass your token as a parameter to the Gradle task.
For example, you can to provide the parameter `-Dorg.gradle.project.intellijPublishToken=YOUR_HUB_TOKEN_HERE` on the command line or by putting it in the arguments of your run configuration.

**CN:**  与使用环境变量类似，您还可以将令牌作为参数传递给Gradle任务。
         例如，您可以在命令行上提供参数`-Dorg.gradle.project.intellijPublishToken=YOUR_HUB_TOKEN_HERE`，或者将其放入运行配置的参数中。


Note that also, in this case, you still need to put some default values in your Gradle properties.

**CN:**  注意，在这种情况下，你仍然需要在你的Gradle属性中放入一些默认值。



## Deploying a Plugin with Gradle——使用Gradle部署插件
The first step when deploying a plugin is to confirm that it works correctly. 
You may wish to verify this by [installing your plugin from disk](https://www.jetbrains.com/help/idea/managing-plugins.html) on a fresh instance of your target IDE(s). 

**CN:**  部署插件的第一步是确认它工作正常。
         您可能希望通过[installing your plugin from disk](https://www.jetbrains.com/help/idea/managing-plugins.html)在目标IDE的新实例上验证这一点。


### Publishing a Plugin——发布插件
Once you are confident the plugin works as intended, make sure the plugin version is updated, as the JetBrains Plugin repository won't accept multiple artifacts with the same version. 
To deploy a new version of your plugin to the JetBrains plugin repository, execute the following Gradle command:  

**CN:**  一旦您确信插件能够正常工作，请确保更新了插件版本，因为JetBrains插件存储库不会接受具有相同版本的多个工件。
         要将新版本的插件部署到JetBrains插件库，请执行以下Gradle命令:

```bash
gradle publishPlugin
```

Now check the most recent version of your plugin appears on the [Plugin Repository](https://plugins.jetbrains.com/). 
If successfully deployed, any users who currently have your plugin installed on an eligible version of the IntelliJ Platform are notified of a new update available on the following restart.

**CN:**  现在检查你的插件的最新版本出现在[Plugin Repository](https://plugins.jetbrains.com/)上。
         如果成功部署，任何在IntelliJ平台的合格版本上安装了您的插件的用户都会在重启后收到更新通知。


### Specifying a Release Channel——指定发布通道
You may also deploy plugins to a release channel of your choosing, by configuring the `publishPlugin.channels` property. 
For example:

**CN:**  您还可以通过配置`publishPlugin.channels`属性，将插件部署到您选择的发布通道。
         例如:

```groovy
publishPlugin {
    channels 'beta'
}
```

When empty, this uses the default plugin repository, available to all [JetBrains plugin repository](https://plugins.jetbrains.com/) users. 
However, you can publish to an arbitrarily-named channel. 
These non-default release channels are treated as separate repositories. 
When using a non-default release channel, users need to add a new [custom plugin repository](https://www.jetbrains.com/help/idea/managing-plugins.html#repos) to install your plugin. 
For example, if you specify `publishPlugin.channels 'canary'`, then users need to add the `https://plugins.jetbrains.com/plugins/canary/list` repository to install the plugin and receive updates. 
Popular channel names include:

**CN:**  当为空时，它使用默认的插件库，所有[JetBrains plugin repository](https://plugins.jetbrains.com/)用户都可以使用。
         但是，您可以发布到任意命名的通道。
         这些非默认的发布通道被视为独立的存储库。
         当使用非默认的发布渠道时，用户需要添加一个新的[custom plugin repository](https://www.jetbrains.com/help/idea/managing-plugins.html#repos)来安装你的插件。
         例如，如果您指定`publishPlugin.channels 'canary'`，那么用户需要添加`https://plugins.jetbrains.com/plugins/canary/list`存储库来安装插件并接收更新。
         流行的频道名称包括:

* `alpha`: https://plugins.jetbrains.com/plugins/alpha/list
* `beta`: https://plugins.jetbrains.com/plugins/beta/list
* `eap`: https://plugins.jetbrains.com/plugins/eap/list

More information about the available configuration options is in the [documentation of the intellij gradle plugin](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md#publishing-dsl).

**CN:**  有关可用配置选项的更多信息在[documentation of the intellij gradle plugin](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md#publishing-dsl)中。

