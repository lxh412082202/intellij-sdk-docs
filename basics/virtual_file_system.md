---
title: Virtual File System 虚拟文件系统
---

The virtual file system (VFS) is a component of *IntelliJ Platform* that encapsulates most of its activity for working with files. It serves the following main purposes:

**CN:**  虚拟文件系统(VFS)是IntelliJ平台的一个组件，它封装了大部分处理文件的活动。它的主要用途如下:

* Providing a universal API for working with files regardless of their actual location (on disk, in archive, on a HTTP server etc.)

**CN:**  提供一个通用的API来处理文件，不管它们的实际位置如何(在磁盘上、归档文件中、HTTP服务器上等等)

* Tracking file modifications and providing both old and new versions of the file content when a modification is detected.

**CN:**  跟踪文件修改，并在检测到修改时提供文件内容的新旧版本。

* Providing a possibility to associate additional persistent data with a file in the VFS.

**CN:**  提供了将其他持久数据与VFS中的文件关联的可能性。

In order to provide the last two features, the VFS manages a _persistent snapshot_ of some of the contents of the user's hard disk. The snapshot stores only those files which have been requested at least once through the VFS API, and is asynchronously updated to match the changes happening on the disk.

**CN:**  为了提供最后两个特性，VFS管理用户硬盘中某些内容的持久快照(_persistent snapshot_)。快照只存储那些至少通过VFS API请求过一次的文件，并进行异步更新以匹配磁盘上发生的更改。

The snapshot is application level, not project level - so, if some file (for example, a class in the JDK) is referenced by multiple projects, only one copy of its contents will be stored in the VFS.

**CN:**  快照是应用程序级别的，而不是项目级别的——因此，如果某个文件(例如JDK中的一个类)被多个项目引用，那么VFS中只会存储其内容的一个副本。

All VFS access operations go through the snapshot. 

**CN:**  所有VFS访问操作都经过快照。

If some information is requested through the VFS APIs and is not available in the snapshot, it is loaded from disk and stored into the snapshot. If the information is available in the snapshot, the snapshot data is returned. The contents of files and the lists of files in directories are stored in the snapshot only if that specific information was accessed - otherwise, only file metadata like name, length, timestamp, attributes is stored.

**CN:**  如果某些信息是通过VFS api请求的，并且在快照中不可用，那么它将从磁盘加载并存储到快照中。如果快照中有可用的信息，则返回快照数据。只有在访问特定信息时，文件的内容和目录中的文件列表才存储在快照中——否则，只存储诸如名称、长度、时间戳和属性等文件元数据。

> **Note** This means that the state of the file system and the file contents displayed in the IntelliJ Platform UI comes from the snapshot, which may not always match the actual contents of the disk. 
>
>**CN:**  这意味着文件系统的状态和IntelliJ平台UI中显示的文件内容来自快照，而快照可能并不总是与磁盘的实际内容相匹配。
>
> For example, in some cases deleted files can still be visible in the UI for some time before the deletion is picked up by the IntelliJ Platform.
>
>**CN:**  例如，在某些情况下，被删除的文件在被IntelliJ平台拾取之前，仍然可以在UI中看到一段时间。

The snapshot is updated from disk during _refresh operations_, which generally happen asynchronously. All write operations made through the VFS are synchronous - i.e. the contents is saved to disk immediately.

**CN:**  快照是在刷新操作期间从磁盘更新的，这些操作通常是异步进行的。所有通过VFS进行的写操作都是同步的——即内容被立即保存到磁盘。

A refresh operation synchronizes the state of a part of the VFS with the actual disk contents. Refresh operations are explicitly invoked by the *IntelliJ Platform* or plugin code - i.e. when a file is changed on disk while the IDE is running, the change will not be immediately picked up by the VFS. The VFS will be updated during the next refresh operation which includes the file in its scope.

**CN:**  刷新操作将VFS的一部分状态与实际的磁盘内容同步。刷新操作是由IntelliJ平台或插件代码显式地调用的——也就是说，当一个文件在IDE运行时在磁盘上发生了改变，VFS不会立即接受这个改变。VFS将在下一次刷新操作期间更新，该操作将文件包含在其范围内。

*IntelliJ Platform* refreshes the entire project contents asynchronously on startup. By default, it performs a refresh operation when the user switches to it from another app, but users can turn this off via **Settings \| Appearance & Behavior \| System Settings \| Synchronize files on frame activation**.

**CN:**  IntelliJ平台在启动时异步刷新整个项目内容。默认情况下，当用户从另一个应用切换到它时，它会执行刷新操作，但用户可以通过**Settings \| Appearance & Behavior \| System Settings \| Synchronize files on frame activation**进行手动刷新。

On Windows, Mac and Linux, *IntelliJ Platform* starts a native file watcher process that receives file change notifications from the file system and reports them to *IntelliJ Platform*. If a file watcher is available, a refresh operation looks only at the files that have been reported as changed by the file watcher. If no file watcher is present, a refresh operation walks through all directories and files in the refresh scope.

**CN:**  在Windows、Mac和Linux上，IntelliJ平台启动一个本地文件监视器进程，接收来自文件系统的文件更改通知，并将其报告给IntelliJ平台。如果文件监视程序可用，则刷新操作仅查看被文件监视程序报告为已更改的文件。如果没有文件监视器，则刷新操作将遍历刷新范围内的所有目录和文件。

Refresh operations are based on file timestamps. If the contents of a file was changed but its timestamp remained the same, *IntelliJ Platform* will not pick up the updated contents.

**CN:**  刷新操作基于文件时间戳。如果文件的内容改变了，但它的时间戳保持不变，IntelliJ平台将不会获取更新的内容。

There is currently no facility for removing files from the snapshot. If a file was loaded there once, it remains there forever unless it was deleted from the disk and a refresh operation was called on one of its parent directories.

**CN:**  目前还没有从快照中删除文件的工具。如果一个文件只加载了一次，那么它将永远保留在那里，除非它被从磁盘中删除，并且在它的父目录中调用了一个刷新操作。

The VFS itself does not honor ignored files listed in **Settings \| File Types \| Files** and folders to ignore and excluded folders listed in **Project Structure \| Modules \| Sources \| Excluded**. If the application code accesses them, the VFS will load and return their contents. In most cases, the ignored files and excluded folders must be skipped from processing by higher level code.

**CN:**  VFS本身并不支持**Settings \| File Types \| Files**中列出的被忽略的文件，也不支持**Project Structure \| Modules \| Sources \| Excluded**中列出的被忽略和被排除的文件夹。如果应用程序代码访问它们，VFS将加载并返回它们的内容。在大多数情况下，高级代码必须跳过被忽略的文件和被排除的文件夹。

During the lifetime of a running instance of an IntelliJ Platform IDE, multiple `VirtualFile` instances may correspond to the same disk file. They are equal, have the same `hashCode` and share the user data.

**CN:**  在IntelliJ平台IDE的一个运行实例的生命周期内，多个`VirtualFile`实例可能对应于同一个磁盘文件。它们是相等的，具有相同的`hashCode`并共享用户数据。

## Synchronous and asynchronous refreshes 同步和异步刷新

From the point of view of the caller, refresh operations can be either synchronous or asynchronous. In fact, the refresh operations are executed according to their own threading policy, and the synchronous flag simply means that the calling thread will be blocked until the refresh operation (which will most likely run on a different thread) is completed.

**CN:**  从调用者的角度来看，刷新操作可以是同步的，也可以是异步的。实际上，刷新操作是根据它们自己的线程策略执行的，而同步标志仅仅意味着调用线程将被阻塞，直到刷新操作(很可能在另一个线程上运行)完成。

Both synchronous and asynchronous refreshes can be initiated from any thread. If a refresh is initiated from a background thread, the calling thread must not hold a read action, because otherwise a deadlock would occur. See [IntelliJ Platform Architectural Overview](/basics/architectural_overview/general_threading_rules.md) for more details on the threading model and read/write actions.

**CN:**  同步和异步刷新都可以从任何线程启动。如果从后台线程发起刷新，则调用线程不能持有读操作，因为否则会发生死锁。有关线程模型和读/写操作的更多细节，请参见[IntelliJ Platform Architectural Overview](/basics/architectural_overview/general_threading_rules.md)。

The same threading requirements also apply to functions like [`LocalFileSystem.refreshAndFindFileByPath()`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/LocalFileSystem.java), which perform a partial refresh if the file with the specified path is not found in the snapshot.

**CN:**  同样的线程需求也适用于[`LocalFileSystem.refreshAndFindFileByPath()`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/LocalFileSystem.java)之类的函数，如果在快照中没有找到具有指定路径的文件，这些函数将执行部分刷新。

In nearly all cases, using asynchronous refreshes is strongly preferred. If there is some code that needs to be executed after the refresh is complete, the code should be passed as a `postRunnable` parameter to one of the refresh methods:

**CN:**  在几乎所有情况下，强烈建议使用异步刷新。如果有一些代码需要在刷新完成后执行，那么应该将这些代码作为一个`postRunnable`参数传递给某个刷新方法:
 
* [`RefreshQueue.createSession()`](upsource:///platform/analysis-api/src/com/intellij/openapi/vfs/newvfs/RefreshQueue.java)<!--#L36-->
* [`VirtualFile.refresh()`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFile.java)<!--#L681-->
 
Synchronous refreshes can cause deadlocks in some cases, depending on which locks are held by the thread invoking the refresh operation.

**CN:**  同步刷新在某些情况下可能导致死锁，这取决于调用刷新操作的线程持有哪些锁。

## Virtual file system events——虚拟文件系统事件

All changes happening in the virtual file system, either as a result of refresh operations or caused by user's actions, are reported as _virtual file system events_. VFS events are always fired in the event dispatch thread, and in a write action.

**CN:**  在虚拟文件系统中发生的所有更改，无论是由于刷新操作还是由于用户的操作，都被报告为虚拟文件系统事件。VFS事件总是在事件调度线程和写操作中触发。

The most efficient way to listen to VFS events is to implement the `BulkFileListener` interface and to subscribe with it to the [`VirtualFileManager.VFS_CHANGES`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFileManager.java)<!--#L34--> topic.
A non-blocking variant [`AsyncFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/AsyncFileListener.java) is also available in 2019.2 or later.
See [How do I get notified when VFS changes?](/basics/architectural_overview/virtual_file.md#how-do-i-get-notified-when-vfs-changes) for implementation details.

**CN:**  监听VFS事件的最有效方法是实现`BulkFileListener`接口并将其订阅到[`VirtualFileManager.VFS_CHANGES`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFileManager.java)<!--#L34-->主题。
非阻塞的[`AsyncFileListener`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/AsyncFileListener.java)在2019.2或更高版本中也可用。实现细节见[How do I get notified when VFS changes?(当VFS发生变化时，我如何得到通知?)](/basics/architectural_overview/virtual_file.md#how-do-i-get-notified-when-vfs-changes)。

> **WARNING** VFS listeners are application level and will receive events for changes happening in *all* the projects opened by the user. You may need to filter out events that aren't relevant to your task (e.g., via [`ProjectFileIndex#isInContent()`](upsource:///platform/projectModel-api/src/com/intellij/openapi/roots/ProjectFileIndex.java)).
>
>**CN:**  警告：VFS监听器是应用程序级别的，它将接收用户打开的所有项目中发生的更改事件。您可能需要过滤掉与任务无关的事件(例如，[`ProjectFileIndex#isInContent()`](upsource:///platform/projectModel-api/src/com/intellij/openapi/roots/ProjectFileIndex.java))。

VFS events are sent both before and after each change, and you can access the old contents of the file in the before event. Note that events caused by a refresh are sent after the changes have already occurred on disk - so when you process the `beforeFileDeletion` event, for example, the file has already been deleted from disk. However, it is still present in the VFS snapshot, and you can access its last contents using the VFS API.

**CN:**  每次更改之前和之后都会发送VFS事件，您可以在before事件中访问文件的旧内容。请注意，由刷新引起的事件是在磁盘上已经发生了更改之后发送的——因此，当您处理`beforeFileDeletion`事件时，例如，文件已经从磁盘上删除了。但是，它仍然存在于VFS快照中，您可以使用VFS API访问它的最后内容。

Note that a refresh operation fires events only for changes in files that have been loaded in the snapshot. For example, if you accessed a `VirtualFile` for a directory but never loaded its contents using [`VirtualFile.getChildren()`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFile.java)<!--#L135-->, you may not get `fileCreated` notifications when files are created in that directory.

**CN:**  请注意，刷新操作仅对已在快照中加载的文件中的更改触发事件。例如，如果您为某个目录访问了一个`VirtualFile`，但从未使用[`VirtualFile.getChildren()`](upsource:///platform/core-api/src/com/intellij/openapi/vfs/VirtualFile.java)<!--#L135-->加载它的内容，那么在该目录中创建文件时，您可能不会收到`fileCreated`通知。

If you loaded only a single file in a directory using `VirtualFile.findChild()`, you will get notifications for changes to that file, but you may not get created/deleted notifications for other files in the same directory.

**CN:**  如果您仅使用`VirtualFile.findChild()`加载目录中的一个文件，您将获得该文件更改的通知，但您可能无法为同一目录中的其他文件创建/删除通知。
