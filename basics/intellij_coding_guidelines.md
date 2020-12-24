---
title: IntelliJ Platform Coding Guidelines——IntelliJ平台编码指南
---

If you are writing code that you would like to contribute to the IntelliJ Platform (either as a patch or as a plugin), following these guidelines will make it easier for the JetBrains development team to review and accept your changes.

**CN:**  如果您正在编写希望贡献给IntelliJ平台的代码(作为补丁或插件)，遵循这些指导方针将使JetBrains开发团队更容易地审查和接受您的更改。

#### Following the Latest Source Code——遵循最新的源代码

If you submit patches, we strongly recommend building your patches against the latest version of the code from the Git repository. The easiest way to do so is to clone the JetBrains Git repository, track your work in Git, and create patches using the "git format-patch" command.

**CN:**  如果您提交补丁，我们强烈建议您根据Git存储库中最新版本的代码构建补丁。最简单的方法是克隆JetBrains Git存储库，在Git中跟踪您的工作，并使用“Git format-patch”命令创建补丁。

#### General Architectural Principles——总体架构原则

Please do your best to follow common Java architectural principles. "Effective Java" by Joshua Bloch is a good place to start.

**CN:**  请尽量遵循常见的Java体系结构原则。约书亚•布洛赫(Joshua Bloch)的《高效Java》(Effective Java)一书是一个不错的起点。

#### Tests——测试

Most of the existing functionality of IntelliJ IDEA is covered by functional tests. If the area you're modifying is covered by tests, you must run the tests and make sure that your changes do not introduce any new test failures. It's also strongly recommended that you provide new functional tests that cover the bugs you fix or the new features that you add.

**CN:**  IntelliJ IDEA的大部分现有功能都被功能测试覆盖了。如果测试覆盖了您正在修改的区域，那么您必须运行测试，并确保您的更改不会引入任何新的测试失败。还强烈建议您提供新的功能测试，以覆盖您修复的bug或您添加的新功能。

#### Code Formatting——代码格式化

We're generally pretty lax about code formatting, but at least the following conventions must be observed:

**CN:**  一般来说，我们对代码格式很宽松，但至少必须遵守以下约定:

- 2 space indents in source files

**CN:**  源文件中的两个空格缩进

- **my** prefix for instance variables and **our** prefix for class variables

**CN:**  实例变量的前缀和类变量的前缀

- new source code files must include a copyright statement with the Apache 2 license and the name of the contributor.

**CN:**  新的源代码文件必须包含Apache 2许可的版权声明和贡献者的名称。

The easiest way to follow our code formatting guidelines is to reformat your code submissions using the shared code style, which is included in the IntelliJ IDEA Community Edition project directory.

**CN:**  遵循我们的代码格式化指南的最简单的方法是使用共享代码样式重新格式化您的代码提交，该样式包含在IntelliJ IDEA Community Edition项目目录中。

#### Inspections——检查

The IntelliJ IDEA Community Edition project includes a shared inspection profile. We strongly recommend making sure that the code you submit does not contain any warnings highlighted by the inspections configured in that inspection profile.

**CN:**  IntelliJ IDEA社区版项目包括一个共享的检查配置文件。我们强烈建议确保您提交的代码不包含在检查配置文件中配置的检查所突出显示的任何警告。

#### JavaDoc Comments——JavaDoc注释

If your code adds new OpenAPI interfaces, classes, methods or extension points, you must provide JavaDoc comments describing the parameters and intended usage of the APIs. Providing JavaDoc or other comments for other parts of the code is a good idea but isn't required.

**CN:**  如果您的代码添加了新的OpenAPI接口、类、方法或扩展点，您必须提供JavaDoc注释来描述参数和api的预期用途。为代码的其他部分提供JavaDoc或其他注释是一个好主意，但不是必需的。

#### Commits——提交

To avoid unnecessary work when reviewing your changes, please follow these guidelines:

**CN:**  为了避免不必要的工作时，审查你的变化，请遵循以下准则:

- Look through all of your changes in your patch or pull request before you submit it to us. Make sure that everything you've changed is there for a reason.

**CN:**  在您提交给我们之前，请查看您的补丁或拉请求中的所有更改。确保你所改变的一切都是有原因的。

- Please don't include unfinished work to the patch. Make sure that it doesn't include any TODO comments. If you added some code and ended up not needing it, please make sure that you delete it before you submit your patch.

**CN:**  请不要将未完成的工作添加到补丁中。确保它不包含任何TODO注释。如果您添加了一些代码，但最终不需要它，请确保在提交补丁之前删除它。

- Please don't include any changes that affect formatting, fixing "yellow code" (warnings) or code style along with actual changes that fix a bug or implement a feature. No one likes to leave poor code, but remember that having these changes mixed with each other complicates the process of review.

**CN:**  请不要包含任何影响格式、修复“黄色代码”(警告)或代码样式的更改，以及修复错误或实现功能的实际更改。没有人喜欢留下糟糕的代码，但是请记住，将这些更改混合在一起会使评审过程变得复杂。

- Please don't fix multiple problems within a single patch or pull request.

**CN:**  请不要在一个补丁或拉请求中修复多个问题。

- Please don't commit your personal changes to configuration files (runConfigurations/IDEA.xml, codeStyleSettings.xml, misc.xml, etc.) unless it is essential for the fix itself.

**CN:**  请不要将您的个人更改提交到配置文件(runConfigurations/IDEA.xml, codeStyleSettings.xml, misc.xml等)，除非对修复本身是必要的。

- Please avoid moving or renaming classes unless it is necessary for the fix.

**CN:**  除非必要，否则请避免移动或重命名类。

The ideal pull request would contain 1 commit with everything needed to fix the bug or implement a feature, but nothing else. "Commit early, commit often" perfectly applies only to local commits, but such "public commits" are hard to review (the reviewer needs either to go commit by commit spending more time to review work-in-progress, or to review all changes at once thus losing valuable information stored in commit messages).

**CN:**  理想的pull请求应该包含一个commit，包含修复bug或实现某个特性所需的所有内容，但不包含其他内容。“尽早提交，经常提交”完全只适用于本地提交，但是这种“公共提交”很难进行审查(审查者需要一次又一次地提交，花费更多的时间来审查正在进行的工作，或者一次审查所有的更改，从而丢失存储在提交消息中的有价值的信息)。

The best would be to commit early, but then to squash all commits into one with a descriptive commit message.

**CN:**  最好是尽早提交，然后使用描述性提交消息将所有提交压缩到一个提交中。

Sometimes several commits for a single issue are also acceptable, but each of these need to be self-contained "steps" to solve the problem.

**CN:**  有时对单个问题进行多次提交也是可以接受的，但是每一次提交都需要是自包含的“步骤”来解决问题。