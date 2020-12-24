---
title: XML DOM API
---


<!-- TODO content: DOM <=> PSI, Go To Symbol, editor gutter icon->DOM -->

## Abstract——摘要

This article is intended for plugin writers who create custom web server integrations, or some UI for easy XML editing. It describes the *Document Object Model* (DOM) in IntelliJ Platform --- an easy way to work with DTD or Schema-based XML models.
The following topics will be covered: working with DOM itself (reading/writing tags content, attributes, and subtags) and easy XML editing in the UI by connecting UI to DOM.

**CN:**  这篇文章是为插件作者编写的，他们创建了定制的web服务器集成，或者一些用于轻松编辑XML的UI。
它描述了IntelliJ平台中的文档对象模型（DOM）——一种使用DTD或基于模式的XML模型的简单方法。
将讨论以下主题:使用DOM本身(读取/编写标记内容、属性和子标记)，以及通过将UI连接到DOM来轻松编辑UI中的XML。

It's assumed that the reader is familiar with Java, Swing, IntelliJ Platform XML PSI (classes [`XmlTag`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlTag.java), [`XmlFile`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlFile.java), [`XmlTagValue`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlTagValue.java), etc.), IntelliJ Platform plugin development basics (application and project components, file editors).

**CN:**  假设读者熟悉Java、Swing、IntelliJ平台XML PSI(类
[`XmlTag`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlTag.java), [`XmlFile`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlFile.java), [`XmlTagValue`](upsource:///xml/xml-psi-api/src/com/intellij/psi/xml/XmlTagValue.java)等)、IntelliJ平台插件开发基础(应用和项目组件、文件编辑器)。

## Introduction——介绍

So, how to operate with XML from an IntelliJ Platform plugin? Usually, one has to take `XmlFile`, get its root tag, and then find a required sub-tag by path. The path consists of tag names, each of them a string. Typing these everywhere is tedious and error-prone. Let's assume you have the following XML:

**CN:**  那么，如何从IntelliJ平台插件中操作XML呢?通常，必须使用`XmlFile`，获取它的根标记，然后按路径查找所需的子标记。该路径由标记名组成，每个标记名都是一个字符串。在任何地方输入这些都是冗长的，而且容易出错。假设您有以下XML:

```xml
<root>
    <foo>
        <bar>42</bar>
        <bar>239</bar>
    </foo>
</root>
```

Let's say you want to read the contents of the second bar element, namely, "239".

**CN:**  假设您想要读取第二个bar元素的内容，即“239”。

It's _not_ correct to create chained calls like

**CN:**  创建像这样的链接调用是不正确的

```java
file.getDocument().getRootTag().findFirstSubTag("foo").
findSubTags("bar")[1].getValue().getTrimmedText()
```
because each call here may return `null`.

**CN:**  因为这里的每个调用都可能返回`null`。

So the code would probably look like this: 

**CN:**  所以代码可能是这样的:
                
```java
XmlFile file = ...;
final XmlDocument document = file.getDocument();
if (document != null) {
    final XmlTag rootTag = document.getRootTag();
    if (rootTag != null) {
        final XmlTag foo = rootTag.findFirstSubTag("foo");
        if (foo != null) {
            final XmlTag[] bars = foo.findSubTags("bar");
            if (bars.length > 1) {
                String s = bars[1].getValue().getTrimmedText();
                // do something
            }
        }
    }
}
```

Looks awful, doesn't it? But there's a better way to do the same thing. You just need to extend a special interface --- [`DomElement`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomElement.java).

**CN:**  看起来很糟糕，不是吗?但是有一个更好的方法来做同样的事情。您只需要扩展一个特殊的接口 --- [`DomElement`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomElement.java)。

For example, let's create several interfaces:

**CN:**  例如，让我们创建几个接口:

```java
interface Root extends com.intellij.util.xml.DomElement {
    Foo getFoo();
}

interface Foo extends com.intellij.util.xml.DomElement {
    List<Bar> getBars();
}

interface Bar extends com.intellij.util.xml.DomElement {
    String getValue();
}
```

Next, you should create a [`DomFileDescription`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomFileDescription.java) object, pass to its constructor the root tag name and root element interface, and register it with extension point `com.intellij.dom.fileDescription`.

**CN:**  接下来，您应该创建一个[`DomFileDescription`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomFileDescription.java) 对象，将根标记名称和根元素接口传递给它的构造函数，并将其注册到扩展点`com.intellij.dom.fileDescription`。

> **NOTE** If your plugin targets 2019.1 or later, please use extension point `com.intellij.dom.fileMetaData` instead and specify `rootTagName` and `domVersion`/`stubVersion` in `plugin.xml`.
>
>**CN:**  注意：如果您的插件版本是2019.1或更高版本，那么请使用扩展点`com.intellij.dom.fileMetaData`，并在`plugin.xml`中指定`rootTagName`和`domVersion`/`stubVersion`。

You can now get the file element from [`DomManager`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomManager.java). To get the "239" value, you only have to write the following code:

**CN:**  现在可以从[`DomManager`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomManager.java)获取文件元素。要获得“239”的值，您只需编写以下代码:

```java
DomManager manager = DomManager.getDomManager(project);
Root root = manager.getFileElement(file).getRootElement();
List<Bar> bars = root.getFoo().getBars();
if (bars.size() > 1) {
    String s = bars.get(1).getValue();
    // do something
}
```

I suppose this looks a little bit nicer. You often work with your model in more than one place. Re-creating the model is too inefficient, so we cache it for you, and any subsequent calls to `DomManager.getFileElement()` will return the same instance. So, it is useful to invoke this method just once, and then keep everywhere only the "root" object you've obtained. In this case you wont need to repeat that scary first line, and the code will look even nicer.

**CN:**  我想这个看起来更好一点。您经常在多个地方使用您的模型。重新创建模型太低效了，所以我们为您缓存它，任何后续对`DomManager.getFileElement()`的调用都会返回相同的实例。因此，只调用此方法一次是很有用的，然后在任何地方只保留您获得的"root"对象。在这种情况下，您不需要重复第一行代码，代码看起来会更好。

It is also important to note that with this scenario we avoid potential `NullPointerException`: our DOM guarantees that every method accessing a tags child will return a not-null element, even if the correspondingly-named sub-tag doesn't exist. That may seem strange at a first glance, but it appears to be rather convenient. How does it work? Simple. Given those interfaces, DOM generates all code for accessing correct sub-tags and creating model elements at runtime. The sub-tag names and element types are taken from method names, return types and method annotations, if any. In most cases annotations can be omitted, as in our example, but this is discussed further in this article anyway.

**CN:**  同样重要的是，在这个场景中，`NullPointerException`:我们的DOM保证每个访问标记的子标记的方法都将返回一个非空元素，即使相应命名的子标记并不存在。乍一看，这似乎很奇怪，但似乎很方便。它是如何工作的?简单。有了这些接口，DOM就可以生成所有代码来访问正确的子标记并在运行时创建模型元素。子标记名和元素类型取自方法名、返回类型和方法注释(如果有的话)。在大多数情况下，注释可以被省略，就像在我们的示例中一样，但是本文将进一步讨论这一点。

Now let us explore more thoroughly what the DOM can do, and look at possible ways of representing various XML concepts such as tag content, attributes or sub-tags. Later we will discuss basic methods for working with the model, as well as cover more advanced functionality. Finally, we'll see how to easily create an UI editor for DOM model elements.

**CN:**  现在让我们更深入地研究DOM可以做什么，并研究表示各种XML概念(如标记内容、属性或子标记)的可能方法。稍后，我们将讨论使用模型的基本方法，以及更高级的功能。最后，我们将看到如何轻松地为DOM模型元素创建一个UI编辑器。

## Building the Model——创建模型


### Tag Content——标签内容

In XML PSI, tag content is referred to as tag value, so well do the same for consistency. To read and change a tag value, you have to add two methods (getter and setter) to your interface, like this:

**CN:**  在XML PSI中，标签内容被称为标签值，为了保持一致性，我们也这样做。要读取和更改标签值，您必须向接口添加两个方法(getter和setter)，如下所示:

```java
String getValue();
void setValue(String s);
```

These method names (`getValue` and `setValue`) are standard, and they are used for accessing tag values by default. If you want to use custom method names for the same goal, you should annotate these methods with `@TagValue`, for example:

**CN:**  这些方法名(`getValue`和`setValue`)是标准的，默认情况下它们用于访问标签值。如果你想自定义方法名达到同样的效果，可以用@TagValue注释这些方法，例如:

```java
@TagValue
String getTagValue();

@TagValue
void setTagValue(String s);
```

As you can see, our accessors work with `String` values. This is natural, since XML represents a text format, and tag content is always text. But sometimes you may want to operate with integers, booleans, enums, or even class names (they, of course, will be represented as `PsiClass`), and more generic Java types (`PsiType`). In such cases, you just need to change the type in methods to the one you need, and everything will keep working correctly.

**CN:**  如您所见，我们的访问器使用`String`值。这很自然，因为XML表示一种文本格式，而标记内容总是文本。但是有时您可能希望操作整数、布尔值、枚举，甚至类名(当然，它们将被表示为`PsiClass`)和更通用的Java类型(`PsiType`)。在这种情况下，您只需要将方法的类型更改为您需要的类型，一切都会保持正常工作。

#### Custom Value Types——自定义值类型

If you operate with even more exotic types, you should tell DOM how to deal with them. First, annotate your accessor methods with the `@Convert` annotation, and specify your own class that should extend the `Converter<T>` class in the annotation. Here `T` is your exotic type, while `Converter<T>` is a thing that knows how to convert values between `String` and `T`. If the value cannot be converted (for example, "foo" is not convertible into `Integer`), the converter may return `null`. Please also note that your implementation should have a no-argument constructor.

**CN:**  如果您使用的是更奇怪的类型，您应该告诉DOM如何处理它们。首先，使用`@Convert`注释您的访问器方法，并指定您自己的类，该类应该在注释中扩展`Converter<T>`类。这里`T`是您的外来类型，而`Converter<T>`是一个知道如何在`String`和`T`之间转换值的东西。如果值不能转换(例如，"foo" 不能转换为`Integer`)，转换器可能会返回`null`。还请注意，您的实现应该有一个无参数构造函数。

Let us consider an interesting case when `T` represents an enum value. Usually, the converter just searches for enum elements with the names specified in XML. But sometimes, for their names, you may need or want to use values that are not valid Java identifiers. For example, CMP version in EJB may be "1.x" or "2.x", but you can't create Java enums with such names. For such cases, let your enum implement `NamedEnum` interface, and then name your enum elements as you wish. Now, just provide the `getValue()` implementation that will return the right value to match with XML contents, and voilà\!
In our example, the code will look as follows:

**CN:**  让我们考虑一个有趣的情况，当`T`表示enum值时。通常，转换器只搜索在XML中指定名称的enum元素。但是有时候，对于它们的名称，您可能需要或希望使用不是有效的Java标识符的值。例如，EJB中的CMP版本可能是"1.x"或"2.x" ，但是您不能使用这样的名称创建Java枚举。对于这种情况，让您的enum实现`NamedEnum`接口，然后根据需要命名enum元素。
现在，只需提供`getValue()`实现，它将返回与XML内容匹配的正确值，瞧!在我们的例子中，代码如下:

```java
enum CmpVersion implements NamedEnum {
    CmpVersion_1_X ("1.x"),
    CmpVersion_2_X ("2.x");

    private final String value;
    
    private CmpVersion(String value) { 
        this.value = value; 
    }

    public String getValue() { return value; }
}
```

As we have already mentioned, an XML tag may have lots of artifacts besides its value: there can be attributes, children, but rather often (e.g., according to DTD or Schema) it should have only the value. Of course such tags also need a DOM element to associate with. And we provide such an element:

**CN:**  正如我们已经提到的，XML标记除了它的值之外可能还有许多工件:可能有属性、子元素，但是通常(例如，根据DTD或模式)它应该只有值。当然，这样的标记还需要一个DOM元素来关联。我们提供了这样一个元素:

```java
interface GenericDomValue<T> {
    T getValue();
    void setValue(T t);

    @TagValue
    String getStringValue();

    @TagValue
    void setStringValue(String s);
}
```

So, you can just specify a particular `T` when using this interface --- and everything will work. Methods that work with `String` are provided for many reasons. For example, your `T` is [`PsiClass`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiClass.java). It would be useful to highlight invalid values in UI. To get the value to highlight (the string from the XML file) we have the `getStringValue()` method. The error message will be taken from the converter via `getErrorMessage()`.

**CN:**  所以，当你使用这个接口时，你可以指定一个特定的`T`，一切都会正常工作。提供使用`String`的方法有很多原因。例如，你的`T`是[`PsiClass`](upsource:///java/java-psi-api/src/com/intellij/psi/PsiClass.java)。在UI中高亮无效的值是很有用的。要获得突出显示的值(来自XML文件的字符串)，我们有`getStringValue()`方法。错误消息将通过`getErrorMessage()`从转换器中提取。

### Attributes——属性

Attributes are also rather simple to deal with. You can read their values, set them, and operate with different types. So it's natural to create something like `GenericDomValue<T>` and then work as usual. "Something like" will be an inheritor, as shown below:

**CN:**  属性处理起来也相当简单。您可以读取它们的值、设置它们并对不同的类型进行操作。所以很自然地，我们会创建一些类似于`GenericDomValue<T>`的东西，然后像往常一样工作。"Something like" 将会是一个继承者，如下图所示:

```java
interface GenericAttributeValue<T> extends GenericDomValue<T> {
    XmlAttribute getXmlAttribute();
}
```

Consider that you want to work with an attribute named _some-class_ having a value of type `PsiClass`:

**CN:**  考虑到您想要处理一个名为some-class的属性，其值类型为`PsiClass`:

```java
@Attribute("some-class")
GenericAttributeValue<PsiClass> getMyAttributeValue();
```

That's all\! Now you can get/set values, resolve this `PsiClass`, get its `String` representation, etc. The name of the attribute will be taken from the method name (see next paragraph). If you name your method in a special way, you can even omit the annotation. For example:

**CN:**  到这里!您可以获取/设置值，解析这个PsiClass，获取它的字符串表示，等等。属性的名称将取自方法名(参见下一段)。如果以特殊的方式命名方法，甚至可以省略注释。例如:

```java
GenericAttributeValue<PsiClass> getSomeClass();
```

The [`DomNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomNameStrategy.java) interface specifies how to convert accessor names to XML element names. Or more precisely, not the full accessor names, but rather the names minus any "get", "set" or "is" prefixes. The strategy class is specified in the `@NameStrategy` annotation in any DOM element interface. Then any descendants and children of this interface will use this strategy. The default strategy is [`HyphenNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/HyphenNameStrategy.java), where words are delimited by hyphens (see sample above). Another common variant is [`JavaNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/JavaNameStrategy.java) that capitalizes the first letter of each word, as in Java's naming convention. In our example, the attribute name would be "someClass".

**CN:**  [`DomNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomNameStrategy.java)接口指定如何将访问器名称转换为XML元素名称。或者更准确地说，不是完整的访问器名称，而是去掉任何"get"、"set"或"is"前缀的名称。strategy类在任何DOM元素接口的`@NameStrategy`注释中指定。然后，此接口的任何后代和子代都将使用此策略。默认策略是[`HyphenNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/HyphenNameStrategy.java)，其中单词用连字符分隔(参见上面的示例)。另一种常见的变体是[`JavaNameStrategy`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/JavaNameStrategy.java)，它将每个单词的第一个字母大写，就像Java的命名约定一样。在我们的例子中，属性名应该是“someClass”。

If attribute doesn't define a `PsiClass`, but some other custom `T` that needs a converter, you just need to specify the `@Convert` annotation to the getter.

**CN:**  如果属性没有定义`PsiClass`，但是定义了其他需要转换器的自定义`T`，则只需将`@Convert`注释指定为getter。

Please note that the attributes getter method will never return `null`, even if the attribute isn't specified in XML. Its `getValue()`, `getStringValue()` and `getXmlAttribute()` methods will return `null`, but the DOM interface instance will exist and be valid. If the element has an underlying attribute, this can be easily fixed (surely, only if you need that): just call the `undefine()` method (defined in `DomElement`), and the XML attribute disappears, while [`GenericAttributeValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericAttributeValue.java) remains valid.
**CN:**  请注意，即使属性没有在XML中指定，属性getter方法也不会返回`null`。它的`getValue()`、`getStringValue()`和`getXmlAttribute()`方法将返回`null`，但是DOM接口实例将存在并有效。如果元素有一个底层属性，那么这个问题很容易解决(当然，只有在需要的时候):只需调用`undefine()`方法(在`DomElement`中定义)，XML属性就会消失，而[`GenericAttributeValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericAttributeValue.java)仍然有效。

### Children: Fixed Number——子元素：固定数量

You may often deal with tags that have at most one sub-tag with the given name (e.g. `<ejb-name>`, `<ejb-class>` or `<cmp-field>`) in tags defining entity EJBs. To work with such children, provide getters for them. These getters should have a return type that extends `DomElement`:

**CN:**  在定义实体EJB的标记中，您可能经常要处理最多有一个子标记和给定名称(例如`<ejb-name>`、`<ejb-class>` 或 `<cmp-field>`)的标记。要和这样的子元素一起工作，就要给他们提供getters。这些getter应该有一个扩展`DomElement`的返回类型:

```java
GenericDomValue<String> getEjbName();
GenericDomValue<String> getEjbClass();
CmpField getCmpField();
```

There's also an annotation to designate such children explicitly: `@SubTag`. Its "value" attribute contains a tag name. If it is not specified, the name is implied from the method name using the current name strategy.

**CN:**  还有一个注释可以显式地指定这些子元素:`@SubTag`。它的“value”属性包含一个标签名。如果未指定，则使用当前名称策略从方法名称中隐含该名称。

Sometimes it is the sub-tag's presence that means something, rather than its content --- `<unchecked>` in EJB method permissions, for example. If it exists, then permissions are unchecked, otherwise checked. For such things one should create a special `GenericDomValue<Boolean>` child. Usually its `getValue()` returns `true` if there's "true" in a tag value, `false` if there's "false" in a tag value, and `null` otherwise. In the `@SubTag` annotation, you can specify the attribute like `indicator=true`. In this case, `getValue()` will return `true` if the tag exists and `false` otherwise.

**CN:**  有时，子标记的存在意味着什么，而不是它的内容——例如，EJB方法权限中的`<unchecked>`。
如果存在，则未选中权限，否则选中权限。对于这样的事情，一个人应该创造一个特殊的子`GenericDomValue<Boolean>`。
通常它的`getValue()`在标签值中有"true"时返回`true`，在标签值中有"false"时返回false，否则返回null。
在`@SubTag`注释中，可以像`indicator=true`那样指定属性。
在这种情况下，如果标记存在，`getValue()`将返回`true`，否则返回`false`。

Let's consider another interesting example inspired by EJB, where there is a relation that has two roles, each designating one relation end: first role and second role. Both are represented by tags with the same values. So, we could create a collection of role elements, and every time we access some role we would check if this collection has sufficient number of elements. But one of the main purposes of the DOM is to eliminate unnecessary checks. So why cant we have a fixed (more than one) number of children with the same tag name? Let's have them\!

**CN:**  让我们考虑另一个受EJB启发的有趣示例，其中有一个具有两个角色的关系，每个角色指定一个关系结束:第一个角色和第二个角色。两者都由具有相同值的标记表示。因此，我们可以创建一个角色元素集合，每次访问某个角色时，我们都会检查这个集合是否有足够数量的元素。但是DOM的主要目的之一是消除不必要的检查。那么，为什么我们不能有一个固定的(多于一个)数目的具有相同标记名称的子标记呢?让他们吧!

```java
@SubTag(value = "ejb-relationship-role", index = 0)
EjbRelationshipRole getEjbRelationshipRole1();

@SubTag(value = "ejb-relationship-role", index = 1)
EjbRelationshipRole getEjbRelationshipRole2();
```

The first method will return the DOM element for the first subtag named `<ejb-relationship-role>`, and the second --- for the second one. Hence the term "fixed-number" for such children. According to DTD or Schema, there should be fixed number of subtags with the given name. Most often this fixed number is 1; in our case with the relations it is 2. Just like attributes, fixed-number children exist regardless of underlying tag existence. If you need to delete tags, it can be done with the help of the same `undefine()` method.

**CN:**  第一个方法将返回第一个名为`<ejb-relationship-role>`的子标记的DOM元素，第二个方法返回第二个子标记的DOM元素。因此，这种子元素被称为“固定数量”。根据DTD或模式，应该有固定数量的具有给定名称的子标记。这个固定的数字通常是1;在关系式中，它是2。就像属性一样，固定数量的子元素的存在与底层标签的存在无关。如果需要删除标记，可以使用相同的`undefine()`方法来完成。

For children of [`GenericDomValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericDomValue.java) type, you can also specify a converter, just as you can for attributes.

**CN:**  对于[`GenericDomValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericDomValue.java)类型的子类型，您还可以指定一个转换器，就像您可以指定属性一样。

### Children: Collections——子元素：集合

One more common case in DTD and Schemas is when children have the same tag name and a non-fixed upper limit in count. Their accessors differ from those of the fixed-number children in the following: the return result is `Collection` or `List` of a special type that extends `DomElement`, and if you want to use name strategies, the method name must be in pluralized form. For example, in EJB we would have the following method:

**CN:**  DTD和模式中更常见的一种情况是，子元素具有相同的标记名和不固定的计数上限。它们的访问器与下面这些固定数量的子元素的访问器不同:返回的结果是扩展`DomElement`的特殊类型的`Collection`或`List`，如果希望使用名称策略，则方法名称必须是复数形式。例如，在EJB中我们有以下方法:

```java
List<Entity> getEntities();
```
There's also an annotation `@SubTagList` where you can explicitly specify the tag name.

**CN:**  还有一个注释`@SubTagList`，您可以在其中显式地指定标记名。

Returned collections cannot be modified directly. To delete an element from collection, just call `undefine()` on this element. The tag will then be removed, and element will become invalid (`DomElement.isValid() == false`). Note that this behavior differs from that of fixed-number children and attributes: they are always valid, even after `undefine()`. Again, unlike those children types, collection children always have valid underlying XML tags.
**CN:**  不能直接修改返回的集合。要从集合中删除一个元素，只需调用该元素上的`undefine()`。然后标签将被删除，元素将成为无效的(`DomElement.isValid() == false`)。请注意，这种行为不同于固定数量的子元素和属性:它们总是有效的，即使在`undefine()`之后也是如此。同样，与那些子类型不同，集合子类型始终具有有效的底层XML标记。

Adding elements is a little bit harder. Since all DOM elements are created internally, you can't just pass some of your DOM elements to some method, to add the element to the collection. In fact, you have to ask a parent element to add a child to the collection. In our example it's done in the following way:
添加元素有点难。因为所有的DOM元素都是在内部创建的，所以您不能只是将一些DOM元素传递给某个方法来将元素添加到集合中。实际上，您必须要求父元素将子元素添加到集合中。在我们的例子中，它是这样做的:

```java
Entity addEntity(int index);
```

which adds an element to wherever you want, or

**CN:**  将元素添加到您想要的任何位置，还是

```java
Entity addEntity();
```
which adds a new DOM element to the end of the collection. Please note the singular tense of the word "Entity". That's because here we deal with one `Entity` object, while in the collection getter we dealt with potentially many entities.

**CN:**  它将一个新的DOM元素添加到集合的末尾。请注意“Entity”一词的单数时态。这是因为在这里我们处理一个`Entity`对象，而在集合getter中我们可能处理多个实体。

Now, you can do anything you want with the returned value: modify, define the tag's value, children, etc.

**CN:**  现在，您可以对返回的值做任何您想做的事情:修改、定义标记的值、子元素等等。

The last common case is also a collection, but one consisting of tags with different names that are arbitrarily mixed. To work with it, you should define collection getters for all tag names within the mixed collection, and then define an additional specially annotated getter:

**CN:**  最后一种常见的情况也是一个集合，但它由任意混合的具有不同名称的标记组成。要使用它，您应该为混合集合中的所有标记名定义集合getter，然后定义一个附加的特殊注释getter:

```java
// <foo> elements
List<Foo> getFoos();

// <bar> elements
List<Bar> getBars();

// all <foo> and <bar> elements
@SubTagsList({"foo", "bar"})
List<FooBar> getMergedListOfFoosAndBars();
```
The annotation here is mandatory - we cannot guess several tag names from one method name.

**CN:**  这里的注释是强制性的——我们不能从一个方法名中猜测多个标记名。

To add elements to such mixed collections, you should create "add" methods for each possible tag name:

**CN:**  要将元素添加到这样的混合集合中，您应该为每个可能的标记名创建“add”方法:

```java
@SubTagsList(value={"foo","bar"}, tagName="foo") Fubar addFoo();
@SubTagsList(value={"foo","bar"}, tagName="bar") Fubar addBar(int index);
```
The index parameter in the last example means the index in the merged collection, not in the collection of tags named "bar".

**CN:**  最后一个示例中的索引参数表示合并集合中的索引，而不是名为“bar”的标记集合中的索引。

### Dynamic Definition——动态定义
You can extend existing DOM model at runtime by implementing `com.intellij.util.xml.reflect.DomExtender<T>`. Register it in "extenderClass" attribute of EP `com.intellij.dom.extender`, where "domClass" specifies DOM class `<T>` to be extended. [`DomExtensionsRegistrar`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/reflect/DomExtensionsRegistrar.java) provides various methods to register dynamic attributes and children.

**CN:**  您可以在运行时通过实现`com.intellij.util.xml.reflect.DomExtender<T>`来扩展现有的DOM模型。在EP `com.intellij.dom.extender`的“extenderClass”属性中注册它，其中“domClass”指定要扩展的DOM类`<T>`。

If the contributed elements depend on anything other than plain XML file content (used framework version, libraries in classpath, ...), make sure to return `false` from `DomExtender#supportsStubs`.
如果贡献的元素依赖于纯XML文件内容之外的任何内容(使用的框架版本、类路径中的库……)，请确保从`DomExtender#supportsStubs`返回`false`。

### Generating DOM from existing XSD——从现有XSD生成DOM
DOM can be generated automatically from existing XSD/DTD. Output correctness/completeness will largely depend on the input scheme and may require additional manual adjustments.
可以从现有的XSD/DTD自动生成DOM。输出的正确性/完整性很大程度上取决于输入方案，可能需要额外的手动调整。

Follow these steps:遵循以下步骤:

* Run IntelliJ IDEA with _Plugin DevKit_ enabled in [internal mode](/reference_guide/internal_actions/enabling_internal.md)
运行IntelliJ IDEA与插件DevKit在[internal mode(内部模式)](/reference_guide/internal_actions/enabling_internal.md)中启用
* Select *Tools \| Internal Actions \| DevKit \| Generate DOM Model*
选择*Tools \| Internal Actions \| DevKit \| Generate DOM Model*
* Select Scheme file and set options, then click "Generate" to generate sources
选择方案文件，设置选项，点击“生成”，生成源文件
* Modify generated sources according to your needs
根据您的需要修改生成的源代码

### IDE support——IDE支持
_Plugin DevKit_ supports the following features for working with DOM related code:
_Plugin DevKit_支持使用DOM相关代码的以下特性:

* [`DomElement`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomElement.java) - provide implicit usages for all DOM-related methods defined in inheriting classes (to suppress "unused method" warning)
为所有在继承类中定义的与dom相关的方法提供隐式用法(以避免“未使用的方法”警告)
* [`DomElementVisitor`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomElementVisitor.java) - provide implicit usages for all DOM-related visitor methods defined in inheriting classes (to suppress "unused method" warning)
为所有在继承类中定义的与dom相关的访问者方法提供隐式用法(以避免“未使用的方法”警告)

## Working with the DOM——使用DOM

### Class Choosers——类选择器
It often happens that a collection contains same-named tags that may have different structure or even be represented by different types in the DTD or Schema. As an example, JSF Managed Beans may be of three types. If a `<managed-bean>` tag contains a `<map-entries>` sub-tag, then the Managed Bean type is `MapEntriesBean`. If it contains a `<list-entries>` sub-tag — can you guess? Right — `ListEntriesBean`! Otherwise it's a `PropertyBean` (all three interfaces extend `ManagedBean`). And when we write `List<ManagedBean> getManagedBeans()`, we expect to get not only a list where all elements are instances of the `ManagedBean` interface, but a list where each element is of a certain type, i.e. `MapEntriesBean`, `ListEntriesBean`, or `PropertyBean`.
经常发生的情况是，一个集合包含相同名称的标记，这些标记可能具有不同的结构，甚至在DTD或模式中由不同的类型表示。例如，JSF托管bean可能有三种类型。如果`<managed-bean>`标签包含`<map-entries>`子标签，则托管Bean类型为`MapEntriesBean`。如果它包含一个`<list-entries>`子标签 --- 你能猜到吗？正确---`ListEntriesBean`!否则它就是一个`PropertyBean`(所有三个接口都继承了`ManagedBean`)。当我们编写`List<ManagedBean> getManagedBeans()`时，我们不仅希望得到一个列表，其中所有元素都是`ManagedBean`接口的实例，而且还希望得到一个列表，其中每个元素都是某种类型的，例如`MapEntriesBean`、`ListEntriesBean`或`PropertyBean`。

In such cases one should decide which interface the DOM element should actually implement (according to the given tag). This is achieved by extending the [`TypeChooser`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/TypeChooser.java) abstract class:
在这种情况下，应该决定DOM元素应该实际实现哪个接口(根据给定的标记)。这是通过扩展[`TypeChooser`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/TypeChooser.java)抽象类实现的:

```java
public abstract class TypeChooser {
    public abstract Type chooseType(XmlTag tag);
    public abstract void distinguishTag(XmlTag tag, Type aClass) throws IncorrectOperationException;
    public abstract Type[] getChooserTypes();
}
```

Here, the first method (`chooseType()`) does exactly what it is named after (chooses the particular type, most often it's a class). The second one (`distinguishTag()`) acts in reverse: it modifies a tag so that when the element is read from an XML file next time (for example, after the user has closed and opened the project again), the newly created DOM element will implement the same interface and no model data will be lost. Finally, `getChooserTypes()` just returns all the types that could be returned by `chooseType()`.
在这里，第一个方法(`chooseType()`)的作用与它的名称完全相同(选择特定的类型，通常是一个类)。第二个(`distinguishTag()`)行为反过来:这样修改一个标签,当元素是下次读取XML文件(例如,在用户再次关闭,打开项目),新创建DOM元素不会实现相同的接口和模型数据将丢失。最后，`getChooserTypes()`只返回`chooseType()`可以返回的所有类型。

To make your `TypeChooser` work, register it in your overridden `DomFileDescription.initializeFileDescription()` method by calling `registerTypeChooser()`.
要使`TypeChooser`工作，请通过调用`registerTypeChooser()`将其注册到覆盖的`DomFileDescription.initializeFileDescription()`方法中。

### Useful Methods of DomElement and DomManager——DomElement和DomManager的有用方法


#### PSI Connection——PSI连接
Of course, DOM is tightly connected to XML PSI, so there's always a way of getting the `XmlTag` instance (which can be `null` for fixed-number children and attributes) using the `getXmlTag()` method. We remember that in `GenericAttributeValue` there's also the `getXmlAttribute()` method. In general case there is `getXmlElement()` method. You can also get a DOM element by its underlying XML PSI element using the `DomManager.getDomElement()` method.

If DOM element has no underlying XML element, it can be created by calling `ensureTagExists()`. To delete a tag, use the already known `undefine()` method. This method will always delete the underlying XML element (tag or attribute). If the element was a collection child, then neither it nor its entire sub-tree will be valid anymore.

#### Tree Structure——树结构
In every normal tree there's always a possibility to walk up. `DomElement` is no exception. Method `getParent()` just returns element's parent in tree.

The method `<T extends DomElement> T getParentOfType(Class<T> requiredClass, boolean strict)` returns the tree ancestor of the given class. You can see the standard _strict_ parameter, that can return the DOM element itself, if it's `false` and your current DOM element is an instance of _requiredClass_.

Finally, `getRoot()` will return the `DomFileElement`, which is the root of every DOM tree.

#### Validity——有效性
An element becomes invalid if it has been deleted explicitly or due to external PSI changes. Fixed-number children and attributes are meant to stay valid as long as possible, no matter what happens with XML. They can become invalid only if they have collection tree ancestor that has been deleted.

Newly created DOM elements are always correct and valid, so their `isValid()` methods will return `true`.

Element validity is very important, since you cannot invoke any methods on invalid elements (except, of course, `isValid()` itself).

#### DOM Reflection——DOM反射
DOM also has a kind of reflection, called "Generic Info". One would use it to be able to access children by tag names directly, instead of calling getter methods. See [`DomGenericInfo`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/reflect/DomGenericInfo.java) interface and `getGenericInfo()` methods in `DomElement` and `DomManager` for more information. There's also `DomElement.getXmlElementName()` method that returns the name of a corresponding tag or attribute.

#### Presentation——描述
<!-- TODO: using @Presentation -->

`DomElement.getPresentation()` returns an instance of [`ElementPresentation`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ElementPresentation.java), an interface that knows presentable element type, name, and sometimes even its icon. Presentations are actually obtained from presentation factory objects that, like ClassChoosers's, should be registered in [`ElementPresentationManager`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ElementPresentationManager.java) as early as possible. You can specify type name and icon for all elements of some class, ways of getting type name, icon and presentable name for particular objects. When not specified, presentable name is taken from the object itself, if it contains a method annotated with `@NameValue` annotation, that returns `String` or `GenericValue`. If there's no such method, it will return `null`. For `DomElement`, there's another way to get this presentable name: 
`DomElement.getGenericInfo().getElementName()`.

#### Events——事件
If you want to be notified on every change in the DOM model, add `DomEventListener` to `DomManager`. DOM supports the following events: tag value changed, element defined/undefined/changed, and collection child added/removed.

#### Highlighting Annotations——高亮显示注释
The DOM supports error checking and highlighting. It's based on annotations which you add to the DOM element in a special place (don't confuse these annotations with the ones of Java 5 — they are very different). You need to implement the [`DomElementAnnotator`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/highlighting/DomElementsAnnotator.java) interface, and override `DomFileDescription.createAnnotator()` method, and create this annotator there. In `DomElementsAnnotator.annotate(DomElement element, DomElementsProblemsHolder annotator)` you should report about all errors and warnings in the element's sub-tree to the annotator (`DomElementsProblemsHolder.createProblem()`). You should return this annotator in the corresponding virtual method of the `DomFileDescription`.

#### Automatic highlighting (BasicDomElementsInspection)——自动高亮显示(BasicDomElementsInspection)
The following errors can be highlighted automatically by providing an instance of `BasicDomElementsInspection`:

- `@Required` element missing or having empty text
- XML value cannot be converted by some `Converter`
- name is not unique while it should be

The latter case requires you to specify the name getter with `@NameValue` annotation. The checking uses the `DomFileDescription.getIdentityScope()` method to get the element defining the root scope in which the name should be unique.

To suppress spellchecking annotate your DomElement with `@com.intellij.spellchecker.xml.NoSpellchecking`.

#### Required Children
There is a common case in error highlighting, when one needs to say, that some required sub-tag or attribute is missing. DOM will do this for you automatically, if you annotate the getter for that child with the `@Required` annotation. For collection children getters, this annotation will mean, that the collection should be not empty (corresponding to '+' sign in DTD). Also, when you create a new element that has required fixed-number or attribute children, their tags or attributes will also be created in XML.



### Resolving
Remember the interface `GenericDomValue<T>` and its sub-interface `GenericAttributeValue<T>`? Remember, that ANY class may be passed as `T` — for example, let's interpret `GenericDomValue<PsiClass>` as a reference to a class. Then we can always consider it as a reference to an object of class `T`! With Strings or enums, it is not a very useful idea, but we'll use it in another way. Very often XML has such a structure that an object is declared at some place, and is referenced at some other place (more precisely, in a tag or attribute value). So, if you want to create a method like `GenericValue<MyDomElement> getMyDomElementReference()`, then you just have to specify a proper converter that will find an instance in your model of `MyDomElement` with the name specified in the `GenericDomValue`.

That's the core idea. Since creating such converters is quite boring, we've done it for you. You don't have to annotate reference getters at all, as the name resolution will be made automatically. Elements will be searched by name, and the name will be taken from the method annotated with `@NameValue`. The converter used is [`DomResolveConverter`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/DomResolveConverter.java). Its constructor takes a parameter, so it can't be referenced in `@Convert` annotation, but its subclasses (if you create them) — can. If you still want to specify explicitly that your reference to `DomElement` should be resolved "model-wide", use the `@Resolve` annotation parameterized with the desired class. The resolution scope will be taken from the `DomFileDescription.getResolveScope()`.

In addition to the above, auto-resolving in DOM also provides some features in your XML text editor: error highlighting, completion, Find Usages, Rename Refactoring... Unresolved references will be highlighted, and even completed. If you want to create a custom converter and want to have this code insight with it, you should extend not only the [`Converter`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/Converter.java) but [`ResolvingConverter`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ResolvingConverter.java). It has one more method `getVariants()`, where you'll have to provide the collection consisting of all targets your reference may resolve to. Those familiar with [`PsiReference`](upsource:///platform/core-api/src/com/intellij/psi/PsiReference.java) will recognize the similarities here.

If you need to choose a `Converter` depending on other values (e.g. in sibling/parent element) or any runtime condition (e.g. presence or version of library), you can use `WrappingConverter`. See also `GenericDomValueConvertersRegistry` for managing an extensible registry of available Converters to choose from.

### Mock and Stable Elements
Your DOM elements do not have to be tied to a physical file. `DomManager.createMockElement()` will help you to create a virtual element of a given class with the given module. An element may be physical or not. 'Physical' here means that DOM will create a mock document for it, so you can enjoy Undo functionality if you pass this document to the right place in file editor.

`DomElement.copyFrom()` allows you to copy information from one `DomElement` to another. In fact, it just replaces XML tags, and all the old data is lost. Nevertheless, the element's fixed-number children don't become invalid, they only contain new tag values, attribute values, etc. The tree is actually rather conservative.

The combination of `createMockElement()` and `copyFrom()` is useful for editing element contents in dialogs. You create a mock copy of an element, work with it in the dialog and then, if the user doesn't cancel, copy the element back to the main model. Since it's a common case, a special shortcut method has been created in `DomElement`, called `createMockCopy()`.

IntelliJ Platform's XML parser is incremental: changes in text do not cause the whole file to be reparsed. But you should keep in mind that this rule may sometimes not work correctly. For example, your DOM elements can unexpectedly become broken as a result of manual editing of the XML file (even if it didn't happen inside those elements). If a file editor depends on such a broken element, this can lead to closing the tab, which isn't very nice from the user's point of view. For example, suppose you have an entity bean named "SomeEntity". You open an editor for it, then you go into the XML, change the tag name from entity to session, and then back to entity. Of course, no DOM element can survive after such blasphemy. But notwithstanding, you still want your editor to stay open! Well, there is a solution, and it's called `DomManager.createStableValue(Factory factory)`.
This method creates a DOM element that delegates all its functionality to some real element (returned from the factory parameter). As soon as that real element becomes invalid, the factory is called once more, and if it returns something valid, it becomes the new delegate. And so on... In the example with EJB, the factory would once again look for an Entity Bean named "SomeEntity".

Stable DOM elements also implement the `StableElement` interface, which has the following methods:

* `DomElement getWrappedElement()` — just returns the current element to which all method calls are delegated;
* `void invalidate()` — makes the wrapped element invalid. Any following method call will cause the factory to create a new delegate;
* `void revalidate()` — calls the factory, and if it returns something new (i.e. not the same as the current wrapped element) invalidates the old value and adopts the new one.

### Visitor
Visitor is a very common design pattern. DOM model also has a visitor, and it's called `DomElementVisitor`. The `DomElement` interface has methods `accept()` and `acceptChildren()` that take this visitor as a parameter. If you look at the interface `DomElementVisitor` itself, you may be surprised, since it has only one method: `visitDomElement(DomElement)`. Where is the Visitor pattern? Where are all those methods with names like `visitT(T)` that are usually found in it? There are no such methods, because the actual interfaces (T's) aren't known to anyone except you. But when you instantiate the `DomElementVisitor` interface, you may add there these `visitT()` methods, and they will be called! You may even name them just `visit()`, specify the type of the parameter, and everything will be fine. For example, if you have two DOM element classes — `Foo` and `Bar` — your visitor may look like this:

```java
class MyVisitor implements DomElementVisitor {
    void visitDomElement(DomElement element) {}
    void visitFoo(Foo foo) {}
    void visitBar(Bar bar) {}
}
```

### Implementation
Sometimes you may want to extend your model with some functionality that isn't directly connected with XML, but relates to your program logic. And the most appropriate place for this functionality is the DOM element interface. What to do then?

The simplest case is when you want to add to your interface a method that returns exactly what some other getter in this element (or in one of its children) returns. You can easily write this helper method and annotate it with the `@PropertyAccessor` annotation, in which you should specify the path consisting of property names (getter names without the "get" or "is" prefixes). For example, you can write:

```java
GenericDomValue<String> getVeryLongName()

@PropertyAccessor("very-long-name")
GenericDomValue<String> getName()
```
In this case, the second method will return just the same as the first one. If there were "foo.bar.name" instead of "very-long-name" in the annotation, the system would actually call `getFoo().getBar().getName()` and return the result to you. Such annotations are useful when you're extending some interface that is inconsistent with your model, or you try to extract a common super-interface from two model interfaces with differently named children that have the same sense (see `<ejb-ref>` and `<ejb-local-ref>`).

The case just described is simple, but rare. More often, you really have to incorporate some logic into your model. Then nothing except Java code helps you. And it will. Add the desired methods to your interface, then create an abstract class implementing the interface, and implement there only methods that you added manually and that are not directly connected to your XML model. Note that the class should have a constructor with no arguments.

Now you only have to let DOM know that you wish to use this implementation every time you're creating a model element that should implement the necessary interface. Simply register it using
extension point `com.intellij.dom.implementation` and DOM will generate at run-time the class that not only implements the needed interface, but also extends your abstract class.

### Models across multiple files
Many frameworks require a set of XML configuration files ("fileset") to work as one model, so resolving/navigation works across all related DOM files.
Depending on implementation/plugin, providing filesets implicitly (using existing framework's setup in project) or via user configuration (usually via dedicated `Facet`) can be achieved.

Extend [`DomModelFactory`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/model/impl/DomModelFactory.java) (or [`BaseDomModelFactory`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/model/impl/BaseDomModelFactory.java) for non-`Module` scope) and provide implementation of your `DomModel`. Usually you will want to add searcher/utility methods to work with your `DomModel` implementation.
Example can be found in Struts 2 plugin (package `com.intellij.struts2.dom.struts.model`).

### DOM Stubs
> **NOTE** Please use it sparingly and only for heavily accessed parts in your DOM model, as it increases disk space usage/indexing run time.

DOM elements can be stubbed, so (costly) access to XML/PSI is not necessary (see [Indexing and PSI Stubs](/basics/indexing_and_psi_stubs.md) for similar feature for custom languages). Performance relevant elements, tag or attribute getters can simply be annotated with `@com.intellij.util.xml.Stubbed`.
Return `true` from `DomFileDescription#hasStubs` and increase `DomFileDescription#getStubVersion` whenever you change `@Stubbed` annotations usage in your DOM hierarchy to trigger proper rebuilding of Stubs during indexing.

## Building a DOM-based GUI

### Forms
All forms that deal with DOM are organized in a special way. They support two main things: getting data from XML into the UI, and saving UI data to XML. The former is called resetting, the latter — committing. There's [`Committable`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/Committable.java) interface that has corresponding methods: `commit()` and `reset()`. There's also a way of structuring your forms into smaller parts, namely the Composite pattern: [`CompositeCommittable`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/CompositeCommittable.java). Methods `commit()` and `reset()` are invoked automatically on editor tab switch or undo. So you only need to ensure that all your Swing structure is organized in a tree of `CompositeCommittable`, and all the hard work will be done by the IDE.

DOM controls are special descendants of `Committable`. All of them implement `DomUIControl`. Note that they are not Swing components — they are only a way of connecting DOM model and Swing components. One end of the connection — the DOM element — is usually specified in the controls constructor. The other end — Swing component — can be obtained in 2 ways. The first is to ask DOM control to create it. But that is rather inconvenient if you want to create the forms in, say, IntelliJ IDEA's GUI Designer. In that case, you'll need the second way: ask the control to `bind()` to an existing Swing component of a correct type (that depends on the type of value that you're editing). After that, your Swing components will be synchronized with DOM, they'll even highlight errors reported by `DomElementsAnnotator`.

Sometimes you may need to do some work (enable or disable some components, change their values) after a particular DOM control is committed. Then you should define the `addCommitListener()` method of that DOM control and override the `CommitListener.afterCommit()` method. This method will be invoked inside the same write action as the main `commit()`, so any changes you do in this method to the XML will be merged with the `commit()` in the Undo queue.

### Simple Controls
With simple controls, you can edit `GenericDomValue`: simple text, class names, enums and boolean values. These controls take a special object as a constructor parameter. This object should implement the `DomWrapper` interface that knows how to set/get values to/from DOM model.

We have three major DomWrapper's: [`DomFixedWrapper<T>`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/DomFixedWrapper.java) redirecting calls to [`GenericDomValue<T>`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericDomValue.java), [`DomStringWrapper`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/DomStringWrapper.java) redirecting calls to string accessors of [`GenericDomValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericDomValue.java), and [`DomCollectionWrapper`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/DomCollectionWrapper.java) that gets/sets values of the first element of the given [`GenericDomValue`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/GenericDomValue.java) collection. Some controls (those having a text field as part of itself) take additional boolean constructor parameter — _commitOnEveryChange_, whose meaning is evident from the name. We don't recommend using it anywhere except small dialogs, because committing on every change slows down the system significantly.

Most often these controls are created by `DomUIFactory.createControl(GenericDomValue)`. This method understands which control to create by using DOM reflection (`DomGenericInfo`, as you probably remember). But sometimes you may want to create the controls directly. So let's look at the simple controls more closely.

##### BooleanControl
It allows you to edit boolean values. The control is bound to `JCheckBox`.

![BooleanControl](/xml_dom_api/booleancontrol.gif)

##### ComboControl
The control is bound to a non-editable `JComboBox`, so it can be used to choose something from a limited set. One case of such a limited set is enum. Or it can be a constructor where you can provide a `Factory<List<String>>`, and return from this factory anything you want (for example, a list of database names to choose from). By default, the wrong values (written in XML, but not present in the list you've given to the control) are displayed in red. Since it's common practice to specify custom `CellRenderer` for combo boxes, the control has the `isValidValue(String)` method. If it returns `false` on the value you're rendering, you can highlight it in some way, to achieve the same result as the default renderer. Or you can just delegate to that renderer in your own way.

![ComboControl](/xml_dom_api/combocontrol.gif)

##### BooleanEnumControl
Sometimes, when there are only 2 alternatives, it's convenient to use a check box instead of combo box. This control is designed specially for such cases. While being (and being bound to) a check box, the control edits not just "true" or "false", but any two String values, or two enum elements. In the last case, it has a boolean _invertedOrder_ parameter, to specify which element corresponds to the checked state. By default _invertedOrder_ is set to `false`, so the first element corresponds to the unchecked state, and the second — to the checked one. If you set the parameter to `true`, the states will swap.

### Editor-based Controls
Please note that editor-based controls are built on IntelliJ Platform's `Editor` instead of standard `JTextField`. Since there's currently no way to instantiate Editor directly through the Open API, controls are bound to special `JPanel` inheritors, and their `bind()` method adds the necessary content to those panels.

##### TextControl
This control allows you to edit simple string values. The control is bound to a `TextPanel` component. There's also an inheritor of that panel — `MultiLineTextPanel`. If you bind a `StringControl` to it, a big editor will appear on the screen. In case you don't have space for a big editor, bind it to a `BigTextPanel`. Then it will be filled with a text editor, and the browse button will be added to open a dialog with the big editor where you can type a longer string.

##### PsiClassControl
This is a one-line editor with a browse button that opens the standard  class selection dialog. The control accepts class names only. It is bound to `PsiClassPanel`.

![PsiClassControl](/xml_dom_api/psiclasscontrol.gif)

##### PsiTypeControl
This is almost the same as PsiClassControl, but allows entering not only class names, but also Java primitive types and even arrays. It is bound to `PsiTypePanel`.


### Collection Control
There is a special table component where each row represents one collection child. It's called `DomCollectionControl<T>`, where `T` is your collection element type. To function properly, it needs `DomElement` (parent of the collection), some description of the collection (sub-tag name or a `DomCollectionChildDescription` from DOM reflection), and a `ColumnInfo` array. This can be passed to the constructor, or can be created in a `DomCollectionControl` inheritor, in an overriden method `createColumnInfos()`.

What is a column info? It's just a somewhat more comfortable way to work with the table model. It uses Java 5 generics and is more object-oriented. So, it's named `ColumnInfo<Item,Aspect>`, where `Item` is a type variable corresponding to the type of elements in the collection, and `Aspect` is a type variable corresponding to this particular column information type: `String`, `PsiClass`, `Boolean`, etc. The basic things that a column knows are: column name, column class, reading value (Aspect `valueOf(Item)`), writing value (`setValue(Item item, Aspect aspect)`), cell renderer (`getRenderer(Item)`), cell "editability" (`isCellEditable(Item)`), cell editor (`getEditor(Item)`), etc.

There are a lot of predefined column infos, so you'll probably never create a new one. 

First, if a collection child is a `GenericDomValue`, it's usually convenient to edit it directly in the table. For this, you may need one of the following classes: `StringColumnInfo`, `BooleanColumnInfo`, or more generic `GenericValueColumnInfo`. But such collections are encountered very rarely.

A more common case is when a collection element is more complex and has several `GenericDomValue` children. Then one may create a column for each of those children. The appropriate column info is `ChildGenericValueColumnInfo<T>`. It will ask you for a `DomFixedChildDescription` (one more thing from DOM reflection), a renderer and an editor — nothing else. So, the main things left to customize are the renderer and the editor.

As for the renderer, there are two main choices: `DefaultTableCellRenderer`, and IntelliJ Platform's `BooleanTableCellRenderer`. Editors are more complicated, but they closely resemble simple DOM controls.

`BooleanTableCellEditor`, `DefaultCellEditor(JTextField)`, `ComboTableCellEditor`, etc. `DomUIFactory.createCellEditor()` will create any of them automatically (including the editor for `PsiClass`), so that you won't need to think about which one to select every time.

Collection control is a complex control, so it's bound to a complex Swing component. It's called [`DomTableView`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/DomTableView.java). It has a toolbar (you can override `DomTableView.getToolbarPosition()` to customize its location), with Add and Delete buttons. If you want, you may specify custom addition actions in `DomCollectionControl.createAdditionActions()` (it's recommended to extend `ControlAddAction`). If there is only one addition action, it will be invoked after pressing the Add button; if there are many, then a popup menu will be displayed. To change the removal policy, override the `DomCollectionControl.doRemove(List<T>)` method.

The toolbar may also have an Edit button, if you specify that `DomCollectionControl.isEditable()`. To add a behavior to this button, override `DomCollectionControl.doEdit(T)`. There can also be a Help button, if you pass a non-null String _helpId_ parameter while constructing your `DomTableView`.

If there are no items in the collection, `DomTableView` may display a special text (`DomTableView.getEmptyPaneText()`), instead of an empty table.

You can add your own popup menu to the control. Call the `DomTableView.installPopup()` method after construction, and pass a `DefaultActionGroup` with your popup actions.

Tables can have single or multiple (default) row selection. If you want to change this behavior, override `DomTableView.allowMultipleRowsSelection()`.

![CollectionControl](/xml_dom_api/collectioncontrol.gif)

### UI Organization
The easiest way to create a DOM-based UI form is to extend the [`BasicDomElementComponent`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/BasicDomElementComponent.java) class. This will require you to pass some DOM element to the constructor. Then you bind an IntelliJ IDEA GUI Designer form to your subclass and design a beautiful form there. You will surely want to bind some controls to DOM UI, in which case you should of course ensure that they have right types. Finally, you should create some DOM controls in class' constructor and bind them. But you can create controls and bind them to the `DomElement`'s children — `GenericDomValue`'s automatically.

Just name your components properly and call the `bindProperties()` method in the constructor. The field names should correspond to the getter names for the element's children. They may also be prefixed with "my". Imagine that you have such DOM interface:

```java
public interface Converter extends DomElement {
    GenericDomValue<String> getConverterId();
    GenericDomValue<PsiClass> getConverterClass();
}
```

In this case, the UI form class can look like this:

```java
public class ConverterComponent extends BasicDomElementComponent<Converter> {
    private JPanel myRootPane;
    private TextPanel myConverterId;
    private PsiClassPanel myConverterClass;

    public ConverterComponent(final Converter domElement) {
        super(domElement);

        bindProperties();
    }
}
```
All the fields here are now bound to controls in a GUI form.

Very often you'll have to create your own file editor. Then, to use all the binding and undo functionality, it's suggested to inherit your [`FileEditorProvider`](upsource:///platform/platform-api/src/com/intellij/openapi/fileEditor/FileEditorProvider.java) from [`PerspectiveFileEditorProvider`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/PerspectiveFileEditorProvider.java), create an instance of [`DomFileEditor`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/DomFileEditor.java) there, and pass a [`BasicDomElementComponent`](upsource:///xml/dom-openapi/src/com/intellij/util/xml/ui/BasicDomElementComponent.java). To easily create an editor with a caption at the top, like in our EJB and JSF, you may use the static method `DomFileEditor.createDomFileEditor()`. `DomFileEditor` automatically listens to all changes in the document corresponding to the given DOM element, and therefore refreshes your component on undo. If you want to listen to changes in additional documents, use the methods `addWatchedDocument()`, `removeWatchedDocument()`, `addWatchedElement()`, `removeWatchedElement()` in `DomFileEditor`.

## Conclusion
Thank you for your time and attention. We hope you've found this article really useful. You are welcome to post your questions and comments to our [Open API and Plugin Development Forum](https://intellij-support.jetbrains.com/hc/en-us/community/topics/200366979-IntelliJ-IDEA-Open-API-and-Plugin-Development).

### Further Material
The following bundled open source plugins make (heavy) use of DOM:

- [Android](https://github.com/JetBrains/android)
- [Ant](upsource:///plugins/ant)
- [Plugin DevKit](upsource:///plugins/devkit/devkit-core)
- [Maven](upsource:///plugins/maven)
- [Struts 2](https://github.com/JetBrains/intellij-plugins/tree/master/struts2) (Ultimate Edition)

