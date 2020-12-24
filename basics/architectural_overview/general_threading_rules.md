---
title: General Threading Rules——通用线程规则
---

## Read/write lock——读/写锁

In general, code-related data structures in the *IntelliJ Platform* are covered by a single reader/writer lock. This applies to PSI, VFS and project root model.

**CN:**  通常，IntelliJ平台中与代码相关的数据结构是由一个读/写锁覆盖的。这适用于PSI、VFS和项目根模型。

Reading data is allowed from any thread.  Reading data from the UI thread does not require any special effort. However, read operations performed from any other thread need to be wrapped in a read action by using `ApplicationManager.getApplication().runReadAction()` or, shorter, `ReadAction.run/compute`.

**CN:**  允许从任何线程读取数据。从UI线程读取数据不需要任何特殊的工作。但是，从任何其他线程执行的读操作都需要使用`ApplicationManager.getApplication().runReadAction()`或更短的`ReadAction.run/compute`包装在一个读操作中

Writing data is only allowed from the UI thread, and write operations always need to be wrapped in a write action with `ApplicationManager.getApplication().runWriteAction()` or, shorter, `WriteAction.run()/compute()`.

**CN:**  只允许从UI线程中写入数据，并且写入操作始终需要包装在具有`ApplicationManager.getApplication().runWriteAction()`或更短的`WriteAction.run()/compute()`的写入操作中

In addition, modifying the model is only allowed from write-safe contexts, which include user actions and `invokeLater()` calls from them (see the next section). You may not modify PSI, VFS or project model from inside UI renderers or `SwingUtilities.invokeLater()` calls.

**CN:**  此外，只允许从写安全上下文中修改模型，其中包括用户操作和来自用户操作的invokeLater()调用(参见下一节)。你不能从UI渲染器或SwingUtilities.invokeLater() 调用中修改PSI, VFS或项目模型。

You must not access the model outside a read or write action. The corresponding objects are not guaranteed to survive between several consecutive read actions. So as a rule of thumb, whenever you start a read action, you should check if your PSI/VFS/project/module are still valid.

**CN:**  您不能在读写操作之外访问模型。相应的对象不能保证在多个连续读操作之间存活。因此，根据经验，无论何时开始读操作，都应该检查PSI/VFS/project/module是否仍然有效。

## `invokeLater`

To pass control from a background thread to the event dispatch thread, instead of the standard `SwingUtilities.invokeLater()`, plugins should use `ApplicationManager.getApplication().invokeLater()`. The latter API allows specifying the _modality state_ for the call, i.e. the stack of modal dialogs under which the call is allowed to execute. 

**CN:**  要将控制从后台线程传递给事件调度线程，而不是标准的`SwingUtilities.invokeLater()`，插件应该使用`ApplicationManager.getApplication().invokeLater()`。后一个API允许指定调用的模态状态，即允许执行调用的模态对话框的堆栈。

* Passing `ModalityState.NON_MODAL` means that the operation will be executed after all modal dialogs are closed. Note that if any of the open (unrelated) project displays a per-project modal dialog, the action will be executed after the dialog is closed. 

**CN:**  通过`ModalityState.NON_MODAL`意味着该操作将在所有模式对话框关闭后执行。请注意，如果打开的(不相关的)项目中的任何一个显示每个项目的模式对话框，则将在对话框关闭后执行操作。

* Passing `ModalityState.stateForComponent()` means that the operation can be executed when the topmost shown dialog is the one that contains the specified component, or is one of its parent dialogs. 

**CN:**  传递`ModalityState.stateForComponent()`意味着当最上面显示的对话框是包含指定组件的对话框，或者是它的父对话框之一时，可以执行操作。

* If no modality state is passed, `ModalityState.defaultModalityState()` will be used. In most cases, this is the optimal choice, that uses current modality state when invoked from UI thread, and has a special handling for background processes started with `ProgressManager`: `invokeLater()` from such a process may run in the same dialog that the process started.

**CN:**  如果没有传递通道状态，则使用`ModalityState.defaultModalityState()`。在大多数情况下，这是最佳选择，即在从UI线程调用时使用当前模式状态，并对使用`ProgressManager`: `invokeLater()`启动的后台进程进行特殊处理，这样的进程可以在与启动进程相同的对话框中运行。

* `ModalityState.any()` means that the runnable will be executed as soon as possible regardless of modal dialogs. Please note that modifying PSI, VFS or project model is prohibited from such runnables.

**CN:**  `ModalityState.any()`意味着无论模态对话框是什么，runnable都会尽快执行。请注意，禁止修改PSI、VFS或项目模型。

If your UI thread activity needs to access [file-based index](../indexing_and_psi_stubs.md) (e.g. it's doing any kind of project-wide PSI analysis, resolves references, etc), please use `DumbService.smartInvokeLater()`. That way, your activity will be run after all possible indexing processes have been completed.

**CN:**  如果你的UI线程活动需要访问[file-based index基于文件的索引](../indexing_and_psi_stubs.md)(例如，它正在做任何类型的项目范围的PSI分析，解析引用，等等)，请使用`DumbService.smartInvokeLater()`。

## Background processes and `ProcessCanceledException`——后台进程和`ProcessCanceledException`

Background progresses are managed by [`ProgressManager`](upsource:///platform/core-api/src/com/intellij/openapi/progress/ProgressManager.java) class, which has plenty of methods to execute the given code
with a modal (dialog), non-modal (visible in the status bar) or invisible progress. In all cases, the code is
executed on a background thread which is associated with a [`ProgressIndicator`](upsource:///platform/core-api/src/com/intellij/openapi/progress/ProgressIndicator.java) object.
The current thread's indicator can be retrieved any time via `ProgressIndicatorProvider.getGlobalProgressIndicator()`.

**CN:**  后台进程由[`ProgressManager`](upsource:///platform/core-api/src/com/intellij/openapi/progress/ProgressManager.java)类管理，它有大量的方法来执行给定的代码，有模态(对话框)、非模态(在状态栏中可见)或不可见的进程。在所有情况下，代码都是在与[`ProgressIndicator`](upsource:///platform/core-api/src/com/intellij/openapi/progress/ProgressIndicator.java)对象相关联的后台线程上执行的。可以通过`ProgressIndicatorProvider.getGlobalProgressIndicator()`随时检索当前线程的指示器。

For visible progresses, threads can use `ProgressIndicator` to notify the user about current status:
e.g. set text or visible fraction of the work done.

**CN:**  对于可见的进程，线程可以使用`ProgressIndicator`来通知用户当前的状态:例如，设置文本或已完成工作的可见部分。

Progress indicators also provide means to handle cancellation of background processes, either by user (pressing "Cancel" button),
or from code (e.g. when the current operation becomes obsolete due to some changes in the project).
The progress can be marked as canceled by calling `ProgressIndicator.cancel()`.
The process reacts to this by calling `ProgressIndicator.checkCanceled()` (or `ProgressManager.checkCanceled()` if you don't have indicator instance at hand).
This call throws a special unchecked [`ProcessCanceledException`](upsource:///platform/util/src/com/intellij/openapi/progress/ProcessCanceledException.java) if the background process has been canceled.

**CN:**  进度指示器还提供了处理后台进程取消的方法，可以通过用户(按“取消”按钮)，也可以通过代码(例如，当当前操作由于项目中的某些更改而变得过时时)来处理。通过调用`ProgressIndicator.cancel()`可以将进度标记为已取消。该进程通过调用`ProgressIndicator.checkCanceled()`(或`ProgressManager.checkCanceled()`，如果您手头没有指示符实例)来对此作出反应。如果后台进程已被取消，此调用将抛出一个特殊的未检查的[`ProcessCanceledException`](upsource:///platform/util/src/com/intellij/openapi/progress/ProcessCanceledException.java)。

All code working with PSI, or in other kinds of background processes, should be prepared to a [`ProcessCanceledException`](upsource:///platform/util/src/com/intellij/openapi/progress/ProcessCanceledException.java) being thrown from any point.
This exception should never be logged, it should be rethrown, and it'll be handled in the infrastructure that started the process.

**CN:**  所有使用PSI或其他类型后台进程的代码都应该准备好随时抛出[`ProcessCanceledException`](upsource:///platform/util/src/com/intellij/openapi/progress/ProcessCanceledException.java)。不应该记录这个异常，应该重新抛出它，它将在启动该进程的基础设施中处理。

The `checkCanceled()` should be called often enough to guarantee smooth cancellation of the process. PSI internals
have a lot of `checkCanceled()` calls inside. But if your process does lengthy non-PSI activity, you might need to
insert explicit `checkCanceled()` calls so that it happens frequently, e.g. on each _Nth_ loop iteration.

**CN:**  应该经常调用`checkCanceled()`，以确保流程的顺利取消。PSI内部有很多`checkCanceled()`调用。但是，如果您的进程执行冗长的非psi活动，您可能需要插入显式的`checkCanceled()`调用，以使其频繁发生，例如在每个Nth循环迭代上。

## Read action cancellability——

Background threads shouldn't take plain read actions for a long time. The reason is that if the UI thread needs a write action (e.g. the user types something), it must be acquired as soon as possible, otherwise the UI will freeze until all background threads have released their read actions.

**CN:**  

The best known approach to that is to cancel background read actions whenever there's a write action about to occur, and restart that background read action later from the scratch. Editor highlighting, code completion, Goto Class/File/etc actions all work like this.
To achieve that, the lengthy background operation is started with a `ProgressIndicator`, and a special listener
cancels that indicator when write action is started.
The next time the background thread calls `checkCanceled()`, a `ProcessCanceledException` will be thrown,
and the thread should stop its operation (and finish the read action) as soon as possible. 
 
**CN:**  
 
There are two recommended ways of doing this:

**CN:**  

* If you're on UI thread, call `ReadAction.nonBlocking()` which returns [`NonBlockingReadAction`](upsource:///platform/core-api/src/com/intellij/openapi/application/NonBlockingReadAction.java)

**CN:**  

* If you're already in a background thread, use `ProgressManager.getInstance().runInReadActionWithWriteActionPriority()` in a loop, until it passes or the whole activity becomes obsolete.

**CN:**  

In both approaches, you should always check at the start of each read action, if the objects you're working with are still valid, and if the whole operation still makes sense (i.e. not canceled by the user, the project isn't closed, etc.). With `ReadAction.nonBlocking()`,
use `expireWith()` or `expireWhen()` for that.

**CN:**  

If the activity you're doing has to access [file-based index](../indexing_and_psi_stubs.md) (e.g. it's doing any kind of project-wide PSI analysis, resolves references, etc), use `ReadAction.nonBlocking(...).inSmartMode()`.

**CN:**  
