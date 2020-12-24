[![official JetBrains project](https://jb.gg/badges/official-flat-square.svg)](https://confluence.jetbrains.com/display/ALL/JetBrains+on+GitHub)

IntelliJ平台SDK文档
=======

欢迎使用[IntelliJ Platform SDK Documentation](http://www.jetbrains.org/intellij/sdk/docs/)站点的存储库。

## 报告Bug
请通过向[YouTrack](https://youtrack.jetbrains.com/issues/IJSDK)提交问题报告文件和样品中发现的任何内容不一致、过期材料、外观问题和其他缺陷。 

## 在本地使用站点
要签出并运行站点的本地副本，请执行以下步骤。

### 准备

*  确保安装了 
   [git客户端](https://git-scm.com/downloads)

*  此站点需要
   [Ruby 2.0](https://www.ruby-lang.org/)或更高版本。
   按照官方的Ruby语言
   [下载](https://www.ruby-lang.org/en/downloads/)
   和
   [安装](https://www.ruby-lang.org/en/documentation/installation/)
   说明，让Ruby在您的机器上工作
   
*  此站点需要[Jekyll](https://jekyllrb.com/), 
   一个基于Ruby的站点生成框架。
   要安装Jekyll请参阅其
   [安装指南](https://jekyllrb.com/docs/installation/).
   **Note:** 如果你使用的是Windows系统, 在安装Jekyll时需要一些特别的操作.
   有关详细信息，请参阅本[在Windows上运行Jekyll指南](https://jekyll-windows.juthilo.com/)。
   
### 从git仓库检出站点

运行下面命令以检出源代码:

```bash
git clone https://github.com/JetBrains/intellij-sdk-docs.git
```
   
### 初始化子模块

该网站使用JetBrains自定义web模板。
要在本地启用自定义模板，需要初始化存储库子模块。
在检出目录中运行以下命令。
```bash
git submodule update --init --remote
```

### 安装Gems

在对主存储库和子模块执行初始签出之后，运行`bundle install`以安装额外需要的gem。

### 生成和预览
一组Rake任务，一个用Ruby实现的类似Make的程序，提供了在本地构建和运行站点的简短命令。

#### Building Site from Sources
 
*  确保您位于项目根目录中
*  生成静态网站内容运行
   ```
   rake build
   ```
   
#### 预览

*  启动运行web-server
    ```
    rake preview
    ```
*  在浏览器中打开
   [http://localhost:4000/intellij/sdk/docs/](http://localhost:4000/intellij/sdk/docs/)。
   **Note:** 确保在安装过程中没有更改默认的Jekyll端口。


