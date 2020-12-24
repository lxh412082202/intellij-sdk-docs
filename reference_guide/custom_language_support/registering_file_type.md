---
title: Registering a File Type——注册文件类型
---

The first step in developing a custom language plugin is registering a file type associated with the language.
The IDE normally determines the type of a file by looking at its file name.

**CN:**  开发自定义语言插件的第一步是注册与该语言相关的文件类型。
        IDE通常通过查看文件的文件名来确定文件的类型。

A custom language file type is a class derived from
[`LanguageFileType`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java),
which passes a
[`Language`](upsource:///platform/core-api/src/com/intellij/lang/Language.java)
subclass to its base class constructor.

**CN:**  自定义语言文件类型是从[`LanguageFileType`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java)派生的类，
        它将一个[`Language`](upsource:///platform/core-api/src/com/intellij/lang/Language.java)子类传递给它的基类构造函数。

To register a file type, the plugin developer provides a subclass of
[`FileTypeFactory`](upsource:///platform/platform-api/src/com/intellij/openapi/fileTypes/FileTypeFactory.java), which is registered via the `com.intellij.fileTypeFactory` extension point.

**CN:**  为了注册一个文件类型，插件开发者提供了一个[`FileTypeFactory`](upsource:///platform/platform-api/src/com/intellij/openapi/fileTypes/FileTypeFactory.java)的子类，它是通过`com.intellij.fileTypeFactory`扩展点注册的。

> **NOTE** When targeting 2019.2 or later only, using `com.intellij.fileType` extension point is preferred to using dedicated `FileTypeFactory`.
> 
> **CN:**  注意：当仅针对2019.2或更高版本时，使用`com.intellij.fileType`扩展点比使用专用`FileTypeFactory`更好。


**Examples**:
- [`LanguageFileType`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java)
subclass in
[Properties language plugin](upsource:///plugins/properties/properties-psi-api/src/com/intellij/lang/properties/PropertiesFileType.java)

**CN:**  在[Properties language plugin](upsource:///plugins/properties/properties-psi-api/src/com/intellij/lang/properties/PropertiesFileType.java)中的[`LanguageFileType`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java)子类

- [Custom Language Support Tutorial: Language and File Type](/tutorials/custom_language_support/language_and_filetype.md)

**CN:**  [Custom Language Support Tutorial: Language and File Type(自定义语言支持教程:语言和文件类型)](/tutorials/custom_language_support/language_and_filetype.md)


To verify that the file type is registered correctly, you can implement the
[`LanguageFileType.getIcon()`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java)
method and verify that the correct icon (see [Working with Icons and Images](/reference_guide/work_with_icons_and_images.md)) is displayed for files associated with your file type.

**CN:**  要验证文件类型是否正确注册，您可以实现[`LanguageFileType.getIcon()`](upsource:///platform/core-api/src/com/intellij/openapi/fileTypes/LanguageFileType.java)方法并验证与您的文件类型相关的文件显示了正确的图标(参见[Working with Icons and Images](/reference_guide/work_with_icons_and_images.md))。

If you want IDEs to show a hint prompting users that your plugin supports a specific file type, see [Plugin Recommendations](https://plugins.jetbrains.com/docs/marketplace/intellij-plugin-recommendations.html).

**CN:**  如果您希望ide显示提示，提示用户您的插件支持特定的文件类型，请参阅[Plugin Recommendations](https://plugins.jetbrains.com/docs/marketplace/intellij-plugin-recommendations.html)。