---
title: Style Guide for SDK Documents——SDK文档风格指南
---

This document describes the writing style used in authoring open source IntelliJ Platform SDK documentation.
Before you begin, please read this page thoroughly as well as the [Code of Conduct](/CODE_OF_CONDUCT.md) and [License](https://github.com/JetBrains/intellij-sdk-docs/blob/master/LICENSE.txt) documents.
For information about contributing to the IntelliJ Platform itself, please visit [Contributing to the IntelliJ Platform](/basics/platform_contributions.md).

**CN:**  本文档描述了编写开放源码IntelliJ平台SDK文档时使用的写作风格。
        在开始之前，请仔细阅读本页以及[Code of Conduct](/CODE_OF_CONDUCT.md)和[License](https://github.com/JetBrains/intellij-sdk-docs/blob/master/LICENSE.txt)文件。
        有关IntelliJ平台自身贡献的信息，请访问[Contributing to the IntelliJ Platform](/basics/platform_contributions.md)。

First and foremost, we should keep in mind our audience and their objectives:
_Someone reading technical content is usually looking to answer a specific question. 
That question might be broad or narrowly-focused, but either way, our goal is to provide answers without distraction._

**CN:**  首先，我们应该牢记我们的听众和他们的目标:
        阅读技术内容的人通常是想要回答某个特定的问题。
        这个问题的焦点可能很广，也可能很窄，但不管怎样，我们的目标是提供不受干扰的答案

The style of the _Intellij Platform SDK_ documentation is captured by using a markup language named [_Markdown_](https://github.github.com/gfm/).
The following sections describe the SDK documentation style in terms of the Markdown formats:

**CN:**   _Intellij Platform SDK_ 文档的风格是通过使用名为[_Markdown_](https://github.github.com/gfm/)的标记语言捕获的。
        下面几节将从标记格式的角度描述SDK文档风格:

* bullet list
{:toc}

## Documentation Markup Style——
By default, when building the site, all files are copied to the destination `_site` folder. 
Some files are excluded in the `_config.yml` and `sdkdocs-template/jekyll/_config-defaults.yml` files. 
The documentation files themselves are [Markdown](https://github.github.com/gfm/) files (`*.md`) that get automatically converted to HTML when the site is built.

**CN:**  默认情况下，在构建站点时，所有文件都复制到目标`_site`文件夹。
        有些文件被排除在`_config.yml`和`sdkdocs-template/jekyll/_config-defaults.yml`文件中。
        文档文件本身是[Markdown](https://github.github.com/gfm/)文件(`*.md`)，在构建站点时自动转换为HTML。

### Page Format——
Only Markdown files beginning with a [YAML](https://yaml.org) header are converted to HTML. 
If the Markdown file doesn't contain a header, it won't be converted. 
In other words, to convert a `.md` file to HTML, it should look like this:

**CN:**  只有以[YAML](https://yaml.org)头文件开头的Markdown文件才会被转换成HTML。
        如果Markdown文件不包含头文件，则不会对其进行转换。
        换句话说，要将`.md`文件转换成HTML，它应该是这样的:

```yaml
---
---

Lorem ipsum...
```

The two lines at the top of the file are the markers of the YAML _front matter_. 
Fields can be added in between these markers, and are used when generating the HTML. 
Typically, this header is empty, although it is required by Jekyll (if omitted, the file isn't converted).

**CN:**  文件顶部的两行是YAML  _front matter_ 的标记。
        可以在这些标记之间添加字段，并在生成HTML时使用。
        通常，这个头是空的，尽管Jekyll需要它(如果省略，文件就不会转换)。

The YAML header can contain data that is used when generating the site. 
For example, the page title can be specified as a simple piece of Markdown - `# Title`, or it can be specified in the YAML, and the page template displays it appropriately:

**CN:**  YAML头可以包含生成站点时使用的数据。
        例如，可以将页面标题指定为一个简单的Markdown - `# Title`片段，也可以在YAML中指定，页面模板会适当地显示它:

```yaml
---
title: The Title Of The Page
---

Lorem ipsum...
```

The YAML header can also include [redirect](#redirects) information.

**CN:**  YAML头还可以包含[redirect](#redirects)信息。

Line spacing around headings in Markdown files generally doesn't affect the HTML conversion, but it does make MD pages more readable for authors:

**CN:**  Markdown文件中标题周围的行间距通常不会影响HTML转换，但它确实使MD页面对作者更具可读性:

* No space between a heading and the first content below it.

**CN:**  标题和它下面的第一个内容之间没有空格。

* One space before a heading if it is the same level or a sub-heading of the previous section.

**CN:**  如果标题与前一节的标题是同一层或子标题，则在标题前留出一个空格。

* Two spaces before a heading that is higher-level than the heading of the previous section.

**CN:**  比上一节标题高一级的标题前的两个空格。

### Redirects——
The documentation site is set up to include the [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) plugin, which will generate "dummy" pages that automatically redirect to a given page. 
For example, to specify that the `index.html` page be generated to redirect to `README.md`, the `README.md` file should include the following in the YAML header:

**CN:**  文档站点的设置包括[jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from)插件，它将生成自动重定向到给定页面的"dummy"页面。
        例如，要指定生成`index.html`页来重定向到`README.md`, `README.md`文件应该在YAML头中包含以下内容:

```yaml
---
redirect_from:
  - /index.html
---

Lorem ipsum...
```

The redirect will create an `index.html` file that will automatically redirect to the generated `README.html` file. 
Redirects enable the site URL to automatically show the `README.html` file - `http://localhost:4001/foo-test/` will try to load `index.html`, which will automatically redirect to `README.html`.

**CN:**  重定向将创建一个`index.html`文件，该文件将自动重定向到生成的`README.html`文件。
        重定向使网站URL自动显示`README.html`文件- `http://localhost:4001/foo-test/`将尝试加载`index.html`，它将自动重定向到`README.html`。

It is also useful to redirect when renaming or moving files. 
Multiple redirects can be added to the YAML header.

**CN:**  在重命名或移动文件时重定向也很有用。
        可以将多个重定向添加到YAML头。

> **NOTE** Please update all existing internal links to the new page location.
>
> **CN:**  注意：请将所有现有的内部链接更新到新的页面位置。

### Table of Contents for a Page——页面的目录
The site is configured to use the [Kramdown Markdown converter](http://kramdown.gettalong.org), which adds some extra features over traditional Markdown.
For example, "attribute lists" that can apply attributes to the generated elements.

**CN:**  该站点被配置为使用[Kramdown Markdown converter](http://kramdown.gettalong.org)，这比传统的Markdown增加了一些额外的功能。
        例如，可以对生成的元素应用属性的"attribute lists"。

One useful attribute is `{:toc}`, which can be applied to a list item, which will get replaced with a list of links to header items. 
E.g. the following list item will be replaced by links to all of the header items in the page:

**CN:**  一个有用的属性是`{:toc}`，它可以应用于一个列表项，它将被一个链接到标题项的列表所取代。
        例如:下列列表项目将会被所有页眉项目的链接所取代:

```md
  * Dummy list item
  {:toc}
```

Further Kramdown features are described on the [converter page](https://kramdown.gettalong.org/converter/html.html), and attribute lists are described on the [syntax page](http://kramdown.gettalong.org/syntax.html). 
Note that source code formatting is configured to use [GitHub Flavoured Mardown](https://help.github.com/articles/github-flavored-markdown/) and "code fences", see below.

**CN:**  在[converter page](https://kramdown.gettalong.org/converter/html.html)中描述了进一步的Kramdown特性，在[syntax page](http://kramdown.gettalong.org/syntax.html)中描述了属性列表。
        请注意，源代码格式化配置为使用[GitHub Flavoured Mardown](https://help.github.com/articles/github-flavored-markdown/)和"code fences"，参见下面。

### Liquid tags and filters——Liquid标签及过滤器
Jekyll uses the [Liquid](https://shopify.github.io/liquid/) templating language to process files. 
This process means standard Liquid tags and filters are available. 
There should be little need to use them, however, as the Markdown format is already quite rich. 
See the [Jekyll site](https://jekyllrb.com/docs/liquid/) for more details.

**CN:**  Jekyll使用[Liquid](https://shopify.github.io/liquid/)模板语言来处理文件。
        这个过程意味着标准的液体标签和过滤器是可用的。
        但是，应该不需要使用它们，因为Markdown格式已经很丰富了。
        详见[Jekyll site](https://jekyllrb.com/docs/liquid/)。


### Text Format Conventions——文本格式规范
Consistent text styles are used to standardize references and keywords:

**CN:**  一致的文本样式用于标准化引用和关键字:

* Menu paths are formatted as bold with pipe characters separating each level: \*\*Preferences \\\| Editor\*\* (**Preferences \| Editor**)

**CN:**  菜单路径的格式为粗体，用管道字符分隔每一层:\*\*Preferences \\\| Editor\*\* (**Preferences \| Editor**)

* Keywords and quotations are formatted as italic: \_UI Theme\_ (_UI Theme_)

**CN:**  关键字和引文格式为斜体:\_UI Theme\_ (_UI Theme_)

* File names and paths, in-paragraph code fragments, and language keywords are formatted as code: \`interface\` (`interface`), 

**CN:**  文件名和路径、段内代码片段和语言关键字被格式化为代码: \`interface\` (`interface`), 

* File formats are shown as all capital letters: PNG and XML.

**CN:**  文件格式显示为所有大写字母:PNG和XML。

* File name extensions are not capitalized when part of a full file name, path, or URL: `filename.ext`.

**CN:**  当完整文件名、路径或URL: `filename.ext`的一部分时，文件名扩展名不大写。

* [Links](#links) have a particular format depending on the type of link.

**CN:**  根据链接的类型，[Links](#links)具有特定的格式。

Consistent terminology helps the reader grasp new concepts more quickly:

**CN:**  一致的术语有助于读者更快地掌握新概念:

* The open source code in the GitHub repository `intellij-community` is known as the _IntelliJ Platform_.
  Use the full phrase in SDK documentation.

**CN:**  GitHub存储库`intellij-community`中的开放源代码被称为 _IntelliJ Platform_ 。
        在SDK文档中使用完整的短语。

* IDEs based on the IntelliJ Platform are described as _IntelliJ Platform-based IDEs_.
  Once that term is used on a page, after that authors may use IDEs 

**CN:**  基于IntelliJ平台的ide被称为 _IntelliJ Platform-based IDEs_ 。
        一旦在页面上使用了该术语，之后作者可以使用IDE

* When referring to JetBrains products always use the full name such as _IntelliJ IDEA_.

**CN:**  当提到JetBrains的产品时，总是使用全名，比如 _IntelliJ IDEA_ 。

### Syntax highlighting——语法高亮显示
Source code can be represented by using [GitHub Flavoured Markdown](https://help.github.com/articles/github-flavored-markdown/) code fences, which are three backticks:

**CN:**  源代码可以用[GitHub Flavoured Markdown](https://help.github.com/articles/github-flavored-markdown/)代码栅栏表示，其中有三个反引号:

    ```
    // Source code goes here...
    ```

Syntax highlighting can be applied by specifying the language after the first set of ticks:

**CN:**  语法高亮可以通过指定语言后的第一组蜱:

    ```csharp
    // Some C# code
    ```

    ```java
    // Some Java code
    ```

Here is the list of [supported languages](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers), and also [Kotlin](https://kotlinlang.org), of course.

**CN:**  这是[supported languages](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers)的清单，当然还有[Kotlin](https://kotlinlang.org)。

Please keep code samples concise and avoid any unnecessary "surrounding" code or import statements. 

**CN:**  请保持代码样本的简洁，避免任何不必要的"surrounding"代码或导入语句。

<!-- //TODO: Not currently supported by rouge, or by the site's CSS

The site is also configured to highlight a range of files in the source code, by specifying `{start-end}` which is the start and end line of the highlighting:

    ```java{2-3}
    // Not highlighted
    // Highlighted
    // Highlighted
    // Not highlighted
    ```
-->

### Tables——表格
The Kramdown parser also supports tables. 
The syntax is to use the pipe (`|`) and minus symbols:

**CN:**  Kramdown解析器还支持表。
        语法是使用管道(`|`)和减号:

    ```md
    | Column 1 | Column 2 | Column 3 |
    |----------|----------|----------|
    | Blah     | Blah     | Blah     |
    ```

### Links——链接
Links are handled as normal Markdown links and can be anchored to external sites, pages within the sites, or headings in the sites. 
When a Markdown header is converted to an HTML header, it is assigned an ID so that it can be linked.
For example, `## Introduction` will get the ID of `introduction`, and can be linked either in the same page or cross-page as described below. 

**CN:**  链接处理为正常的Markdown链接，可以锚定到外部站点、站点内的页面或站点中的标题。
        当Markdown标头转换为HTML标头时，会为它分配一个ID，以便它可以被链接。
        例如，`## Introduction`将获得`introduction`的ID，并可以在同一页面或跨页面链接，如下所述。

Links to files in the IntelliJ Platform (`intellij-community`) repository use `upsource:///` instead of the full URL to the repository.
The `upsource:///` URI effectively points to the root of the `intellij-community` repository.

**CN:**  指向IntelliJ平台(`intellij-community`)资源库中的文件的链接使用的是`upsource:///`，而不是指向资源库的完整URL。
        `upsource:///` URI有效地指向`intellij-community`存储库的根。

* `[README.md](upsource:///README.md)` for links to general files in the repository.

**CN:**  `[README.md](upsource:///README.md)`用于链接到存储库中的常规文件。

* `[`\`AnAction\``](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)` for links to source code files for interfaces and classes.
  Note the use of \`\` characters surrounding the class name in the link.

**CN:**  `[`\`AnAction\``](upsource:///platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java)`指向接口和类的源代码文件的链接。
        请注意链接中围绕类名的\`\`字符的使用。

General links have one of the following formats:

**CN:**  一般链接有以下格式之一:

* `[External site](https://example.org)` links to an external site.

**CN:**  `[External site](https://example.org)`链接到外部站点。

* Linking to pages in the SDK documentation:

* **CN:**  链接到SDK文档中的页面:

  * `[SDK doc page in current directory](Page2.md)` links to an SDK doc page in the same directory as the current page. 
    Note that the extension is `.md`, _NOT_ `.html`.
  
  * **CN:**  `[SDK doc page in current directory](Page2.md)`链接到与当前页面位于同一目录下的SDK文档页面。
            注意分机是`.md`,  _NOT_ , `.html`。

  * `[SDK page in another folder](/Folder2/Page2.md)` links to a page in another folder. 
    Note that the URL is navigating from the project root - this works even if the site is hosted in a sub-folder (e.g. this link will work for `http://localhost:4000/devguide/Folder2/Page2.html`). 
    Relative links also work (`../Folder2/Page2.md`).

  * **CN:**  `[SDK page in another folder](/Folder2/Page2.md)`链接到另一个文件夹中的页面。
            请注意，URL是从项目根目录中导航的——即使该站点位于子文件夹中，这也是可行的(例如，该链接将在`http://localhost:4000/devguide/Folder2/Page2.html`中有效)。
            相对链接也可以(`../Folder2/Page2.md`)。


* Linking to specific _sections_ on pages in the SDK documentation.
  The anchor name will be all lower case, and spaces are replaced with `-`, e.g. `## Page setup` becomes `#page-setup`.

* **CN:**  链接到SDK文档中的特定 _sections_ 页面。
          锚点名称将全部小写，空格将替换为`-`，如`## Page setup`变为`#page-setup`。

  * `[Link to a section on the current page](#another-section)` links to a heading on the current page.
  
  * **CN:**  `[Link to a section on the current page](#another-section)`链接到当前页面的标题。

  * `[Link to the section on another page](Page2.md#another-section)` links to a heading on another page. 

  * **CN:**  `[Link to the section on another page](Page2.md#another-section)`链接到另一个页面的标题。


### Notes and callouts——注意和标注
Notes and callouts can be specified using the blockquote syntax. 
The converter looks at the first following word to see if it is bold. 
If so, it applies a callout style. 
The example below will be displayed as a callout, styled as a "note":

**CN:**  可以使用blockquote语法指定注释和标注。
        转换器查看下面的第一个单词，看看它是否是粗体。
        如果是，则应用callout样式。
        下面的例子将显示为一个调出，风格为一个"note":

    > **NOTE** This is a note

> **NOTE** This is a note
    

The styles available for callouts are:

**CN:**  可用于标注的样式有:

* TODO - Generally not used in SDK documentation.
  Embed `<--! //TODO: explanation... -->` instead, which is not visible in published documentation.

**CN:**  TODO - 一般不用于SDK文档。
               而是嵌入`<--! //TODO: explanation... -->`，这在已发布的文档中是不可见的。

* TIP - Information that makes the reader more productive.

**CN:**  TIP - 让读者更有效率的信息。

* NOTE - Information that is important for the reader to understand.
  This callout is reserved for key points and concepts.

**CN:**  NOTE - 对读者理解很重要的信息。
               这个标注是为关键点和概念保留的。

* WARNING - Information critical to the user.
  Use this callout for information that prevents failures or errors.

**CN:**  WARNING - 对用户至关重要的信息。
                  使用此调出来获取防止失败或错误的信息。

### Images——图片
Images can be included by adding the file directly to the `intellij-sdk-docs` repository.
Each subject directory typically has an `img` subdirectory.
Keep images close to the corresponding documentation file in the repository directory structure.

**CN:**  可以通过将文件直接添加到`intellij-sdk-docs`存储库来包含图像。
        每个主题目录通常都有一个`img`子目录。
        将图像放在存储库目录结构中相应的文档文件附近。

Images in this documentation are generally screenshots.
The preferred image format is PNG at 144 DPI resolution.
A resolution of 72 DPI is acceptable but may look blurry on high-resolution monitors.

**CN:**  本文档中的图像通常是屏幕截图。
        首选的图像格式是PNG，分辨率为144 DPI。
        72dpi的分辨率是可以接受的，但在高分辨率显示器上可能会显得模糊。

It is important to reduce the size of image files to prevent bloating the repository and impacting the performance of the documentation site.
Resize an image to be nearly the desired width on a documentation page.
Reducing an image's dimensions is the most effective way to reduce image file size. 
Also optimize the image files using a tool such as the [PNG optimizer](https://plugins.jetbrains.com/plugin/7942-png-optimizer).

**CN:**  减少图像文件的大小对于防止存储库膨胀和影响文档站点的性能非常重要。
        将图像的大小调整为文档页上所需的宽度。
        减小图像的尺寸是减小图像文件大小最有效的方法。
        还可以使用[PNG optimizer](https://plugins.jetbrains.com/plugin/7942-png-optimizer)等工具优化图像文件。

Images are embedded in a document by adding a Markdown link to the image like so:

**CN:**  图像是嵌入在一个文件中，通过添加一个标记链接到图像像这样:

    ![Alt text](path-to-img.png)

If the width of an image needs to be adjusted, use Kramdown markup:

**CN:**  如果图像的宽度需要调整，使用Kramdown标记:

    ![Alt text](path-to-img.png){:width="42px"}


## _SUMMARY Site Table of Contents——
The table of contents for the site is displayed in the tree view on the left-hand side of the site, and it is generated from the `_SUMMARY.md` file. 
It is a simple Markdown list, with each item in the list being a link to another Markdown page, either in the root folder, or sub-folders. 
The list can have nested items, which are displayed as child items in the table of contents.

**CN:**  站点的目录表显示在站点左侧的树视图中，它是由`_SUMMARY.md`文件生成的。
        它是一个简单的Markdown列表，列表中的每一项都是指向根文件夹或子文件夹中的另一个Markdown页面的链接。
        列表可以有嵌套的项，这些项在目录中显示为子项。

> ***WARNING*** Every Markdown (`*.md`) document within the SDK repository (`intellij-sdk-docs`) must have an entry in `_SUMMARY.md`.
>
>**CN:**  警告：SDK库(`intellij-sdk-docs`)中的每个Markdown (`*.md`)文档都必须在`_SUMMARY.md`中有一个条目。

```md
# Summary

* [Introduction](README.md)
* [About This Guide](Intro/About.md)
    * [Key Topics](Intro/KeyTopics.md)
```

The contents can be split into "parts" by separating the list into several lists, each new list starting with a level 2 heading (`##`).

**CN:**  通过将列表分成几个列表，每个新列表以一个级别2的标题(`##`)开始，内容可以被分成"parts"。

```md
# Summary

* [Introduction](README.md)
* [About This Guide](Intro/About.md)
    * [Key Topics](Intro/KeyTopics.md)

## Part I - Extending the Platform
* [Getting Started](Docs/GettingStarted.md)
* ...
```

If a node doesn't have a link but is just plain text, it will still appear in the table of contents but will be greyed out and not clickable. 
It acts as a placeholder for a documentation item. 
A placeholder is useful to keep track of what should be documented, but hasn't yet, and can be useful to show readers that the topic exists, but isn't yet documented (Pull Requests always welcome!).

**CN:**  如果一个节点没有链接，但只是纯文本，它仍然会出现在目录中，但会变灰，不能点击。
        它充当文档项的占位符。
        占位符对于跟踪应该记录但还没有记录的内容很有用，对于向读者显示主题存在但还没有记录的内容也很有用(拉请求总是受欢迎的!)
