---
title: Documents——文档
---

A document is an editable sequence of Unicode characters, which typically corresponds to the text contents of a virtual file. Line breaks in a document are _always_ normalized to `\n`. The *IntelliJ Platform* handles encoding and line break conversions when loading and saving documents transparently.

**CN:**  文档是一个可编辑的Unicode字符序列，它通常对应于虚拟文件的文本内容。文档中的换行符通常被规范化为`\n`。IntelliJ平台在透明地加载和保存文档时处理编码和换行转换。

## How do I get a document?——怎么获取文档？

* From an action: `e.getData(PlatformDataKeys.EDITOR).getDocument()`

**CN:**  从一个操作：`e.getData(PlatformDataKeys.EDITOR).getDocument()`

* From a virtual file: `FileDocumentManager.getDocument()`. This call forces the document content to be loaded from disk if it wasn't loaded previously; if you're only interested in open documents or documents which may have been modified, use `FileDocumentManager.getCachedDocument()` instead.

**CN:**  从虚拟文件：`FileDocumentManager.getDocument()`。这个调用强制从磁盘加载文档内容(如果以前没有加载的话);如果您只对打开的文档或可能已修改的文档感兴趣，请使用`FileDocumentManager.getCachedDocument()`代替。

* From a PSI file: `PsiDocumentManager.getInstance().getDocument()` or `PsiDocumentManager.getInstance().getCachedDocument()`

**CN:**  从PSI文件：`PsiDocumentManager.getInstance().getDocument()` 或 `PsiDocumentManager.getInstance().getCachedDocument()`

## What can I do with a Document?——文档可以用来做什么？

You may perform any operations that access or modify the file contents on "plain text" level (as a sequence of characters, not as a tree of Java elements).

**CN:**  您可以执行在“纯文本”级别上访问或修改文件内容的任何操作(作为字符序列，而不是作为Java元素树)。

## Where does a Document come from?——文档从何而来？

Document instances are created when some operation needs to access the text contents of a file (in particular, this is needed to build the PSI for a file). Also, document instances not linked to any virtual files can be created temporarily, for example, to represent the contents of a text editor field in a dialog.

**CN:**  文档实例是在某些操作需要访问文件的文本内容时创建的(特别是在为文件构建PSI时)。此外，可以临时创建未链接到任何虚拟文件的文档实例，例如，在对话框中表示文本编辑器字段的内容。

## How long does a Document persist?——文档持续多长时间?

Document instances are weakly referenced from `VirtualFile` instances. Thus, an unmodified [`Document`](upsource:///platform/core-api/src/com/intellij/openapi/editor/Document.java) instance can be garbage-collected if it isn't referenced by anyone, and a new instance will be created if the document contents is accessed again later. Storing `Document` references in long-term data structures of your plugin will cause memory leaks.

**CN:**  文档实例是从`VirtualFile`实例中弱引用的。因此，如果没有任何人引用未修改的[`Document`](upsource:///platform/core-api/src/com/intellij/openapi/editor/Document.java)实例，则可以对其进行垃圾收集，如果稍后再次访问文档内容，则会创建一个新实例。将`Document`引用存储在插件的长期数据结构中会导致内存泄漏。

## How do I create a Document?——怎么创建文档？

If you need to create a new file on disk, you don't create a `Document`: you create a PSI file and then get its `Document`. If you need to create a `Document` instance which isn't bound to anything, you can use `EditorFactory.createDocument`.

**CN:**  如果你需要在磁盘上创建一个新文件，你不需要创建一个`Document`:你创建一个PSI文件，然后得到它的`Document`。如果你需要创建一个不绑定任何东西的`Document`实例，你可以使用`EditorFactory.createDocument`。

## How do I get notified when Documents change?——当文档更改时，我如何得到通知?

* `Document.addDocumentListener` allows you to receive notifications about changes in a particular `Document` instance.

**CN:**  `Document.addDocumentListener`允许您接收关于特定`Document`实例中更改的通知。

* `EditorFactory.getEventMulticaster().addDocumentListener` allows you to receive notifications about changes in all open documents.

**CN:**  `EditorFactory.getEventMulticaster().addDocumentListener`允许您在所有打开的文档中接收关于更改的通知。

* Subscribe to `AppTopics#FILE_DOCUMENT_SYNC` on any level bus to receive notifications when any `Document` is saved or reloaded from disk.

**CN:**  订阅任何级别总线上的`AppTopics#FILE_DOCUMENT_SYNC`，以便在从磁盘保存或重新加载任何BBB时接收通知。

## What are the rules of working with Documents?——处理文档的规则是什么?

The general read/write action rules are in effect. In addition to that, any operations which modify the contents of the document must be wrapped in a command (`CommandProcessor.getInstance().executeCommand()`). `executeCommand()` calls can be nested, and the outermost `executeCommand` call is added to the undo stack. If multiple documents are modified within a command, undoing this command will by default show a confirmation dialog to the user.

**CN:**  一般的读/写操作规则是有效的。除此之外，任何修改文档内容的操作都必须包装在命令中(`CommandProcessor.getInstance().executeCommand()`)。可以嵌套`executeCommand()`调用，并将最外层的`executeCommand`调用添加到undo堆栈中。如果在一个命令中修改了多个文档，取消此命令将默认向用户显示一个确认对话框。

If the file corresponding to a `Document` is read-only (for example, not checked out from the version control system), document modifications will fail. Thus, before modifying the `Document`, it is necessary to call `ReadonlyStatusHandler.getInstance(project).ensureFilesWritable()` to check out the file if necessary.

**CN:**  如果`Document`对应的文件是只读的(例如，没有从版本控制系统签出)，则文档修改将失败。因此，在修改`Document`之前，如果需要，需要调用`ReadonlyStatusHandler.getInstance(project).ensureFilesWritable()`来检查文件。

All text strings passed to `Document` modification methods (`setText`, `insertString`, `replaceString`) must use only \n as line separators.

**CN:**  所有传递给`Document`修改方法(`setText`, `insertString`, `replaceString`)的文本字符串必须只使用 \n 作为行分隔符。

## Are there any utilities available for working with Documents?——是否有处理文档的工具可用?

[`DocumentUtil`](upsource:///platform/core-impl/src/com/intellij/util/DocumentUtil.java) contains utility methods for `Document` processing. This allows you to get information like the text offsets of particular lines. This is particularly useful when you need text location/offset information about a given PsiElement.

**CN:**  [`DocumentUtil`](upsource:///platform/core-impl/src/com/intellij/util/DocumentUtil.java)包含用于`Document`处理的实用方法。这允许您获取诸如特定行的文本偏移量之类的信息。当您需要关于给定PsiElement的文本位置/偏移量信息时，这尤其有用。