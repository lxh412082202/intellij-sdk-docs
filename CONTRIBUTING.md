---
title: Contributing to the IntelliJ Platform SDK——为IntelliJ平台SDK做贡献
---

This document describes our contribution guidelines for the open source IntelliJ Platform SDK documentation and sample code.
Before you begin contributing content to the SDK, please read this page thoroughly as well as the [Code of Conduct](/CODE_OF_CONDUCT.md) and [License](https://github.com/JetBrains/intellij-sdk-docs/blob/master/LICENSE.txt) documents.
For information about contributing to the IntelliJ Platform itself, please visit [Contributing to the IntelliJ Platform](/basics/platform_contributions.md).

**CN:**  本文档描述了我们为开放源码IntelliJ平台SDK文档和示例代码提供的贡献指南。
         在您开始向SDK贡献内容之前，请仔细阅读此页以及[Code of Conduct](/CODE_OF_CONDUCT.md)和[License](https://github.com/JetBrains/intellij-sdk-docs/blob/master/LICENSE.txt)文件。
         有关IntelliJ平台自身贡献的信息，请访问[Contributing to the IntelliJ Platform](/basics/platform_contributions.md)。


Here are some useful things to know before authoring SDK content and submitting your Pull Request.

**CN:**  以下是在编写SDK内容和提交拉请求之前需要了解的一些有用信息。

* Dummy list item
{:toc}
 
## Setting Up the Documentation Build Environment——设置文档构建环境

This site runs via [Jekyll](https://jekyllrb.com), which is a popular static site generator, written in Ruby. It can be hosted locally to ensure that any changes are correct. Once set up, running the site is as easy as calling `rake preview`.

**CN:**  这个站点通过[Jekyll](https://jekyllrb.com)运行，它是一个用Ruby编写的流行的静态站点生成器。它可以在本地托管，以确保任何更改都是正确的。一旦设置好，运行站点就像调用`rake preview`一样简单。

Alternatively, the site can also be hosted in a [Docker container](https://www.docker.com). On Mac and Windows, this means the site is hosted in a virtual machine. Docker maintains this container, building it based on the instructions in the [`Dockerfile`](Dockerfile). All dependencies (Ruby, etc.) are automatically installed when building the image, which reduces the manual configuration steps. The Docker image is also used to build the [published site](https://www.jetbrains.org/intellij/sdk/docs/index.html), so it is a known working environment.

**CN:**  或者，站点也可以托管在[Docker container](https://www.docker.com)中。在Mac和Windows上，这意味着站点托管在虚拟机中。Docker维护此容器，并根据[`Dockerfile`](Dockerfile)中的说明进行构建。所有依赖项(Ruby等)都是在构建映像时自动安装的，这减少了手动配置步骤。Docker映像还用于构建[published site](https://www.jetbrains.org/intellij/sdk/docs/index.html)，因此它是一个已知的工作环境。

### Developing Documentation with Docker——使用Docker开发文档

Follow these steps to work with Docker:

**CN:**  按照以下步骤与Docker一起工作:

* Firstly, install Docker, using the [Docker Toolbox](https://www.docker.com/docker-toolbox).

**CN:**  首先，使用[Docker Toolbox](https://www.docker.com/docker-toolbox)安装Docker。

* On Windows and Mac, start the Docker host virtual machine (start the "Docker Quickstart terminal" or run "Kitematic." See the Getting Started guides on [docker.com](https://www.docker.com) for more details).

**CN:**  在Windows和Mac上，启动Docker主机虚拟机(启动"Docker Quickstart terminal"或运行"Kitematic."查看[docker.com](https://www.docker.com)上的入门指南了解更多详细信息)。

* Clone the [`intellij-sdk-docs`](https://github.com/JetBrains/intellij-sdk-docs) repo to the local machine, and [initialise the `sdkdocs-template` submodule](#documentation-repository-submodules).

**CN:**  将[`intellij-sdk-docs`](https://github.com/JetBrains/intellij-sdk-docs) repo克隆到本地机器和[initialise the `sdkdocs-template` submodule](#documentation-repository-submodules)。

* Change the current directory to the parent directory of the git repo.

**CN:**  将当前目录更改为git repo的父目录。

* Run `docker build -t intellij-sdk-docs .` to build the docker image from the current folder, and give it the tag `intellij-sdk-docs`.

* **CN:**  运行`docker build -t intellij-sdk-docs .`从当前文件夹构建docker图像，并将其命名为`intellij-sdk-docs`。

    * Note that this must be run from a command prompt that has the various `DOCKER_*` environment variables set up, such as the Docker Quickstart Terminal.
    
    * **CN:**  请注意，这必须在命令提示符中运行，该命令提示符设置了各种`DOCKER_*`环境变量，例如Docker Quickstart终端。

* Run `docker run -p 4000:4000 -v $PWD:/usr/src/app intellij-sdk-docs`. This command will:

* **CN:**  运行`docker run -p 4000:4000 -v $PWD:/usr/src/app intellij-sdk-docs`。这个命令将：

    * Start the docker container called `intellij-sdk-docs`.
    
    * **CN:**  启动名为`intellij-sdk-docs`的docker容器
    
    * Forward port 4000 from the docker container to port 4000 on the docker client.
    
    * **CN:**  将端口4000从docker容器转发到docker客户机上的端口4000。
    
    > **NOTE** For Windows and Mac, this means port 4000 of the docker container is forwarded to port 4000 of the docker virtual machine, not `localhost`. For Linux, the docker client is the host machine, so `localhost:4000` is forwarded to port 4000 on the docker container.
    >
    > **CN:**  对于Windows和Mac，这意味着docker容器的4000端口被转发到docker虚拟机的4000端口，而不是`localhost`。对于Linux来说，docker客户端是主机，所以`localhost:4000`被转发到docker容器上的端口4000。
    >
    > To hit the container's port 4000 from Windows or the Mac, it is necessary to hit the IP address of the docker client (virtual machine). Use `docker-machine ip default` to get the IP address of the docker client. Use `X.X.X.X:4000` to hit the client in the virtual machine, which is in turn mapped to the container's port 4000.
    >
    > **CN:**  要从Windows或Mac中命中容器的端口4000，需要命中docker客户机(虚拟机)的IP地址。使用`docker-machine ip default`获取docker客户端的IP地址。使用`X.X.X.X:4000`在虚拟机中命中客户端，然后客户端映射到容器的端口4000。
    >
    > Alternatively, modify the virtual machine's settings to automatically forward port 4000 to `localhost`. See this [blog post](https://acaird.github.io/computers/2014/11/16/docker-virtualbox-host-networking) for more details.
    >
    > **CN:**  或者，修改虚拟机的设置，将端口4000自动转发到`localhost`。详情请参阅此[blog post](https://acaird.github.io/computers/2014/11/16/docker-virtualbox-host-networking)。
    >
    * Mount the current directory (`$PWD` is a Unix style environment variable. You can use `%CD%` on Windows, or specify the full path) as `/usr/src/app` inside the docker container. The docker image will see the `intellij-sdk-docs` repo as the folder `/usr/src/app`.
    
    * **CN:**  挂载当前目录(`$PWD`是一个Unix风格的环境变量。您可以在Windows上使用`%CD%`，或者指定完整路径)作为docker容器内的`/usr/src/app`。docker映像将看到`intellij-sdk-docs` repo作为文件夹`/usr/src/app`。
    
    > **NOTE** If running on Windows in an MSYS bash script (e.g. the "Docker Quickstart terminal"), the path to the local folder needs to be properly escaped, or the MSYS environment will translate the paths to standard Windows path, and causing an error such as `invalid value "C:\\Users\\...;C:\\Program Files\\Git\\usr\\src\\app" for flag -v`. To fix this problem, prefix the full path with double slashes, e.g. `-v //c/Users/...`, or `docker run -p 4000:4000 -v /$PWD:/usr/src/app intellij-sdk-docs` (note the leading slash before `$PWD`).
    >
    > **CN:**  如果在MSYS bash脚本(例如"Docker Quickstart terminal")中运行在Windows上，则需要正确地转义到本地文件夹的路径，否则MSYS环境将把这些路径转换为标准的Windows路径，并导致类似`invalid value "C:\\Users\\...;C:\\Program Files\\Git\\usr\\src\\app" for flag -v`的错误。要解决这个问题，可以在完整路径前面加上双斜杠，例如`-v //c/Users/...`或`docker run -p 4000:4000 -v /$PWD:/usr/src/app intellij-sdk-docs`(注意`$PWD`前面的斜杠)。
    >
    * Run the commands in the Dockerfile's `CMD` instruction to execute: 
    
    * **CN:**  运行Dockerfile的`CMD`指令中的命令执行:
    
    * `rake bootstrap`, which ensures all of the prerequisites are installed, 
    
    * **CN:**  `rake bootstrap`，它确保所有的先决条件都安装好了，
    
    * `rake preview`, which builds the site and starts to host it.
    
    * **CN:**  `rake preview`，它构建站点并开始托管它。
        

* Finally, you can access the newly created site by visiting [http://localhost:4000/intellij/sdk/docs/](http://localhost:4000/intellij/sdk/docs/), or by using the IP address of the docker client virtual machine (see the note above).

* **CN:**  最后，您可以通过访问[http://localhost:4000/intellij/sdk/docs/](http://localhost:4000/intellij/sdk/docs/)或使用docker客户机虚拟机的IP地址来访问新创建的站点(参见上面的说明)。

### Developing Documentation Locally——本地开发文档

To build the documentation site, you need:

**CN:**  建立文件网站，你需要:

* Ruby 2 - Jekyll is a Ruby application.

**CN:**  Ruby 2 - Jekyll是一个Ruby应用程序。

* Ruby 2 DevKit (for Windows) - Some of Jekyll's dependencies need to be compiled, and require the DevKit to be installed.

**CN:**  Ruby 2 DevKit (for Windows) - 需要编译Jekyll的一些依赖项，并要求安装DevKit。

* `gem install bundler` - the site uses [Bundler](https://bundler.io) to manage gem dependencies within the repo, rather than globally installing to the local operating system. Run this command to install the Bundler toolset globally.

**CN:**  `gem install bundler` -该网站使用[Bundler](https://bundler.io)在回购协议中管理gem依赖关系，而不是全球安装到本地操作系统。运行此命令全局安装Bundler工具集。

#### macOS——苹果OS系统

macOS comes with Ruby already installed. The only steps required are:

**CN:**  macOS已经安装了Ruby。所需的步骤只有:

* `gem install bundler`

#### Windows——Windows系统

* Install [Ruby 2](https://rubyinstaller.org) and the [Ruby 2 DevKit](http://rubyinstaller.org/downloads/) (one of the gems needs to build a native component)
    * After installing the DevKit, make sure to edit the `config.yml` file to point to the Ruby installation

**CN:**  
* 安装[Ruby 2](https://rubyinstaller.org)和[Ruby 2 DevKit](http://rubyinstaller.org/downloads/)(构建本地组件所需的gems之一)
    * 安装DevKit之后，确保编辑`config.yml`文件以指向Ruby安装

This installation is easier if you use [Chocolatey](https://chocolatey.org), a package manager for Windows:

**CN:**  如果你使用[Chocolatey](https://chocolatey.org), Windows包管理器，安装起来会更简单:

* `choco install ruby`
* `choco install ruby2.devkit`
    * After installing the DevKit, make sure to edit the `config.yml` file to point to the Ruby installation.
    
    **CN:**  安装DevKit之后，确保编辑`config.yml`文件以指向Ruby安装。
    
    * By default, this is `C:\tools\DevKit\config.yml`
    
    **CN:**  默认情况下，这是`C:\tools\DevKit\config.yml`
    
    * Add the line `- C:\tools\ruby21` (including the leading minus sign)

    **CN:**  添加行`- C:\tools\ruby21`(包括前导负号)
    
> **NOTE** Before running the `rake bootstrap` step listed below, please run the `devkitvars.bat` file from the DevKit. E.g. `C:\tools\DevKit\devkitvars.bat`
>
>**CN:**  注意：在运行下面列出的`rake bootstrap`步骤之前，请从DevKit运行`devkitvars.bat`文件。例如`C:\tools\DevKit\devkitvars.bat`
>
### Bootstrapping the Documentation Build Environment——启动文档构建环境

1. Ensure Bundler is installed - `gem install bundler`.
2. On Windows, ensure the `devkitvars.bat` file has been run in the current command prompt (e.g. `c:\tools\DevKit\devkitvars.bat`).
3. Clone the documentation site.
4. Initialize and update the `sdkdocs-template` submodule - `git submodule init` and `git submodule update`
5. `rake bootstrap` - this uses Bundler to download all required gems.
6. `rake preview` - this will build the site, and host it in a local webserver. 

**CN:**  
1. 确保安装了Ensure Bundler - `gem install bundler`。
2. 在Windows上，确保`devkitvars.bat`文件已在当前命令提示符(如`c:\tools\DevKit\devkitvars.bat`)中运行。
3. 克隆文档站点。
4. 初始化和更新`sdkdocs-template`子模块- `git submodule init`和`git submodule update`
5. `rake bootstrap` - 它使用Bundler下载所有需要的gem。
6. `rake preview` - 这将构建站点，并将其托管在本地web服务器中。


### Building and Previewing the Site——建设和预览网站

To build and test the site, run `rake preview`. This will build the site and host it, using the config provided. The URL of the hosted site is displayed on the screen and depends on the `baseurl` field defined in `_config.yml`.

**CN:**  要构建和测试站点，请运行`rake preview`。这将使用提供的配置来构建站点并托管它。托管站点的URL显示在屏幕上，并且依赖于`_config.yml`中定义的`baseurl`字段。

> **NOTE** You must use `localhost` as hostname, _NOT_ 0.0.0.0, otherwise fonts fail to load.
>
>**CN:**  注意：您必须使用`localhost`作为主机名， _NOT_  0.0.0.0，否则字体无法加载。
>
## Documentation Repository Submodules——文档存储仓库
The `sdkdocs-template` directory is actually a Git submodule, and it contains a submodule to the private `webhelp-template` repository. 
The `sdkdocs-template` repository contains build scripts and compiled and minified JS and CSS that allow the site to run. 
The private `webhelp-template` repository contains the code to build the JS and CSS. 
It is currently closed source, but the plan is to make it open source at some point, in which case it is likely the two repositories will be merged.

**CN:**  `sdkdocs-template`目录实际上是一个Git子模块，它包含一个到私有`webhelp-template`存储库的子模块。
         `sdkdocs-template`存储库包含构建脚本和已编译的、缩小的JS和CSS，它们允许站点运行。
         私有的`webhelp-template`存储库包含构建JS和CSS的代码。
         它目前是闭源的，但计划在某个时候使其成为开源的，在这种情况下，两个存储库可能会被合并。

After cloning, a submodule needs to be initialized and updated:

**CN:**  克隆完成后，需要对一个子模块进行初始化和更新:

```sh
git submodule init
git submodule update
```

Initialization creates a `.gitmodules` file, register a submodule in the `sdkdocs-template` folder and check out the files. 
Note that when a repo is added as a submodule, it doesn't get a `.git` folder, but instead gets a `.git` file that points to the actual location of the `.git` folder.

**CN:**  初始化创建一个`.gitmodules`文件，在`sdkdocs-template`文件夹中注册一个子模块并检出文件。
         注意，当repo作为子模块添加时，它不会获得`.git`文件夹，而是获得一个指向`.git`文件夹实际位置的`.git`文件。

A submodule can be updated using normal git commands such as `git pull`. 
It can be switched to a different branch using `git checkout`, and any changes to the currently checked out revision need to be committed back into the main repository using git commands. 
A submodule is initially cloned at a specific revision, and not as part of a branch.update

**CN:**  可以使用常规的git命令(如`git pull`)更新子模块。
         可以使用`git checkout`将其切换到另一个分支，对当前签出的修订的任何更改都需要使用git命令提交回主存储库。
         子模块最初是在特定的修订中克隆的，而不是作为分支.update的一部分

If changes are made to the submodule, they should be made on a branch to a clone, and a Pull Request sent. 
Changes can be made and committed, and the hosting repository will need to commit a pointer to the current version of the submodule.

**CN:**  如果对子模块进行了更改，则应在分支上对克隆进行更改，并发送Pull请求。
         可以进行更改并提交更改，宿主存储库需要提交指向子模块当前版本的指针。

If there are any problems with the `sdkdocs-template`, please [raise an issue](https://github.com/JetBrains/sdkdocs-template/issues).

**CN:**  如果`sdkdocs-template`有任何问题，请告知[raise an issue](https://github.com/JetBrains/sdkdocs-template/issues)。

## Creating IntelliJ Platform SDK Content——创建IntelliJ平台SDK内容
Content contributions to the IntelliJ Platform SDK are welcome.
Please download or clone the open source SDK project from [GitHub](https://github.com/JetBrains/intellij-sdk-docs), make additions or changes, and submit a pull request.
Before creating or altering content, please consult these guides:

**CN:**  欢迎对IntelliJ平台SDK的内容贡献。
         请从[GitHub](https://github.com/JetBrains/intellij-sdk-docs)下载或克隆开源SDK项目，进行添加或更改，并提交拉请求。
         在创建或修改内容之前，请参考以下指南:

* [SDK Documentation Style Guide](intro/sdk_style.md). 
  This guide describes documentation conventions in terms of Markdown syntax.
  Always test documentation changes using a [preview](#building-and-previewing-the-site) of the site.

**CN:**  [SDK Documentation Style Guide](intro/sdk_style.md)。
         本指南使用Markdown语法描述文档约定。
         始终使用站点的[preview](#building-and-previewing-the-site)测试文档更改。

* [SDK Code Sample Guidelines](intro/sdk_code_guidelines.md). 
  Conventions for code sample organization, project settings, and naming conventions are described in this document.
  Always test code changes by building and testing the SDK code sample.

**CN:**  [SDK Code Sample Guidelines](intro/sdk_code_guidelines.md)。
         本文档描述了代码示例组织、项目设置和命名约定。
         始终通过构建和测试SDK代码示例来测试代码更改。

