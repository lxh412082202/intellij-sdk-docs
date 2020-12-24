---
title: Working with Icons and Images 使用图像和图标
---

Icons and images are used widely by IntelliJ Platform plugins. Plugins need icons mostly for actions, custom components renderers, tool windows and so on.
图标和图像已被IntelliJ Platform插件广泛使用。插件主要需要用于操作的图标，自定义组件渲染器，工具窗口等。

> **NOTE** Plugin Icons, which represent a plugin itself, have different requirements than icons and images used within a plugin.
For more information see the [Plugin Icon](/basics/plugin_structure/plugin_icon_file.md) page. 
插件图标代表插件本身，其要求不同于插件中使用的图标和图像。有关更多信息，请参见[Plugin Icon](/basics/plugin_structure/plugin_icon_file.md)页面。

> **TIP** Plugins should reuse existing platform icons whenever possible, see [`AllIcons`](upsource:///platform/util/src/com/intellij/icons/AllIcons.java). A detailed [design guideline](https://jetbrains.design/intellij/principles/icons/) is available for creating custom icons.
插件应尽可能重用现有的平台图标，请参见
[`AllIcons`](upsource:///platform/util/src/com/intellij/icons/AllIcons.java)。
详细的[design guideline](https://jetbrains.design/intellij/principles/icons/)可用于创建自定义图标。

## How to organize and how to use icons?如何组织和使用图标？

The best way to deal with icons and other image resources is to put them to a dedicated source root, say *"icons"* or *"resources"*.
处理图标和其他图像资源的最佳方法是将它们放置在专用的源根目录中，例如“ icons”或“ resources”。

![Icons](img/icons1.png)

The `getIcon()` method of [`IconLoader`](upsource:///platform/util/ui/src/com/intellij/openapi/util/IconLoader.java) can be used to access the icons. Then define a class or an interface with icon constants in a top-level package called `icons`:
[`IconLoader`](upsource:///platform/util/ui/src/com/intellij/openapi/util/IconLoader.java)
的`getIcon()`方法可用于访问图标。然后在一个名为`icons`的顶级包中定义一个带有图标常量的类或接口:

```java
package icons;

public interface DemoPluginIcons {
  Icon STRUCTURE_TOOL_WINDOW = IconLoader.getIcon("/icons/toolWindowStructure.png");
  Icon MY_LANG_FILE_TYPE = IconLoader.getIcon("/icons/myLangFileType.png");
  Icon DEMO_ACTION = IconLoader.getIcon("/icons/demoAction.png");
}
```

Use these constants inside `plugin.xml` as well. Note that the package name `icons` will be automatically prefixed, and shouldn't be added manually.
在`plugin.xml`内部也使用这些常量。注意，包名“icons”将自动添加前缀，不应该手动添加。

```xml
<action id="DemoPlugin.DemoAction"
        class="com.jetbrains.demoplugin.actions.DemoAction"
        text="Demo Action"
        description="This is just a demo"
        icon="DemoPluginIcons.DEMO_ACTION"/>
```

### Image formats 图片格式

IntelliJ Platform supports Retina displays and has dark theme called Darcula. Thus, every icon should have a dedicated variant for Retina devices and Darcula theme. In some cases, you can skip dark variants if the original icon looks good under Darcula.
IntelliJ平台支持视网膜显示，并有一个名为Darcula的黑暗主题。因此，每个图标都应该有视网膜设备和Darcula主题的专用变体。在某些情况下，如果原始图标在Darcula下看起来不错，你可以跳过黑色的变体。

Required icon sizes depend on the usage as listed in the following table:
所需的图标大小取决于下表所列的用法:

| Usage | Icon Size (pixels) |
|-------|--------------------|
| Node, Action, Filetype | 16x16 |
| Tool window            | 13x13 |
| Editor gutter          | 12x12 |


#### SVG format SVG格式
> **NOTE** SVG icons are supported since 2018.2.从2018.2版本开始支持SVG图标。

As SVG icons can be scaled arbitrarily, they provide better results on HiDPI environments or when used in combination with bigger screen fonts (e.g., in presentation mode).
由于SVG图标可以任意缩放，所以它们在HiDPI环境中或与更大的屏幕字体结合使用时(例如，在表示模式中)可以提供更好的结果。

A base size denoting the size (in the user space) of the rendered image in 1x scale should be provided. The size is set via the `width` and `height` attributes omitting the size units. If unspecified, it defaults to 16x16 pixels.
应该提供表示以1x比例呈现的图像的大小(在用户空间中)的基本大小。大小是通过“宽度”和“高度”属性设置的，忽略了大小单位。如果未指定，则默认为16x16像素。

A minimal SVG icon file:
一个最小的SVG图标文件:
```xml
<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16">
 <rect width="100%" height="100%" fill="green"/>
</svg>
```

The naming notation used for PNG icons (see below) is still relevant. However, the `@2x` version of an SVG icon should still provide the same base size. The icon graphics of such an icon can be expressed in more details via double precision. If the icon graphics are simple enough so that it renders perfectly in every scale, then the `@2x` version can be omitted. 
用于PNG图标的命名符号(见下面)仍然是相关的。但是，SVG图标的`@2x`版本应该仍然提供相同的基本大小。这种图标的图标图形可以通过双精度来表达更多的细节。如果图标图形足够简单，使它在每个比例完美呈现，那么`@2x`版本可以省略。

#### PNG format  PNG格式
> **NOTE** Please consider using SVG icons if your plugin targets 2018.2+.如果你的插件目标是2018.2+，请考虑使用SVG图标。

All icon files must be placed in the same directory following this naming pattern (replace `.png` with `.svg` for SVG icons):
所有图标文件必须按照此命名模式放置在相同的目录中(将.png替换为.svg作为SVG图标)

* **iconName.png** W x H pixels (Will be used on non-Retina devices with default theme. 将被用于默认主题的非视网膜设备)
* **iconName@2x.png** 2\*W x 2\*H pixels (Will be used on Retina devices with default theme. 将被用于默认主题的视网膜设备)
* **iconName_dark.png** W x H pixels (Will be used on non-Retina devices with Darcula theme. 将用于Darcula主题的非视网膜设备。)
* **iconName@2x_dark.png** 2\*W x 2\*H pixels (Will be used on Retina devices with Darcula theme. 将用于视网膜装置与Darcula的主题。)

The `IconLoader` class will load the icon that matches the best depending on the current environment.
类`IconLoader`将根据当前环境加载最匹配的图标。

Here are examples of *toolWindowStructure.png* icon representations:
下面是*toolWindowStructure.png*图标代表的例子:

| Theme/Resolution | File name                         | Image |
|------------------|-----------------------------------|-------|
| Default          | `toolWindowStructure.png`         | ![Tool Window Structure](img/toolWindowStructure.png) |
| Darcula          | `toolWindowStructure_dark.png`    | ![Tool Window Structure, dark](img/toolWindowStructure_dark.png) |
| Default + Retina | `toolWindowStructure@2x.png`      | ![Tool Window Structure, retina](img/toolWindowStructure@2x.png) |
| Darcula + Retina | `toolWindowStructure@2x_dark.png` | ![Tool Window Structure, retina, dark](img/toolWindowStructure@2x_dark.png) |

