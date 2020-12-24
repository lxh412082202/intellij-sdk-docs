---
title: Indexing and PSI Stubs——索引和PSI存根
---

## Indices——指数

The indexing framework provides a quick way to locate certain elements, e.g. files containing a certain word or methods with a particular name, in large code bases. Plugin developers can use the existing indexes built by the IDE itself, as well as build and use their own indexes.

**CN:**  索引框架提供了一种快速定位特定元素的方法，例如，在大型代码库中包含特定单词或具有特定名称的方法的文件。插件开发人员可以使用IDE本身构建的现有索引，也可以构建和使用自己的索引。

It supports two main types of indexes:

**CN:**  它支持两种主要类型的索引:

* [File-based indices(基于文件的索引)](/basics/indexing_and_psi_stubs/file_based_indexes.md)
* [Stub indices(存根指数)](/basics/indexing_and_psi_stubs/stub_indexes.md)

File-based indexes are built directly over the content of files. Stub indexes are built over serialized *stub trees*. A stub tree for a source file is a subset of its PSI tree which contains only externally visible declarations and is serialized in a compact binary format.

**CN:**  基于文件的索引直接构建在文件的内容之上。存根索引是在序列化的*stub trees*上构建的。源文件的存根树是它的PSI树的一个子集，它只包含外部可见的声明，并以紧凑的二进制格式序列化。

Querying a file-based index gets you the set of files matching a certain condition. Querying a stub index gets you the set of matching PSI elements. Therefore, custom language plugin developers should typically use stub indexes in their plugin implementations.

**CN:**  查询基于文件的索引可以得到匹配特定条件的文件集。查询一个存根索引可以得到一组匹配的PSI元素。因此，定制语言插件开发人员通常应该在他们的插件实现中使用存根索引。

>> **TIP** [Indices Viewer](https://plugins.jetbrains.com/plugin/13029-indices-viewer/) is a plugin that helps inspecting indices' contents and properties. 
Please see also [Improving indexing performance](/reference_guide/performance/performance.md#improving-indexing-performance).
>>
>>**CN:**  提示：[Indices Viewer](https://plugins.jetbrains.com/plugin/13029-indices-viewer/)是一个插件，帮助检查索引的内容和属性。
          请参阅[Improving indexing performance](/reference_guide/performance/performance.md#improving-indexing-performance)。

## Dumb mode——哑模式

Indexing is a potentially long process. It's performed in background, and during this time, IDE's features are restricted to the ones that don't require index: basic text editing, version control etc. This restriction is managed by [`DumbService`](upsource:///platform/core-api/src/com/intellij/openapi/project/DumbService.java).

**CN:**  索引可能是一个很长的过程。它是在后台执行的，在此期间，IDE的特性仅限于那些不需要索引的特性:基本的文本编辑、版本控制等。这个限制由[`DumbService`](upsource:///platform/core-api/src/com/intellij/openapi/project/DumbService.java)管理。

`DumbService` provides API to query whether the IDE is currently in "dumb" mode (where index access is not allowed) or "smart" mode (with all index built and ready to use). It also provides ways of delaying code execution until indices are ready. Please see its javadoc for more details.

**CN:**  `DumbService`提供API来查询IDE当前是处于"dumb"模式(不允许索引访问)还是"smart"模式(构建所有索引并准备使用)。它还提供了在准备好索引之前延迟代码执行的方法。请查看它的javadoc以了解更多细节。

## Gists——依据

Sometimes, the following conditions hold:

**CN:**  有时，以下条件成立:

* the aggregation functionality of file-based indices is not needed. One just needs to calculate some data based on particular file's contents, and cache it on disk

**CN:**  不需要基于文件的索引的聚合功能。只需根据特定文件的内容计算一些数据，并将其缓存到磁盘上

* eagerly calculating the data for the entire project during indexing isn't needed (e.g. it slows down the indexing, and/or this data probably will ever be needed for a minor subset of all project files)

**CN:**  在索引期间急切地计算整个项目的数据是不需要的(例如，它降低了索引速度，并且/或者对于所有项目文件的一个小子集可能需要这些数据)

* the data can be recalculated lazily on request without major performance penalties

**CN:**  可以根据请求延迟地重新计算数据，而不会导致严重的性能损失


In such cases, file-based index can be used, but file gists provide a way to perform data calculation lazily, caching on disk, and a more lightweight API. Please see [`VirtualFileGist`](upsource:///platform/indexing-api/src/com/intellij/util/gist/VirtualFileGist.java) and [`PsiFileGist`](upsource:///platform/indexing-api/src/com/intellij/util/gist/PsiFileGist.java) documentation.

**CN:**  在这种情况下，可以使用基于文件的索引，但是file gist提供了一种延迟执行数据计算、在磁盘上缓存和更轻量级的API的方法。请参阅[`VirtualFileGist`](upsource:///platform/indexing-api/src/com/intellij/util/gist/VirtualFileGist.java)和[`PsiFileGist`](upsource:///platform/indexing-api/src/com/intellij/util/gist/PsiFileGist.java)文件。