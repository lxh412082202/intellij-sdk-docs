---
title: Messaging infrastructure 消息传递基础结构
---


# Purpose 目的

The purpose of this document is to introduce the messaging infrastructure available in the IntelliJ Platform to developers and plugin writers. It is intended to answer why, when and how to use it.

**CN:**  本文档的目的是向开发人员和插件作者介绍IntelliJ平台中可用的消息传递基础结构。它的目的是回答为什么、何时以及如何使用它。

# Rationale 基本原理

So, what is messaging in the IntelliJ Platform and why do we need it? Basically, its implementation of
[Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern)
that provides additional features like _broadcasting on hierarchy_ and special _nested events_ processing (_nested event_ here is a situation when new event is fired (directly or indirectly) from the callback of another event).

**CN:**  那么，IntelliJ平台中的消息传递是什么?我们为什么需要它?
基本上，它的
[Observer pattern(观察者模式)](https://en.wikipedia.org/wiki/Observer_pattern)
的实现提供了额外的特性，比如在层次结构上广播和特殊的嵌套事件处理(嵌套事件在这里是一种情况，当新的事件被触发(直接或间接)从另一个事件的回调)。

# Design 设计

Here are the main components of the messaging API. 

**CN:**  下面是消息传递API的主要组件。

## Topic 主题

This class serves as an endpoint at the messaging infrastructure. I.e. clients are allowed to subscribe to the topic within particular bus and to send messages to particular topic within particular bus.

**CN:**  该类充当消息传递基础设施的端点。例如，客户可以订阅特定总线内的主题，并向特定总线内的特定主题发送消息。

![Topic](img/topic.png)

*  *display name*  just a human-readable name used for logging/monitoring purposes;
*  **CN:**  *display name*  只是一个人类可读的名称，用于日志/监视目的;
*  *broadcast direction*  will be explained in details at Broadcasting. Default value is *TO\_CHILDREN*;
*  **CN:**  *broadcast direction* 将在Broadcasting中详细解释。默认值为 *TO\_CHILDREN*；
*  *listener class*  that is a business interface for particular topic.
Subscribers register implementation of this interface at the messaging infrastructure and publishers may later retrieve object that conforms (IS-A) to it and call any method defined there. Messaging infrastructure takes care on dispatching that to all subscribers of the topic, i.e. the same method with the same arguments will be called on the registered callbacks;
*  **CN:**  *listener class*  这是针对特定topic的业务接口。
订阅者在消息传递基础结构上注册此接口的实现，发布者稍后可以检索符合该接口的对象并调用其中定义的任何方法。消息传递基础设施负责将其分派给主题的所有订阅者，即在已注册的回调中调用具有相同参数的相同方法;
## Message bus——消息总线

Is the core of the messaging system. Is used at the following scenarios:

**CN:**  是消息传递系统的核心。用于以下场景:

![Bus](img/bus.png)

## Connection——连接

Manages all subscriptions for particular client within particular bus.

**CN:**  管理特定总线中特定客户端的所有订阅。

![Connection](img/connection.png)

*  keeps number of *topic handler* mappings (callbacks to invoke when message for the target topic is received)
*Note*: not more than one handler per-topic within the same connection is allowed;

* **CN:**  保持*topic handler*映射的数量(当接收到目标主题的消息时调用的回调)，*Note*:在同一个连接中每个主题不允许超过一个处理程序;

*  it's possible to specify *default handler* and subscribe to the target topic without explicitly provided callback.
Connection will use that *default handler* when storing *(topic-handler)* mapping;

* **CN:**  可以在不显式提供回调的情况下指定*default handler*并订阅目标主题。
           Connection在存储 *(topic-handler)* 映射时将使用该 *default handler*;

*  it's possible to explicitly release acquired resources (*disconnect()* method).
Also it can be plugged to standard semi-automatic disposing 
(
[`Disposable`](upsource:///platform/util/src/com/intellij/openapi/Disposable.java)
);

* **CN:**  可以显式地释放获得的资源( *disconnect()* 方法)。
           可插接标准半自动处置([`Disposable`](upsource:///platform/util/src/com/intellij/openapi/Disposable.java));

## Putting altogether——完全

*Defining business interface and topic*

**CN:**  定义业务接口和主题

```java
public interface ChangeActionNotifier {

    Topic<ChangeActionNotifier> CHANGE_ACTION_TOPIC = Topic.create("custom name", ChangeActionNotifier.class)

    void beforeAction(Context context);
    void afterAction(Context context);
}
```

*Subscribing*——订阅

![Subscribing](img/subscribe.png)

> **NOTE** If targeting 2019.3 or later, use [declarative registration](/basics/plugin_structure/plugin_listeners.md) if possible.

> **CN:**  **NOTE** 如果目标是2019.3或更高，尽可能使用[declarative registration](/basics/plugin_structure/plugin_listeners.md)。

```java
public void init(MessageBus bus) {
    bus.connect().subscribe(ActionTopics.CHANGE_ACTION_TOPIC, new ChangeActionNotifier() {
        @Override
        public void beforeAction(Context context) {
            // Process 'before action' event.
        }
        @Override
        public void afterAction(Context context) {
            // Process 'after action' event.
        }
    });
}
```

*Publishing*——发布

![Publishing](img/publish.png)

```java
public void doChange(Context context) {
    ChangeActionNotifier publisher = myBus.syncPublisher(ActionTopics.CHANGE_ACTION_TOPIC);
    publisher.beforeAction(context);
    try {
        // Do action
        // ...
    } finally {
        publisher.afterAction(context)
    }
}
```

*Existing resources*——现有资源

*  *MessageBus* instances are available via
[`ComponentManager.getMessageBus()`](upsource:///platform/extensions/src/com/intellij/openapi/components/ComponentManager.java)<!--#L85-->
(many standard interfaces implement it, e.g.
[`Application`](upsource:///platform/core-api/src/com/intellij/openapi/application/Application.java),
[`Project`](upsource:///platform/core-api/src/com/intellij/openapi/project/Project.java);

*  **CN:**  *MessageBus* 实例通过以下方式可用
                         [`ComponentManager.getMessageBus()`](upsource:///platform/extensions/src/com/intellij/openapi/components/ComponentManager.java) <!--#L85-->
                         (许多标准接口实现了它，例如。
                         [`Application`](upsource:///platform/core-api/src/com/intellij/openapi/application/Application.java)、
                         [`Project`](upsource:///platform/core-api/src/com/intellij/openapi/project/Project.java);

*  number of public topics are used by the *IntelliJ Platform*, e.g.
[`AppTopics`](upsource:///platform/platform-api/src/com/intellij/AppTopics.java),
[`ProjectTopics`](upsource:///platform/projectModel-api/src/com/intellij/ProjectTopics.java)
etc.
So, it's possible to subscribe to them in order to receive information about the processing;

*  **CN:**  公共topics的数量被 *IntelliJ Platform* 使用，例如。
            [`AppTopics`](upsource:///platform/platform-api/src/com/intellij/AppTopics.java)、
            [`ProjectTopics`](upsource:///platform/projectModel-api/src/com/intellij/ProjectTopics.java)等等。
            所以，我们可以订阅它们来接收关于处理过程的信息;

# Broadcasting——广播

Message buses can be organised into hierarchies. Moreover, the *IntelliJ Platform* has them already:

**CN:**  消息总线可以组织成层次结构。此外， *IntelliJ Platform* 已经具备了:

![Standard hierarchy](img/standard-hierarchy.png)

That allows to notify subscribers registered in one message bus on messages sent to another message bus.

**CN:**  允许在发送到另一个消息总线的消息上通知在一个消息总线中注册的订阅者。

*Example:*——例子：

![Parent-child broadcast](img/parent-child-broadcast.png)

Here we have a simple hierarchy (*application bus* is a parent of *project bus*) with three subscribers for the same topic.

We get the following if *topic1* defines broadcast direction as *TO\_CHILDREN*:
1.  A message is sent to *topic1* via *application bus*;
2.  *handler1* is notified about the message;
3.  The message is delivered to the subscribers of the same topic within *project bus* (*handler2* and *handler3*);

*Benefits*

We don't need to bother with memory management of subscribers that are bound to child buses but interested in parent bus-level events.

Consider the example above we may want to have project-specific functionality that reacts to the application-level events. 
All we need to do is to subscribe to the target topic within the *project bus*.
No hard reference to the project-level subscriber will be stored at application-level then, 
i.e. we just avoided memory leak on project re-opening.

*Options*

Broadcast configuration is defined per-topic. Following options are available:

*  _TO\_CHILDREN_ (default);

*  _NONE_;

*  _TO\_PARENT_;

# Nested messages

_Nested message_ is a message sent (directly or indirectly) during another message processing.
The IntelliJ Platform's Messaging infrastructure guarantees that all messages sent to particular topic will be delivered at the sending order.

*Example:*

Suppose we have the following configuration:

![Nested messages](img/nested-config.png)

Let's see what happens if someone sends a message to the target topic:

*  _message1_ is sent;

*  _handler1_ receives _message1_ and sends _message2_ to the same topic;

*  _handler2_ receives _message1_;

*  _handler2_ receives _message2_;

*  _handler1_ receives _message2_;

# Tips'n'tricks

## Relief listeners management

Messaging infrastructure is very light-weight, so, it's possible to reuse it at local sub-systems in order to relief
[Observers](https://en.wikipedia.org/wiki/Observer_pattern) construction. Let's see what is necessary to do then:

1. Define business interface to work with;

2. Create shared message bus and topic that uses the interface above (_shared_ here means that either _subject_ or _observers_ know about them);

Let's compare that with a manual implementation:

1. Define listener interface (business interface);

2. Provide reference to the _subject_ to all interested listeners;

3. Add listeners storage and listeners management methods (add/remove) to the _subject_;

4. Manually iterate all listeners and call target callback in all places where new event is fired;

## Avoid shared data modification from subscribers

We had a problem in a situation when two subscribers tried to modify the same document
([IDEA-71701](https://youtrack.jetbrains.com/issue/IDEA-71701)).

The thing is that every document change is performed by the following scenario:

1. _before change_ event is sent to all document listeners and some of them publish new messages during that;

2.  actual change is performed;

3.  _after change_ event is sent to all document listeners;

We had the following then:

1.  _message1_ is sent to the topic with two subscribers;
2.  _message1_ is queued for both subscribers;
3.  _message1_ delivery starts;
4.  _subscriber1_ receives _message1_;
5.  _subscriber1_ issues document modification request at particular range (e.g. _document.delete(startOffset, endOffset)_);
6.  _before change_ notification is sent to the document listeners;
7.  _message2_ is sent by one of the standard document listeners to another topic within the same message bus during _before change_ processing;
8.  the bus tries to deliver all pending messages before queuing _message2_;
9.  _subscriber2_ receives _message1_ and also modifies a document;
10.  the call stack is unwinded and _actual change_ phase of document modification operation requested by _subscriber1_ begins;

**The problem**  is that document range used by _subscriber1_ for initial modification request is invalid if _subscriber2_ has changed document's range before it.


