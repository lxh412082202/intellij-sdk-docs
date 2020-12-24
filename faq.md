---
title: Plugin Development FAQ
---


This FAQ is a topical index of questions that have been asked (and answered) on our
[IntelliJ IDEA Open API and Plugin Development forum](https://intellij-support.jetbrains.com/hc/en-us/community/topics/200366979-IntelliJ-IDEA-Open-API-and-Plugin-Development).

**CN:**  这个常见问题是一个主题索引的问题，已被问(和回答)在我们的[IntelliJ IDEA Open API and Plugin Development forum(IntelliJ IDEA开放API和插件开发论坛)](https://intellij-support.jetbrains.com/hc/en-us/community/topics/200366979-IntelliJ-IDEA-Open-API-and-Plugin-Development)。

## Open-Source Plugins——开源插件
*  [How do I compile the Scala plugin?(如何编译Scala插件?)](https://github.com/jetbrains/intellij-scala#setting-up-the-project)

## Action System——action系统
*  [How do I trigger actions programmatically?(如何以编程方式触发操作?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206130119-Triggering-AnAction-instances-)
*  [How do I add a main menu item?(如何添加主菜单项?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206750335-Add-new-Main-menu-option-for-plugin)
*  [How do I customize the "New..." menu?(如何自定义“新…”菜单?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206765055-Overriding-the-New-context-menu-in-the-Project-view)
*  [How do I customize the compiler output context menu?(如何自定义编译器输出上下文菜单?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142169-How-to-add-a-menu-item-below-Exclude-From-Compile-)
*  [How do I get the context of an action (selected file, active project etc.)?(我如何得到一个action的上下文(选定的文件，活动的项目等)?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206756455-Question-about-Actions)
*  [How do I change the icon of an action dynamically?(如何动态更改action的图标?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206763405-How-to-dynamically-change-icons-in-the-tool-bar-)
*  [How do I add icons to the IDEA toolbar?(如何向IDEA工具栏添加图标?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206151289-How-to-add-icons-to-the-toolbar-)
*  [Where do I get the list of built-in action IDs?(我从哪里获得内建action ID的列表?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206126699-List-of-built-in-action-ID-s-)

## Accessing and Modifying the Source Code——访问和修改源代码
*  [PSI Architectural Overview(PSI的体系结构概述)](/basics/architectural_overview/psi.md)
*  [How do I find all subclasses of a class?(如何查找一个类的所有子类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791895-finding-all-derived-class-given-parent-class)
*  [How do I find all anonymous classes created in a class?(如何查找在一个类中创建的所有匿名类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206792205-How-to-find-anonymous-classes-in-PsiClass-)
*  [How do I calculate the value of a string literal token?(如何计算字符串文字标记的值?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139829-How-to-evaluate-the-value-of-PsiJavaToken-of-STRING-LITERAL-type)
*  [How do I rename or move a Java class?(如何重命名或移动Java类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791825-How-to-rename-a-class-)
*  [How do I build the list of all classes used by a given class or package?(如何构建给定类或包使用的所有类的列表?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139469-Using-DependencyValidationManager-to-Get-Required-Classes)
*  [How do I insert whitespace into the PSI?(如何在PSI中插入空白?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143839-Adding-PsiElements-to-a-PsiFile)
*  [How do I add properties to a .properties file?(如何向.properties文件添加属性?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142279-Dynamically-add-new-properties-to-properties-files)
*  [How do I find specific method calls inside a PsiMethod?(如何在PsiMethod中找到特定的方法调用?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143579-finding-a-statement-within-a-PsiMethod)
*  [What is the lifecycle of a PSI element?(PSI元素的生命周期是什么?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206796015-What-is-the-lifecycle-of-a-PsiElement-)
*  [How do I find a file given its name (but no path)?(我如何找到一个文件的名称(但没有路径)?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206768795-How-to-search-file-by-file-name-in-project-s-root-directory-)
*  [How do I create a new class in the given package?(如何在给定的包中创建新类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206771665-Creating-a-new-class)
*  [How do I make a PsiClass extend another PsiClass?(如何创建一个PsiClass继承自另外一个PsiClass?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206794255-How-to-make-a-PsiClass-derive-from-another-one-)
*  [How do I find references to a class from non-Java files?(如何从非java文件中查找对类的引用?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800695-How-to-obtain-the-references-to-a-class-from-non-java-files-)
*  [How do I find the source file given the path to a .class file?(如果给定.class文件的路径，如何查找源文件?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800595-How-to-find-the-source-for-a-class-file)
*  [How do I find classes with the specified non-qualified name?(如何查找具有指定非限定名的类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206146759-How-to-resolve-unqualified-name-to-possible-PsiClasses-)
*  [How do I find the module in which a class is located?(如何查找类所在的模块?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206103859-How-to-get-Module-from-PsiClass-)
*  [PSI Cookbook(PSI食谱)](/basics/psi_cookbook.md)

## Working with XML and XML DOM——使用XML和XML DOM
*  [How do I change the value of an XML attribute through the PSI?(如何通过PSI更改XML属性的值?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139639-Change-xml-attribute-value)
*  [How do I add custom references to Java elements in XML files?(如何在XML文件中向Java元素添加自定义引用?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206795875-XmlTagValue-reference-to-Java-methods)
*  [How do I programmatically register a DTD or schema?(如何以编程方式注册DTD或模式?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206795425-How-to-register-DTD-with-idea)
*  [What is the "strict" parameter in DomElement.getParentOfType()?(DomElement.getParentOfType()中的"strict"参数是什么?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791535-DOM-DomElement-getParentOfType)

## Code Completion——代码自动完成
*  [How do I determine what type of code completion was invoked?(如何确定调用了哪种类型的代码完成?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206133529-How-to-determine-what-type-of-code-completion-was-invoked)
*  [How do I provide additional code completion in specific places of a Java file?(如何在Java文件的特定位置提供额外的代码补全?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139729-Custom-completion-in-editor)

## Refactoring——重构
*  [How can I receive notifications about refactoring events?(如何接收重构事件的通知?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206795955-Refactoring-Listeners)
*  [How do I show a refactoring dialog programmatically?(如何以编程方式显示重构对话框?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800005-How-to-invoke-refactoring-dialog-not-refactoring-itself-)

## Run/Debug——运行/调试
*  [How do I implement a custom run configuration?(如何实现自定义运行配置?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143009-Creating-a-Run-type-)

## Make/Compile——使/编译
*  [How do I get access to class files generated by javac?(如何访问由javac生成的类文件?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800625-Implementing-a-ClassInstrumentingCompiler-how-to-get-the-generated-class-files)

## Version Control Integration——版本控制集成
*  [Can I provide line status markers for files in a custom file system?(我可以为自定义文件系统中的文件提供行状态标记吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791585-Editor-diff-functionality-for-custom-file-system)
*  [How do I update the state of VCS actions depending on file status?(如何根据文件状态更新VCS操作的状态?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791975-VCS-context-menu)
*  [How can I find out the module of a deleted file?(如何查找已删除文件的模块?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206792195-Module-for-deleted-file-)
*  [Can I provide additional decorations for changelists in the Changes view?(我可以在Changes视图中为变更列表提供额外的修饰吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139549-Is-it-possible-to-decorate-change-lists-)
*  [How do I report out-of-date files?(如何报告过期的文件?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791775-VCS-OpenAPI-what-to-do-with-files-detected-as-out-of-date-)

## Test Framework——测试框架
*  [How do I create a library dependency in my test module?(如何在测试模块中创建库依赖项?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791555-library-depndency-in-InspectionTestCase)

## Plugin Architecture——插件体系结构
*  [How do I provide a custom exception reporter for my plugin?(我如何为我的插件提供一个自定义异常报告器?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206793965-Custom-exception-reporting)
*  [How can I add the help topics of my plugin to the IntelliJ Platform help system?(如何将插件的帮助主题添加到IntelliJ平台帮助系统中?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206760095-how-do-i-plug-into-the-main-idea-help-system-)
*  [How do I get the version of IntelliJ Platform under which my plugin is running?(我如何获得运行我的插件的IntelliJ平台的版本?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800275-How-to-get-the-idea-version)

## Editors, Documents and Files——编辑器、文档和文件
*  [Why doesn't the file change on disk after I changed it through the PSI?(为什么我通过PSI修改后，磁盘上的文件没有改变?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791625-Action-doesn-t-see-changes-in-xml-file)
*  [Can I hook into the file save logic?(我能钩入文件保存逻辑吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206790685-Can-you-tie-into-the-file-save-logic-)
*  [Can I mark a part of a file as read-only?(我可以将文件的一部分标记为只读吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/207042355-Read-only-section-in-editor)
*  [How do I control what happens when the user tries to edit such a part?(我如何控制当用户试图编辑这样一个部分时发生了什么?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791375-Using-locked-regions)
*  [How do I implement a custom editor?(如何实现自定义编辑器?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143969-Rough-guide-to-xml-gui-editor-type-plugin-)
*  [How can I show several editors for a single file in tabs?(How can I show several editors for a single file in tabs?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206795495-Alternative-Editors-ala-HTML-Preview)
*  [Can I open an editor which has no underlying file on disk?(我可以打开一个没有在磁盘上的基础文件的编辑器吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206135449-Create-an-Editor-for-a-non-physical-file)
*  [How do I save the content of my custom editor when the user saves all documents?(当用户保存所有文档时，如何保存自定义编辑器的内容?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206792085-Catching-the-Save-All-action)
*  [How do I highlight elements in a source code editor?(如何在源代码编辑器中突出显示元素?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143909-MarkupModel-navigate-highlighted-elements)
*  [How do I allow to navigate between highlighted elements using Find Next?(如何使用Find Next在突出显示的元素之间导航?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143879-HighlightManager-how-to-enable-F3-functionality)
*  [How do I force code to be re-analyzed?(如何强制代码重新分析?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143679-Forcing-an-annotator-to-update-status-of-a-file)
*  [How do I get the active editor instance?(如何获得活动编辑器实例?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206141119-how-to-get-the-Editor-from-PsiElement-)
*  [How do I get the cursor position in the current editor?(如何在当前编辑器中获取光标位置?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206794335-How-to-get-cursor-position-in-the-current-editor-)
*  [How do I clear the read-only status of a file?(如何清除文件的只读状态?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142039-Clear-read-only-status)
*  [How do I show a popup hint in an editor?(如何在编辑器中显示弹出提示?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206146719-HintManager-API-question)
*  [How do I create live template-like red box edit regions in an editor?(如何在编辑器中创建动态模板样的红色框编辑区域?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800165-How-to-%C3%A7reate-live-template-like-red-box-edit-regions-in-an-editor)
*  [How can I show an editor with error highlighting in a tool window?(如何在工具窗口中显示带有错误高亮显示的编辑器?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206146679-Error-highlighting-in-Editors)

## Inspections——检查
*  [Can I build an inspection that processes XML files?(我可以构建一个处理XML文件的检查吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139579-LocalInspectionTool-for-XML-files-/comments/206204765)
*  [Why are the inspection results shown multiple times?(为什么检验结果要显示多次?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142489-visitXmlAttribute-question)
*  [How can I provide a quick fix that creates a method?(如何提供创建方法的快速修复?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142769-Triggering-Create-Method-intention)
*  [Is it possible to inspect only the elements that have been modified after the last full inspection?(是否可以只检查上次全面检查后修改过的元素?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800645-How-to-inspect-only-the-elements-modified-since-the-last-class-inspection)
*  [ExternalAnnotator not in sync with current editor(ExternalAnnotator与当前编辑器不同步)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/115000337510-Only-trigger-externalAnnotator-when-the-file-system-is-in-sync)

## Project Structure——项目结构
*  [Can I add a new module dependency storage format?(我可以添加一个新的模块依赖存储格式吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206137859-Dependency-storage-formats-)
*  [What is the Pair to be passed to JavaModuleBuilder.setSourcePaths()?(要传递给JavaModuleBuilder.setSourcePaths()的内容是什么?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143559-Usage-of-class-Pair-A-B-)
*  [How do I access all configured JDKs?(如何访问所有配置的jdk ?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206141569-selecting-a-configured-jdk)

## Custom Languages——自定义语言
*  [How do I provide Parameter Info support for my language?(如何为我的语言提供参数信息支持?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791995-Parameter-Info)
*  [How do I provide auto-popup code completion in my language?(如何在我的语言中提供自动弹出式代码完成?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139359-Autopopup-code-completion-in-custom-language)
*  [How to make a closing brace unindent?(如何使右括号缩进?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206797085-Custom-Language-How-to-make-a-closing-brace-unindent-)
*  [How to automatically insert closing quotes?(如何自动插入结束引号?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206144059-How-the-insertion-of-closing-quote-works-in-Javascript-plugin-)
*  [How do I provide Ctrl+mouse popups for my language?(如何为我的语言提供Ctrl+鼠标弹出窗口?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206797015-Ctrl-mouse)
*  [How do I enable debugging for my custom language which is compiled into Java?(如何为编译成Java的自定义语言启用调试?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206786875-Debugging-custom-languages-)
*  [How do I generate virtual Java classes mirroring the classes of my language?(我如何生成虚拟Java类镜像我的语言的类?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143749-Custom-languages-masquarding-as-a-java-source-file-within-IntelliJ)

## User Interface——用户接口
*  [How do I provide animated status bar notifications?(如何提供动态状态栏通知?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206791945-IDE-Notifications)
*  [How do I enable file name completion in a combobox?(如何在组合框中启用文件名补全?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139509-Combobox-with-Browse-Button-and-Autocompletion-)
*  [How do I show a popup with left-aligned and right-aligned parts for each item?(我如何显示一个弹出窗口与左对齐和右对齐的每个项目的部分?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139049-popup-menu-with-left-and-right-aligned-items)
*  [Can I use an embedded Web browser from my plugin?(我可以从我的插件使用嵌入式网络浏览器吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206351479-Using-Browser-Component)
*  [How do I provide a custom icon for files/PSI elements?(如何为文件/PSI元素提供自定义图标?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143779-Is-it-possible-to-change-icon-of-file-in-Project-view-)
*  [Can I show a progress indicator for WriteActions?(我可以为WriteActions显示一个进度指示器吗?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206139159-WriteActions-and-ProgressIndicator)
*  [How do I make it possible to search the options of my plugin in the Settings dialog?(我怎样才能在设置对话框中搜索我的插件选项?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206143999-DEMETRA-how-to-contribute-to-searchable-dialog-options-)
*  [How do I show a custom window or popup based on Structure View?(我如何显示一个自定义窗口或弹出基于结构视图?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142679-Opening-a-customised-StructureView)
*  [Is it possible to extend the Project View tree?(是否可以扩展项目视图树?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206141229-is-it-possible-to-extend-the-project-tree-)
*  [How do I show the "Project Structure" dialog programmatically?(如何以编程方式显示“项目结构”对话框?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206141379-Showing-Project-Strucuture-dialog-programmatically)
*  [How do I print messages in the console view?(如何在console视图中打印消息?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206141419-Putting-messages-into-console-window-)
*  [How do I show the package selector dialog programmatically?(如何以编程方式显示包选择器对话框?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206794265-Package-selector-dialog)
*  [How do I provide syntax and error highlighting in a combo box editor?(如何在组合框编辑器中提供语法和错误高亮显示?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800495-EditorTextField-in-3403-How-to-get-an-Editor-that-does-error-highlighting-)
*  [How can I get notified when my tool window is activated?(当我的工具窗口被激活时，我如何得到通知?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206800405-How-can-i-run-some-code-when-a-ToolWindow-activates)
*  [How can I provide Close and Rerun buttons in my Usage View window?(如何在Usage视图窗口中提供关闭和重新运行按钮?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206146779-How-to-get-a-Close-button-in-an-own-Usage-View-)
*  [How can I display the SDK list in a JComboBox?(如何在JComboBox中显示SDK列表?)](https://stackoverflow.com/questions/51499884/how-to-display-the-sdk-list-in-a-jcombobox)

## General
*  [How do I get the currently active project outside of an AnAction?(如何在AnAction之外获得当前活动的项目?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206763335-Getting-active-project-)
*  [How do I detect when a project is closing?(如何检测项目何时结束?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206792155-Detecting-frame-project-closing)
*  [How can I implement a custom stack trace analyzer?(如何实现自定义堆栈跟踪分析器?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206142959-Stack-Analyzer-extension)
*  [Where is the state of an ApplicationComponent stored?(ApplicationComponent的状态存储在哪里?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206794095-Where-is-ApplicationComponent-state-stored-in-)
*  [How do I open a project programmatically?(如何以编程方式打开项目?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206146969-how-to-open-a-project-)
*  [How do I get the folder of the currently selected file?(如何获得当前选定文件的文件夹?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206121889-How-to-get-the-folder-of-currenctly-selected-file)
*  [How do I encrypt some values in the configuration data of my plugin?(我如何加密我的插件配置数据中的一些值?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206147039-JDOMExternalizable-and-encrypting-)
*  [How can I track exceptions from my plugin?(如何从我的插件跟踪异常?)](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206762245-How-can-I-track-plugin-s-exceptions-/comments/206112345)
