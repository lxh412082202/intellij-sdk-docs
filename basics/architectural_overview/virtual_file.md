---
title: Virtual Files——虚拟文件
---

A virtual file [`VirtualFile`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFile.java) is the *IntelliJ Platform's* representation of a file in a file system (VFS). Most commonly, a virtual file is a file in your local file system. However, the *IntelliJ Platform* supports multiple pluggable file system implementations, so virtual files can also represent classes in a JAR file, old revisions of files loaded from a version control repository, and so on.

**CN:**  虚拟文件[`VirtualFile`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFile.java)是IntelliJ平台对文件系统(VFS)中文件的表示。最常见的情况是，虚拟文件是本地文件系统中的文件。然而，IntelliJ平台支持多种可插拔的文件系统实现，因此虚拟文件也可以表示JAR文件中的类、从版本控制库中加载的旧版本文件等等。

The VFS level deals only with binary content. You can get or set the contents of a `VirtualFile` as a stream of bytes, but concepts like encodings and line separators are handled on higher system levels.

**CN:**  VFS级别只处理二进制内容。您可以将`VirtualFile`的内容作为一个字节流获取或设置，但是像编码和行分隔符这样的概念是在更高的系统级别上处理的。

## How do I get a virtual file?——如何获得虚拟文件?

* From an action: `e.getData(PlatformDataKeys.VIRTUAL_FILE)`. If you are interested in multiple selection, you can also use `e.getData(PlatformDataKeys.VIRTUAL_FILE_ARRAY)`.

**CN:**  从action中：`e.getData(PlatformDataKeys.VIRTUAL_FILE)`.如果您对多重选择感兴趣，也可以使用`e.getData(PlatformDataKeys.VIRTUAL_FILE_ARRAY)`。

* From a path in the local file system: `LocalFileSystem.getInstance().findFileByIoFile()`

**CN:**  从一个路径在本地文件系统:`LocalFileSystem.getInstance().findFileByIoFile()`。

* From a PSI file: `psiFile.getVirtualFile()` (may return null if the PSI file exists only in memory)

**CN:**  从一个PSI文件：`psiFile.getVirtualFile()` (如果PSI文件仅存在于内存中，则返回null)

* From a document: `FileDocumentManager.getInstance().getFile()`

**CN:**  从一个文档:`FileDocumentManager.getInstance().getFile()`

## What can I do with it?——我能用它做什么?

Typical file operations are available, such as traverse the file system, get file contents, rename, move, or delete. Recursive iteration should be performed using `VfsUtilCore.iterateChildrenRecursively` to prevent endless loops caused by recursive symlinks.

**CN:**  典型的文件操作是可用的，例如遍历文件系统、获取文件内容、重命名、移动或删除。递归迭代应该使用`VfsUtilCore.iterateChildrenRecursively`来执行，以防止递归符号链接导致的无限循环。

## Where does it come from?——它来自哪里?

The VFS is built incrementally, by scanning the file system up and down starting from the project root. New files appearing in the file system are detected by VFS _refreshes_. A refresh operation can be initiated programmatically using (`VirtualFileManager.getInstance().refresh()` or `VirtualFile.refresh()`). VFS refreshes are also triggered whenever file system watchers receive file system change notifications (available on the Windows and Mac operating systems).

**CN:**  通过从项目根开始向上和向下扫描文件系统，逐步构建VFS。VFS刷新将检测文件系统中出现的新文件。可以使用(`VirtualFileManager.getInstance().refresh()`或`VirtualFile.refresh()`)以编程方式启动刷新操作。每当文件系统监视者收到文件系统更改通知(在Windows和Mac操作系统上可用)时，也会触发VFS刷新。

As a plugin developer, you may want to invoke a VFS refresh if you need to access a file that has just been created by an external tool through the IntelliJ Platform APIs.

**CN:**  作为插件开发人员，如果需要通过IntelliJ平台API访问由外部工具创建的文件，可能需要调用VFS刷新。

## How long does a virtual file persist?——虚拟文件持续多长时间?

A particular file on disk is represented by equal `VirtualFile` instances for the entire lifetime of the IDEA process. There may be several instances corresponding to the same file, and they can be garbage-collected. The file is a `UserDataHolder`, and the user data is shared between those equal instances. If a file is deleted, its corresponding VirtualFile instance becomes invalid (the `isValid()` method returns `false` and operations cause exceptions).

**CN:**  在整个IDEA流程的生命周期中，磁盘上的特定文件由相等的`VirtualFile`实例表示。同一文件可能有多个对应的实例，可以对它们进行垃圾收集。该文件是`UserDataHolder`，用户数据在这些相等的实例之间共享。如果一个文件被删除，那么它对应的VirtualFile实例将变为无效(`isValid()`方法返回`false`，操作将导致异常)。

## How do I create a virtual file?——如何创建虚拟文件?

Usually you don't. As a rule, files are created either through the PSI API or through the regular java.io.File API.

**CN:**  通常你不需要创建。通常，文件要么通过PSI API创建，要么通过常规的java.io.File API创建。

If you do need to create a file through VFS, you can use the `VirtualFile.createChildData()` method to create a `VirtualFile` instance and the `VirtualFile.setBinaryContent()` method to write some data to the file.

**CN:**  如果确实需要通过VFS创建文件，可以使用`VirtualFile.createChildData()`方法创建`VirtualFile`实例，使用`VirtualFile.setBinaryContent()`方法向文件写入一些数据。

## How do I get notified when VFS changes?——当VFS发生变化时，我如何得到通知?

> **NOTE** See [Virtual file system events](/basics/virtual_file_system.md#virtual-file-system-events) for important details.
>
>**CN:**  有关重要细节，请参阅[Virtual file system events(虚拟文件系统事件)](/basics/virtual_file_system.md#virtual-file-system-events)。

Implement the [`BulkFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/newvfs/BulkFileListener.java) interface and subscribe to the [message bus](/reference_guide/messaging_infrastructure.md) topic `VirtualFileManager.VFS_CHANGES`. For example:

**CN:**  实现[`BulkFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/newvfs/BulkFileListener.java)接口并订阅[message bus(消息总线)](/reference_guide/messaging_infrastructure.md)主题`VirtualFileManager.VFS_CHANGES`。例如:

```java
project.getMessageBus().connect().subscribe(VirtualFileManager.VFS_CHANGES, new BulkFileListener() {
    @Override
    public void after(@NotNull List<? extends VFileEvent> events) {
        // handle the events
    }
});
```

See [Message Infrastructure](/reference_guide/messaging_infrastructure.md) and [Plugin Listeners](/basics/plugin_structure/plugin_listeners.md) for more details.

**CN:**  参阅[Message Infrastructure(信息基础设施)](/reference_guide/messaging_infrastructure.md)和[Plugin Listeners(插件监听器)](/basics/plugin_structure/plugin_listeners.md)获取更多信息。

For a non-blocking alternative, starting with version 2019.2 of the platform, see [`AsyncFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/AsyncFileListener.java)

**CN:**  对于非阻塞的替代方案，从平台的2019.2版本开始，请参阅[`AsyncFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/AsyncFileListener.java)

Plugins targeting versions 2017.2 or older of the platform can use the now deprecated `VirtualFileManager.addVirtualFileListener()` method which allows you to receive notifications about all changes in the VFS.

**CN:**  针对版本2017.2或更老的平台的插件可以使用现在弃用的`VirtualFileManager.addVirtualFileListener()`方法，它允许您接收关于VFS中所有更改的通知。

## Are there any utilities for analyzing and manipulating virtual files?——有分析和操作虚拟文件的工具吗?

[`VfsUtil`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/VfsUtil.java) and [`VfsUtilCore`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VfsUtilCore.java) provide utility methods for analyzing files in the Virtual File System.

**CN:**  [`VfsUtil`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/VfsUtil.java) 和 [`VfsUtilCore`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VfsUtilCore.java)提供了分析虚拟文件系统中的文件的实用方法。

You can use [`ProjectLocator`](upsource:///platform/projectModel-api/src/com/intellij/openapi/project/ProjectLocator.java) to find the projects that contain a given virtual file.

**CN:**  您可以使用[`ProjectLocator`](upsource:///platform/projectModel-api/src/com/intellij/openapi/project/ProjectLocator.java)来查找包含给定虚拟文件的项目。

## How do I extend VFS?——如何扩展VFS?

To provide an alternative file system implementation (for example, an FTP file system), implement the [`VirtualFileSystem`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFileSystem.java) class (most likely you'll also need to implement `VirtualFile`), and register your implementation as an [application component](/basics/plugin_structure/plugin_components.md).

**CN:**  要提供另一种文件系统实现(例如，FTP文件系统)，请实现[`VirtualFileSystem`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFileSystem.java)类(很可能还需要实现`VirtualFile`)，并将实现注册为[application component(应用程序组件)](/basics/plugin_structure/plugin_components.md)。

To hook into operations performed in the local file system (for example, if you are developing a version control system integration that needs custom rename/move handling), implement the [`LocalFileOperationsHandler`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/LocalFileOperationsHandler.java) interface and register it through the`LocalFileSystem.registerAuxiliaryFileOperationsHandler` method.

**CN:**  要挂接到本地文件系统中执行的操作(例如，如果您正在开发需要自定义重命名/移动处理的版本控制系统集成)，请实现[`LocalFileOperationsHandler`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/LocalFileOperationsHandler.java)接口，并通过`LocalFileSystem.registerAuxiliaryFileOperationsHandler`方法注册它。

## What are the rules for working with VFS?——使用VFS的规则是什么?

See [IntelliJ Platform Virtual File System](/basics/virtual_file_system.md) for a detailed description of the VFS architecture and usage guidelines.

**CN:**  有关VFS体系结构和使用指南的详细描述，请参见[IntelliJ Platform Virtual File System](/basics/virtual_file_system.md)。