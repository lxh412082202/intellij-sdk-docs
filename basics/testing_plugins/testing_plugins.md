---
title: Testing Plugins
redirect_from:
  - /basics/testing_plugins.html
---

Most of the tests in the *IntelliJ Platform* codebase are *model level functional tests*. What this means is the following:

**CN:**  大多数在*IntelliJ Platform*代码库中的测试都是*model level functional tests*(模型级功能测试)。这意味着:

* The tests run in a headless environment that uses real production implementations for the majority of components, except for a number of UI components.

**CN:**  测试运行在一个无头环境中，除了一些UI组件之外，该环境对大多数组件使用实际的生产实现。

* The tests usually test a feature as a whole, rather than individual functions that comprise its implementation.

**CN:**  测试通常测试整个功能，而不是组成其实现的单个功能。

* The tests do not test the Swing UI and work directly with the underlying model instead.

**CN:**  这些测试并不测试Swing UI，而是直接使用底层模型。

* Most of the tests take a source file or a set of source files as input data, execute a feature, and then compare the output with expected results. Results can be specified as another set of source files, as special markup in the input file, or directly in the test code.

**CN:**  大多数测试采用源文件或一组源文件作为输入数据，执行一个特性，然后将输出与预期结果进行比较。结果可以指定为另一组源文件，也可以指定为输入文件中的特殊标记，或者直接在测试代码中指定。

The biggest benefit of this test approach is that tests are very stable and require very little maintenance once they have been written, no matter how much the underlying implementation is refactored or rewritten.

**CN:**  这种测试方法的最大好处是，测试非常稳定，编写后只需很少的维护，不管底层实现被重构或重写了多少次。


In a product with 15+ years of lifetime that has gone through a large number of internal refactorings, we find that this benefit greatly outweighs the downsides of slower test execution and more difficult debugging of failures being compared to more isolated unit tests.

**CN:**  在一个经历了大量内部重构的15年以上生命周期的产品中，我们发现，与更独立的单元测试相比，这种好处远远超过了测试执行速度较慢和更困难的故障调试所带来的负面影响。


Another consequence of our testing approach is what our test framework does not provide:

**CN:**  我们测试方法的另一个后果是我们的测试框架没有提供:

* We do not provide a recommended approach to mocking. We have a few tests in our codebase that use JMock, but in general, we find it difficult to mock all of the interactions with *IntelliJ Platform* components that your plugin class will need to have, and we recommend working with real components instead.

**CN:**  我们不提供建议的模拟方法。在我们的代码库中有一些使用JMock的测试，但是一般来说，我们发现很难模拟与您的插件类所需的*IntelliJ平台*组件的所有交互，我们建议使用真实的组件。

* We do not provide a general-purpose framework for Swing UI testing. You can try using tools such as [FEST](https://code.google.com/p/fest/) or [Sikuli](http://sikulix.com/) for plugin UI testing, but we don't use either of them and cannot provide any guidelines for their use. Internally, we use manual testing for testing our Swing UIs. Please do not use _platform/testGuiFramework_, it is reserved for internal use.

**CN:**  我们不提供用于Swing UI测试的通用框架。您可以尝试使用工具，如[FEST](https://code.google.com/p/fest/)或[Sikuli](http://sikulix.com/)插件用户界面测试，但我们不使用他们中的任何一个，也不能提供任何使用指南。在内部，我们使用手动测试来测试Swing ui。请不要使用 _platform/testGuiFramework_ ，这是预留给内部使用的。


## Topics——主题
* [Tests and Fixtures(测试和固定装置)](tests_and_fixtures.md)
* [Light and Heavy Tests(轻、重试验)](light_and_heavy_tests.md)
* [Test Project and Testdata Directories(测试项目和Testdata目录)](test_project_and_testdata_directories.md)
* [Writing Tests(编写测试)](writing_tests.md)
* [Testing Highlighting(测试强调)](testing_highlighting.md)

Check out [this step-by-step tutorial](/tutorials/writing_tests_for_plugins.md) teaching how to write and run automated tests for your custom language plugin (source code included).

**CN:**  查看[this step-by-step tutorial](/tutorials/writing_tests_for_plugins.md)教学如何为您的自定义语言插件(包括源代码)编写和运行自动化测试。