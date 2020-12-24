---
title: Guidelines for Creating IntelliJ Platform SDK Code Samples——创建IntelliJ平台SDK代码示例的指南
---

This document describes the coding guidelines used for authoring open source IntelliJ Platform SDK code samples.
Before you begin, please read this page thoroughly as well as the [Code of Conduct](/CODE_OF_CONDUCT.md) and [License](https://github.com/JetBrains/intellij-sdk-docs/blob/master/LICENSE.txt) documents.
For information about contributing to the IntelliJ Platform itself visit [Contributing to the IntelliJ Platform](/basics/platform_contributions.md).

**CN:**  

  * Dummy list item
  {:toc}
  
## Objectives——目标
Keep the following considerations in mind while authoring an SDK code sample:

**CN:**  

* The purpose of an SDK sample is to demonstrate an implementation pattern to build on top of subsystems and components of the IntelliJ Platform.

**CN:**  

* SDK code samples are instructional code, intended to teach.

* **CN:**  

  * Instructional code differs from production code in some key aspects:
    
    * **CN:**  
    
    * Sacrifice some robustness in the interest of simplicity and brevity.
      Use error checking where it is necessary to make a point about an implementation pitfall.
    
    * **CN:**  
    
    * Keep implementations as simple as possible, but use the full features of the IntelliJ Platform, Java language, and libraries. 
    
    * **CN:**  
    
    * Use meaningful names for interfaces, classes, fields, methods, and extension points.
    
    * **CN:**  
    
    * Write instructional JavaDoc comments.    
  
    * **CN:**  
  
  * Code samples replace lots of documentation. 

  * **CN:**  

* Aim for two levels of SDK samples:

* **CN:**  

  * A _basic_ sample is focused on a particular subject by demonstrating a limited area of the IntelliJ Platform. 
    It should show all the components and architecture but ultimately accomplish something elementary. 
    For example, demonstrate persistence by storing only a `String`.

  * **CN:**  

  * An _advanced_ sample demonstrates how different parts of the IntelliJ Platform integrate and work together. 
    For example, combining inspections or intentions with non-trivial PsiTree manipulation.
 
  * **CN:** 
 
Ultimately, the goal is to provide developers with roadmaps for implementing functionality in an IntelliJ Platform-based plugin.
Each roadmap should contain:

**CN:**  

* Pointers to SDK documentation about the IntelliJ Platform APIs needed to implement the functionality.

**CN:**  

* Pointers to relevant _basic_ SDK sample plugins.

**CN:**  

* Pointers to related _advanced_ SDK sample plugins.

**CN:**  


## Naming Conventions for SDK Plugins——SDK插件的命名约定
For _basic_ samples, the naming convention is focused on the IntelliJ Platform APIs being demonstrated.
For example, `foo_basics` where `foo` corresponds to an IntelliJ Platform feature, framework, or component.
Some naming examples include `facet_basics` and `editor_basics`.
There is only one _basic_ sample per IntelliJ Platform API area.

**CN:**  

For _advanced_ code samples, the name should reflect the complex functionality delivered by the plugin rather than the IntelliJ Platform APIs.
Advanced samples will be cross-referenced to the IntelliJ Platform APIs demonstrated in the sample.

**CN:**  

Note that the naming convention relates to the `Artifact ID`, which is the Gradle property `rootProject.name`.
This is different than the `<name>` definition provided in the [`plugin.xml`](#pluginxml-conventions) file.
See the [Plugin Gradle Properties](/tutorials/build_system/prerequisites.md#plugin-gradle-properties-and-plugin-configuration-file-elements) section for more information about the distinction.

**CN:**  

## Plugin Copyright Statements——插件版权声明
Use the standard intellij-community copyright notice in all sample plugins authored by JetBrains:

**CN:**  

```text  
Copyright 2000-$today.year JetBrains s.r.o. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file."  
```
The copyright statement must appear in every source file with the `$today.year` [Velocity](https://www.jetbrains.com/help/idea/copyright-profiles.html) template resolved.

**CN:**  

## Plugin ID Conventions——插件ID约定
The `Group ID` for SDK plugins is always `org.intellij.sdk`.
In general, the plugin ID is the `Group ID` concatenated with the `Artifact ID`.

**CN:**  

For _basic_ code samples, it is not necessary to include "basic" in the plugin ID.
A plugin like `facet_basics` has the plugin ID `org.intellij.sdk.facet`.

**CN:**  

## Plugin Package Names——插件包名
Packages in plugins should begin with the plugin ID.
If there is only one package in a plugin, then the package name is the same as the plugin ID.

**CN:**  

## Plugin Directory Structure——插件的目录结构
SDK sample code should have a standard directory footprint.
Standardized structure not only makes the samples simpler to navigate and understand, but it builds on the default Gradle plugin project structure.
The following is the directory structure for a `foo_basics` plugin.

**CN:**  

```text
code_samples/
  foo_basics/
    gradle/
    src/
      main/
        java/
          icons/
            FooBasicsIcons.java
        resources/
          icons/
            sdk.svg           # The standard SDK icon for menus, etc.
          META-INF/
            plugin.xml        # The plugin configuration file
            pluginIcon.svg    # The standard SDK plugin icon
      test/                   # Omit if there are no tests
        java/
        resources/
    build.gradle
    gradlew
    gradle.bat
    settings.gradle
```

## build.gradle Conventions——build.gradle约定
New SDK code samples should be developed [using Gradle](/tutorials/build_system.md). 
As of this writing, the use of Gradle in SDK code samples still relies heavily on the `plugin.xml` for specifying the plugin configuration.
At a later, second phase, the SDK code samples will transition to rely more on the Gradle configuration. 

**CN:**  

The default contents of a `build.gradle` file are produced by the [New Project Wizard](/tutorials/build_system/prerequisites.md#creating-a-gradle-based-intellij-platform-plugin-with-new-project-wizard). 
A consistent structure for an SDK code sample's `build.gradle` file is important for clarity and is based on the default produced by the project wizard. 
Comments in SDK code sample `build.gradle` files should only draw attention to the parts of the Gradle configuration that are unique for a plugin.

**CN:**  

For SDK code samples, a few alterations are needed to the default build.gradle file produced by the plugin wizard:

**CN:**  

* Maintain the Gradle properties `version` (`project.version`) and `group` (`project.group`).
  See the [Plugin Gradle Properties](/tutorials/build_system/prerequisites.md#plugin-gradle-properties-and-plugin-configuration-file-elements) section for how these Gradle properties relate to the elements in `plugin.xml`.

**CN:**  

* Add the following statements to the [Setup DSL](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md#setup-dsl) (`intellij{}`) section:

**CN:**  

  ```groovy
      // Prevents patching <idea-version> attributes in plugin.xml
      updateSinceUntilBuild = false 
      // Define a shared sandbox directory for running code sample plugins within an IDE.
      sandboxDirectory = file("${project.projectDir}/../_idea-sandbox")
  ``` 
* Remove the [Patching DSL](https://github.com/JetBrains/gradle-intellij-plugin/blob/master/README.md#patching-dsl) (`patchPluginXml{}`) section.
  It is not needed in SDK samples.

**CN:**  

* Add the `javadoc` task to build documentation for the code sample:

**CN:**  

  ```groovy
    // Force javadoc rebuild before jar is built
    jar.dependsOn javadoc
  ```

## plugin.xml Conventions——plugin.xml约定
A consistent structure for the [`plugin.xml`](/basics/plugin_structure/plugin_configuration_file.md) configuration file of an SDK code sample is important because we want to draw attention to the unique parts of the file for a plugin.
Inconsistent element order is visually noisy.
Comment profusely about unique elements and configurations, and comment sparingly for the rest.

**CN:**  

The sequence of elements in an SDK code sample `plugin.xml` file is:

**CN:**  

* `<id>` Use the fully qualified [Plugin ID](#plugin-id-conventions).

**CN:**  

* `<name>` The name value does not have to match the [Plugin Name](#naming-conventions-for-sdk-plugins).
  It might reflect the functionality of the plugin.
  The name must start with "SDK: ".

**CN:**  

* `<version>` The code sample's version in MAJOR.MINOR.FIX format.

* **CN:**  

  * MAJOR corresponds to a significant upgrade in functionality.

  * **CN:**  

  * MINOR corresponds to minor refactoring and small improvements in functionality. 
  
  * **CN:**  
  
  * FIX corresponds to changes that fix problems without significant refactoring.

  * **CN:**  

* `<idea-version>` Set the attributes:

* **CN:**  

  * `since-build` attribute to the earliest compatible build number of the IntelliJ Platform.
    The default for SDK samples is "173".

  * **CN:**  

  * `until-build` Omit this attribute for new sample plugins.
    SDK code samples are reviewed before every major release (20XX.1) to ensure compatibility with the latest IntelliJ Platform.
    Add this attribute if a plugin sample is deprecated with a release of the IntelliJ Platform.

  * **CN:**  

* `<depends>` Include at least one dependency with the module `com.intellij.modules.platform` to indicate basic plugin compatibility with IntelliJ Platform-based products.
  Add `<depends>` elements containing module FQNs as needed to describe more specialized [Compatibility with Multiple Products](/basics/getting_started/plugin_compatibility.md), and any other [Plugin Dependencies](/basics/plugin_structure/plugin_dependencies.md). 

* **CN:**  

* `<description>` is a concise explanation of what is being demonstrated and how a user would access the functionality.

* **CN:**  

* `<change-notes>` is an ordered list by version numbers with a brief description of changes for each version.

* **CN:**  

* `<vendor>` Set the value to `IntelliJ Platform SDK`.
  Set the attributes:

* **CN:**  

  * `email` omit this attribute. 

  * **CN:**  

  * `url` to the JetBrains plugin repository `"https://plugins.jetbrains.com"`

  * **CN:**  

* The remainder of the [plugin configuration elements](/basics/plugin_structure/plugin_configuration_file.md) should only appear if they are needed by a specific plugin.

* **CN:**  

## Testing——测试
IntelliJ Platform SDK code samples should be built and tested against the `since-build` version.

**CN:**  

Code samples should build cleanly, with no warnings or errors, and new code samples should pass all default IntelliJ IDEA code inspections.

**CN:**  

Testers should complete the following checklist. 
Here the term "IDE" means the IntelliJ Platform-based IDE in which the plugin is designed to run:

**CN:**  

* The plugin should load in the IDE.

**CN:**  

* The correct information about the plugin should display in the **Preferences \| Plugins** panel.

**CN:**  

* If applicable, the plugin UI such as tool windows, menu additions, etc. should display correctly.

**CN:**  

* The functionality of the plugin should be tested with a sample file.

**CN:**  

* If applicable, the plugin should pass unit tests.

**CN:**  