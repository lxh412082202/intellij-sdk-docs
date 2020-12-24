---
title: Custom Language Support——自定义语言支持
---


*IntelliJ Platform* is a powerful platform for building development tools targeting *any* language.
Most of IDE features consist of language-independent and language-specific parts, and you can support a particular feature for your language with a small amount of effort:
you just need to implement the language-specific part, and the language-independent part is provided for you by the platform.

**CN:**  *IntelliJ Platform*是一个针对*any*语言构建开发工具的强大平台。
        大多数IDE特性由语言独立和特定于语言的部分组成，您可以通过少量的工作为您的语言支持特定的特性:
        您只需要实现特定于语言的部分，平台为您提供了独立于语言的部分。

This part of the documentation will explain the main concepts of the *Language API* and will guide you through the sequence of steps which are usually required to develop a custom language plugin.
You can obtain additional information about the *Language API* from the JavaDoc comments for the *Language API* classes and from the source code of the Properties language support, which is part of the
[IntelliJ IDEA Community Edition](https://github.com/JetBrains/intellij-community)
source code.

**CN:**  本部分的文档将解释*Language API*的主要概念，并将指导您完成开发自定义语言插件通常需要的一系列步骤。
        您可以从*Language API*类的JavaDoc注释和属性语言支持的源代码中获得关于*Language API*的更多信息
        [IntelliJ IDEA Community Edition](https://github.com/JetBrains/intellij-community)源代码。

If you prefer a full example to the detailed description offered on this page, please check out a step-by-step tutorial how to define custom language support on example of ".properties" files:
[Custom Language Support Tutorial](/tutorials/custom_language_support_tutorial.md)

**CN:**  如果你喜欢一个完整的例子，而不是在这个页面上提供的详细描述，请查看一个循序渐进的教程，如何定义自定义语言支持的例子的".properties"文件:
        [Custom Language Support Tutorial](/tutorials/custom_language_support_tutorial.md)

Providing custom language support includes the following major steps:

**CN:**  提供自定义语言支持包括以下主要步骤:

* [Registering File Type(注册文件类型)](/reference_guide/custom_language_support/registering_file_type.md)
* [Implementing Lexer(实现词法分析程序)](/reference_guide/custom_language_support/implementing_lexer.md)
* [Implementing Parser and PSI(实现解析器和PSI)](/reference_guide/custom_language_support/implementing_parser_and_psi.md)
* [Syntax Highlighting and Error Highlighting(语法突出显示和错误突出显示)](/reference_guide/custom_language_support/syntax_highlighting_and_error_highlighting.md)
* [References and Resolve(引用和解决)](/reference_guide/custom_language_support/references_and_resolve.md)
* [Code Completion(代码自动完成)](/reference_guide/custom_language_support/code_completion.md)
* [Find Usages(查找使用)](/reference_guide/custom_language_support/find_usages.md)
* [Rename Refactoring(重命名重构)](/reference_guide/custom_language_support/rename_refactoring.md)
* [Safe Delete Refactoring(安全删除重构)](/reference_guide/custom_language_support/safe_delete_refactoring.md)
* [Code Formatter(代码格式化程序)](/reference_guide/custom_language_support/code_formatting.md)
* [Code Inspections and Intentions(代码检查和意图)](/reference_guide/custom_language_support/code_inspections_and_intentions.md)
* [Structure View(结构视图)](/reference_guide/custom_language_support/structure_view.md)
* [Surround With(环绕)](/reference_guide/custom_language_support/surround_with.md)
* [Go to Class and Go to Symbol(转到类和转到符号)](/reference_guide/custom_language_support/go_to_class_and_go_to_symbol.md)
* [Documentation(文档)](/reference_guide/custom_language_support/documentation.md)
* [Additional Minor Features(额外的小特性)](/reference_guide/custom_language_support/additional_minor_features.md)


Please ask questions or suggest missing topics in [plugin development forum](https://intellij-support.jetbrains.com/hc/en-us/community/topics/200366979-IntelliJ-IDEA-Open-API-and-Plugin-Development).

**CN:**  请在[plugin development forum](https://intellij-support.jetbrains.com/hc/en-us/community/topics/200366979-IntelliJ-IDEA-Open-API-and-Plugin-Development)中提出问题或建议遗漏的主题。