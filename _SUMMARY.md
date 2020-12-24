# Summary

* [Introduction(介绍)](welcome.md)
* [The IntelliJ Platform(IntelliJ平台)](intro/intellij_platform.md)
    * [Contributing to the IntelliJ Platform(贡献给IntelliJ平台)](basics/platform_contributions.md)
    * [IntelliJ Platform Coding Guidelines(IntelliJ平台编码指南)](basics/intellij_coding_guidelines.md)
* [The IntelliJ Platform SDK(IntelliJ平台SDK)](intro/about.md)
    * [Key Topics(关键主题)](intro/key_topics.md)
    * [Contributing to the SDK(为SDK做贡献)](CONTRIBUTING.md)
      * [SDK Docs Style Guide(SDK文档风格指南)](intro/sdk_style.md)
      * [SDK Code Sample Guidelines(SDK代码示例指南)](intro/sdk_code_guidelines.md)
    * [Code of Conduct(行为准则)](CODE_OF_CONDUCT.md)
* [Getting Help(获取帮助)](intro/getting_help.md)
* [Recently Updated(最近更新)](recently_updated.md)

## Part I - Plugins
* [Introduction(介绍)](basics.md)
    * [Types of Plugins(插件类型)](basics/types_of_plugins.md)
* [Getting Started(开始)](basics/getting_started.md)
    * [Using Gradle(使用Gradle)](tutorials/build_system.md)
        * [Getting Started with Gradle(开始使用Gradle)](tutorials/build_system/prerequisites.md)
        * [Configuring Gradle Projects(配置Gradle项目)](tutorials/build_system/gradle_guide.md)
        * [Publishing Plugins with Gradle(使用Gradle发布插件)](tutorials/build_system/deployment.md)
    * [Using DevKit(使用DevKit)](basics/getting_started/using_dev_kit.md)
        * [Setting Up a Development Environment(设置开发环境)](basics/getting_started/setting_up_environment.md)
        * [Creating a Plugin Project(创建插件项目)](basics/getting_started/creating_plugin_project.md)
        * [Running and Debugging a Plugin(运行和调试插件)](basics/getting_started/running_and_debugging_a_plugin.md)
        * [Deploying a Plugin(部署插件)](basics/getting_started/deploying_plugin.md)
        * [Publishing a Plugin(发布插件)](basics/getting_started/publishing_plugin.md)
* [IDE Development Instances(IDE开发实例)](basics/ide_development_instance.md)
* [Custom Plugin Repositories(自定义插件存储库)](basics/getting_started/update_plugins_format.md)
* [Plugin Structure(插件结构)](basics/plugin_structure.md)
    * [Plugin Content(插件内容)](basics/plugin_structure/plugin_content.md)
    * [Plugin Class Loaders(插件类装载器)](basics/plugin_structure/plugin_class_loaders.md)
    * [Plugin Actions(插件操作)](basics/plugin_structure/plugin_actions.md)
    * [Plugin Extensions(插件扩展)](basics/plugin_structure/plugin_extensions.md)
    * [Plugin Services(插件服务)](basics/plugin_structure/plugin_services.md)
    * [Plugin Listeners(插件侦听器)](basics/plugin_structure/plugin_listeners.md)
    * [Plugin Extension Points(插件扩展点)](basics/plugin_structure/plugin_extension_points.md)
    * [Plugin Components(插件组件)](basics/plugin_structure/plugin_components.md)
    * [Plugin Configuration File(插件配置文件)](basics/plugin_structure/plugin_configuration_file.md)
    * [Plugin Logo (Icon)(插件Logo)](basics/plugin_structure/plugin_icon_file.md)
    * [Plugin Dependencies(插件依赖)](basics/plugin_structure/plugin_dependencies.md)
* [Dynamic Plugins(动态插件)](basics/plugin_structure/dynamic_plugins.md)    
* [IntelliJ Platform Artifacts Repositories(IntelliJ平台工件库)](reference_guide/intellij_artifacts.md)
* [Kotlin for Plugin Developers(Kotlin插件开发者)](tutorials/kotlin.md)
* [Internal Actions Menu(内部操作菜单)](reference_guide/internal_actions/internal_actions_intro.md)
    * [Enabling Internal Mode(开启内部模式)](reference_guide/internal_actions/enabling_internal.md)
    * [Internal Actions(内部操作)](reference_guide/internal_actions/interal_actions_menu.md)
    * [UI Tools(UI工具)](reference_guide/internal_actions/internal_ui_sub.md)
    * [UI Inspector(UI检查器)](reference_guide/internal_actions/internal_uii.md)
    * [Laf Defaults(Laf默认)](reference_guide/internal_actions/internal_ui_lafd.md)
* [Optimizing Performance(优化性能)](reference_guide/performance/performance.md)
* [Plugin Development FAQ(插件开发常见问题解答)](faq.md)

## Part II - Base Platform
* [Fundamentals(基本原理)](platform/fundamentals.md)
    * Component Model(组件模型)
    * Disposers
    * [Threading(线程)](basics/architectural_overview/general_threading_rules.md)
        * Background Tasks(后台任务)
    * [Messaging Infrastructure(消息传递基础结构)](reference_guide/messaging_infrastructure.md)
    * Queries and Query Executors(查询和查询执行器)
* [User Interface Components(用户界面组件)](user_interface_components/user_interface_components.md)
    * [Tool Windows(工具窗口)](user_interface_components/tool_windows.md)
    * [Dialogs(对话框)](user_interface_components/dialog_wrapper.md)
    * [Popups(弹出窗口)](user_interface_components/popups.md)
    * [Notifications(通知)](user_interface_components/notifications.md)
    * [File and Class Choosers(文件和类选择器)](user_interface_components/file_and_class_choosers.md)
    * [Editor Components(编辑器组件)](user_interface_components/editor_components.md)
    * [List and Tree Controls(列表和树控件)](user_interface_components/lists_and_trees.md)
    * [Miscellaneous Swing Components(其它Swing组件)](user_interface_components/misc_swing_components.md)
    * [Icons and Images(图标和图片)](reference_guide/work_with_icons_and_images.md)
    * [Color Scheme Management(配色方案管理)](reference_guide/color_scheme_management.md)
    * [Kotlin UI DSL(Kotlin UI DSL)](user_interface_components/kotlin_ui_dsl.md)
* [UI Themes(UI主题)](reference_guide/ui_themes/themes_intro.md)
    * [Creating UI Themes(创建UI主题)](reference_guide/ui_themes/themes.md)
    * [Customizing a UI Theme(自定义UI主题)](reference_guide/ui_themes/themes_customize.md)
    * [Adding Schemes and Images(添加方案和图像)](reference_guide/ui_themes/themes_extras.md)
    * [Exposing Theme Metadata(暴露主题元数据)](reference_guide/ui_themes/themes_metadata.md)
* [Actions(操作)](basics/action_system.md)
    * [Actions Tutorial(操作教程)](tutorials/action_system.md)
        * [Creating Actions(创建操作)](tutorials/action_system/working_with_custom_actions.md)
        * [Grouping Actions(操作分组)](tutorials/action_system/grouping_action.md)
* Settings(设置)
    * [Persisting State of Components(持久化组件状态)](basics/persisting_state_of_components.md)
    * [Persisting Sensitive Data(存储敏感数据)](basics/persisting_sensitive_data.md)
    * Editing Settings(编辑设置)
* [Files(文件)](basics/architectural_overview/files.md)
    * [Virtual File System(虚拟文件系统)](basics/virtual_file_system.md)
    * [Virtual Files(虚拟文件)](basics/architectural_overview/virtual_file.md)
* [Documents(文档)](basics/architectural_overview/documents.md)
* [Editors(编辑器)](reference_guide/editors.md)
    * [Editor Basics(编辑器基础)](tutorials/editor_basics.md)
        * [1. Working with Text(处理文本)](tutorials/editor_basics/working_with_text.md)
        * [2. Editor Coordinates System. Positions and Offsets(编辑器坐标系。位置和偏移)](tutorials/editor_basics/coordinates_system.md)
        * [3. Handling Editor Events(处理编辑事件)](tutorials/editor_basics/editor_events.md)
    * [Multiple Carets(多次插入)](reference_guide/multiple_carets.md)
* [Run Configurations(运行配置)](basics/run_configurations.md)
    * [Run Configuration Management(运行配置管理)](basics/run_configurations/run_configuration_management.md)
    * [Execution(执行)](basics/run_configurations/run_configuration_execution.md)
    * [Run Configurations Tutorial(运行配置教程)](tutorials/run_configurations.md)
* [Version Control Systems(版本控制系统)](reference_guide/vcs_integration_for_plugins.md)
    * Diff
    * Local History
* Tasks and Contexts
* [Localization Guide(本地化指南)](reference_guide/localization_guide.md)
* Diagrams

## Part III - Project Model 项目模型
* [Introduction(介绍)](basics/project_structure.md)
* [Project(项目)](reference_guide/project_model/project.md)
    * [Project Wizard(项目向导)](reference_guide/project_wizard.md)
    * [Project Wizard Tutorial(项目向导教程)](tutorials/project_wizard.md)
        * [Adding New Steps to Project Wizard(向项目向导添加新步骤)](tutorials/project_wizard/adding_new_steps.md)
        * [Supporting Module Types(支持模块类型)](tutorials/project_wizard/module_types.md)
    * [Frameworks(框架)](tutorials/framework.md)
* [Module(模块)](reference_guide/project_model/module.md)
* [SDK](reference_guide/project_model/sdk.md)
* [Library(图书馆)](reference_guide/project_model/library.md)
* [Facet(方面)](reference_guide/project_model/facet.md)
* [External System Integration(外部系统集成)](reference_guide/frameworks_and_external_apis/external_system_integration.md)

## Part IV - PSI
* [What is the PSI?(什么是PSI)](basics/architectural_overview/psi.md)
* [PSI Files(PSI文件)](basics/architectural_overview/psi_files.md)
* [File View Providers(文件视图提供者)](basics/architectural_overview/file_view_providers.md)
* [PSI Elements(PSI元素)](basics/architectural_overview/psi_elements.md)
* [Navigating the PSI(PSI导航)](basics/architectural_overview/navigating_psi.md)
* [References(参考)](basics/architectural_overview/psi_references.md)
* [Modifying the PSI(修改PSI)](basics/architectural_overview/modifying_psi.md)
* [PSI Cookbook(PSI食谱)](basics/psi_cookbook.md)
* [Indexing and PSI Stubs(索引和PSI存根)](basics/indexing_and_psi_stubs.md)
    * [File-based indexes(基于文件的索引)](basics/indexing_and_psi_stubs/file_based_indexes.md)
    * [Stub Indexes(存根索引)](basics/indexing_and_psi_stubs/stub_indexes.md)
* Element Patterns
* Unified AST
* [XML DOM API](reference_guide/frameworks_and_external_apis/xml_dom_api.md)

## Part V - Features 特性
* Navigation
    * Go To Symbol
* Editing
    * Code Completion
    * Templates
        * [Live Templates(实时模板)](tutorials/live_templates.md)
            * [1. Adding Live Template Support(添加实时模板支持)](tutorials/live_templates/template_support.md)
        * File Templates
        * Surround Templates
    * QuickDoc
    * [Intentions(意图)](tutorials/code_intentions.md)
* Analysing
    * Annotator
    * [Inspections(检查)](tutorials/code_inspections.md)
        * Profiles
        * Scopes
        * Suppressing Highlights
        * Structural Search
* Refactoring
* Project View
    * [Modifying Project View Structure(修改项目视图结构)](tutorials/tree_structure_view.md)
* Unit Testing 单元测试
* [Build System(构建系统)](reference_guide/project_model/build_system.md)
    * [External Builder API and Plugins(外部生成器API和插件)](reference_guide/frameworks_and_external_apis/external_builder_api.md)

## Part VI - Testing 测试

* [Testing Plugins(测试插件)](basics/testing_plugins/testing_plugins.md)
* [Tests and Fixtures(测试和固定装置)](basics/testing_plugins/tests_and_fixtures.md)
* [Light and Heavy Tests(轻、重测试)](basics/testing_plugins/light_and_heavy_tests.md)
* [Test Project and Testdata Directories(测试项目和Testdata目录)](basics/testing_plugins/test_project_and_testdata_directories.md)
* [Writing Tests(编写测试)](basics/testing_plugins/writing_tests.md)
* [Testing Highlighting(测试强调)](basics/testing_plugins/testing_highlighting.md)

## Part VII - Custom Languages
* [Custom Language Support(自定义语言支持)](reference_guide/custom_language_support.md)
    * [Registering File Type(注册文件类型)](reference_guide/custom_language_support/registering_file_type.md)
    * [Implementing Lexer(实现词法分析程序)](reference_guide/custom_language_support/implementing_lexer.md)
    * [Implementing Parser and PSI(实现解析器和PSI)](reference_guide/custom_language_support/implementing_parser_and_psi.md)
    * [Syntax Highlighting and Error Highlighting(语法突出显示和错误突出显示)](reference_guide/custom_language_support/syntax_highlighting_and_error_highlighting.md)
    * [References and Resolve(引用和解决)](reference_guide/custom_language_support/references_and_resolve.md)
    * [Code Completion(代码自动完成)](reference_guide/custom_language_support/code_completion.md)
    * [Find Usages(发现用法)](reference_guide/custom_language_support/find_usages.md)
    * [Rename Refactoring(重命名重构)](reference_guide/custom_language_support/rename_refactoring.md)
    * [Safe Delete Refactoring(安全删除重构)](reference_guide/custom_language_support/safe_delete_refactoring.md)
    * [Code Formatter(代码格式化程序)](reference_guide/custom_language_support/code_formatting.md)
    * [Code Inspections and Intentions(代码检查和意图)](reference_guide/custom_language_support/code_inspections_and_intentions.md)
    * [Structure View(结构视图)](reference_guide/custom_language_support/structure_view.md)
    * [Surround With(环绕)](reference_guide/custom_language_support/surround_with.md)
    * [Go to Class and Go to Symbol(转到类和转到符号)](reference_guide/custom_language_support/go_to_class_and_go_to_symbol.md)
    * [Documentation(文档)](reference_guide/custom_language_support/documentation.md)
    * [Additional Minor Features(额外的小特性)](reference_guide/custom_language_support/additional_minor_features.md)
    * Parameter Info
    * Parameter Hints
* [Custom Language Support Tutorial(自定义语言支持教程)](tutorials/custom_language_support_tutorial.md)
    * [1. Prerequisites(先决条件)](tutorials/custom_language_support/prerequisites.md)
    * [2. Language and File Type(语言和文件类型)](tutorials/custom_language_support/language_and_filetype.md)
    * [3. Grammar and Parser(语法和解析器)](tutorials/custom_language_support/grammar_and_parser.md)
    * [4. Lexer and Parser Definition(Lexer和解析器定义)](tutorials/custom_language_support/lexer_and_parser_definition.md)
    * [5. Syntax Highlighter and Color Settings Page(语法高亮和颜色设置页面)](tutorials/custom_language_support/syntax_highlighter_and_color_settings_page.md)
    * [6. PSI Helpers and Utilities(PSI助手和工具)](tutorials/custom_language_support/psi_helper_and_utilities.md)
    * [7. Annotator(注释器)](tutorials/custom_language_support/annotator.md)
    * [8. Line Marker Provider(线标记提供者)](tutorials/custom_language_support/line_marker_provider.md)
    * [9. Completion Contributor(完成贡献者)](tutorials/custom_language_support/completion_contributor.md)
    * [10. Reference Contributor(参考因素)](tutorials/custom_language_support/reference_contributor.md)
    * [11. Find Usages Provider(发现使用提供者)](tutorials/custom_language_support/find_usages_provider.md)
    * [12. Folding Builder(折叠创建者)](tutorials/custom_language_support/folding_builder.md)
    * [13. Go To Symbol Contributor(转到标记贡献者)](tutorials/custom_language_support/go_to_symbol_contributor.md)
    * [14. Structure View Factory(结构视图工厂)](tutorials/custom_language_support/structure_view_factory.md)
    * [15. Formatter(格式化程序)](tutorials/custom_language_support/formatter.md)
    * [16. Code Style Settings(代码风格设置)](tutorials/custom_language_support/code_style_settings.md)
    * [17. Commenter(注释)](tutorials/custom_language_support/commenter.md)
    * [18. Quick Fix(快速修复)](tutorials/custom_language_support/quick_fix.md)
* [Testing a Custom Language Plugin(测试自定义语言插件)](tutorials/writing_tests_for_plugins.md)
    * [1. Tests Prerequisites(测试先决条件)](tutorials/writing_tests_for_plugins/tests_prerequisites.md)
    * [2. Parsing Test(解析测试)](tutorials/writing_tests_for_plugins/parsing_test.md)
    * [3. Completion Test(完成测试)](tutorials/writing_tests_for_plugins/completion_test.md)
    * [4. Annotator Test(注释器测试)](tutorials/writing_tests_for_plugins/annotator_test.md)
    * [5. Formatter Test(格式化程序测试)](tutorials/writing_tests_for_plugins/formatter_test.md)
    * [6. Rename Test(重命名测试)](tutorials/writing_tests_for_plugins/rename_test.md)
    * [7. Folding Test(折叠测试)](tutorials/writing_tests_for_plugins/folding_test.md)
    * [8. Find Usages Test(发现使用测试)](tutorials/writing_tests_for_plugins/find_usages_test.md)
    * [9. Commenter Test(注释测试)](tutorials/writing_tests_for_plugins/commenter_test.md)
    * [10. Reference Test(参考测试)](tutorials/writing_tests_for_plugins/reference_test.md)
* Injected Languages
* Build System
* Compiler
* Debugger

## Part VIII - Product Specific 产品特定
* [Build Number Ranges(构建数字范围)](basics/getting_started/build_number_ranges.md)
* [Developing for Multiple Products(为多种产品开发)](products/dev_alternate_products.md)
* [Compatibility with Multiple Products(兼容多种产品)](basics/getting_started/plugin_compatibility.md)
* [Android Studio](products/android_studio.md)
* AppCode
* [CLion](products/clion.md)
* DataGrip
* [GoLand](products/goland.md)
* [IntelliJ IDEA](products/idea.md)
    * [Tomcat Integration(Tomcat集成)](reference_guide/tomcat_integration.md)
    * [Spring API](reference_guide/frameworks_and_external_apis/spring_api.md)
* [PhpStorm](products/phpstorm/phpstorm.md)
    * [Working with the PHP Open API(使用PHP开放API)](products/phpstorm/php_open_api.md)
    * [Existing Third Party Plugins(现有的第三方插件)](products/phpstorm/existing_plugins.md)
* [PyCharm](products/pycharm.md)
* [Rider](products/rider.md)
* [RubyMine](products/rubymine.md)
* [WebStorm](products/webstorm.md)

## Part IX - Custom IDEs 自定义ide
* Build Your Own IDE
* Licensing

## Part X - Plugin Repository \[moved()]
* [Overview()](plugin_repository/index.md)

## Appendix I - Resources

* [Useful Links(有用的链接)](appendix/resources/useful_links.md)
* [Consulting(咨询)](appendix/resources/consulting.md)

## Appendix II - API Changes

* [Incompatible API Changes()](reference_guide/api_changes_list.md)
    * [2020.*()](reference_guide/api_changes/api_changes_list_2020.md)
    * [2019.*()](reference_guide/api_changes/api_changes_list_2019.md)
    * [2018.*()](reference_guide/api_changes/api_changes_list_2018.md)
    * [2017.*()](reference_guide/api_changes/api_changes_list_2017.md)
    * [2016.*()](reference_guide/api_changes/api_changes_list_2016.md)

* [Notable API Changes()](reference_guide/api_notable/api_notable.md)
    * [2020.*()](reference_guide/api_notable/api_notable_list_2020.md)
    * [2019.*()](reference_guide/api_notable/api_notable_list_2019.md)
    * [2018.*()](reference_guide/api_notable/api_notable_list_2018.md)
