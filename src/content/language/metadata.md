---
# title: Metadata
title: 元数据
# description: Metadata and annotations in Dart.
description: Dart 中的元数据和注解。
prevpage:
  url: /language/functions
  # title: Functions
  title: Functions
nextpage:
  url: /language/libraries
  # title: Libraries & imports
  title: Libraries & imports
---


Use metadata to provide additional static information about your code.
A metadata annotation begins with the character `@`, followed by either
a reference to a compile-time constant (such as `deprecated`) or
a call to a constant constructor.

用元数据给代码附加静态信息。
元数据注解以 `@` 开头，后面跟编译期常量的引用（例如 `deprecated`），
或者常量构造函数的调用。

Metadata can be attached to most Dart program constructs by
adding annotations before the construct's declaration or directive.

把注解写在声明或指令前面，就可以把元数据加到大多数 Dart 程序结构上。

## Built-in annotations

## 内置注解 {:#1}

The following annotations are available to all Dart code:

下列注解在所有 Dart 代码里都可以用：

[`@Deprecated`][]
<br> Marks a declaration as deprecated,
  indicating it should be migrated away from,
  with a message explaining the replacement and potential removal date.

  In addition to the general `@Deprecated` annotation, 
  you can use specific annotations to deprecate certain 
  usages of a declaration:

  * [`@Deprecated.extend()`][]: Extending the class is deprecated.

    [`@Deprecated.extend()`][]：扩展这个类已弃用。
    
  * [`@Deprecated.implement()`][]: Implementing the class or 
    mixin is deprecated.

    [`@Deprecated.implement()`][]：实现这个类或 mixin 已弃用。
    
  * [`@Deprecated.subclass()`][]: Subclassing (extending or 
    implementing) the class or mixin is deprecated.

    [`@Deprecated.subclass()`][]：子类化（扩展或实现）这个类或 mixin 已弃用。
    
  * [`@Deprecated.mixin()`][]: Mixing in the class is deprecated.

    [`@Deprecated.mixin()`][]：把这个类当作 mixin 已弃用。
    
  * [`@Deprecated.instantiate()`][]: Instantiating the class is deprecated.

    [`@Deprecated.instantiate()`][]：实例化这个类已弃用。
    
  * [`@Deprecated.optional()`][]: Omitting an argument for 
    the parameter is deprecated.

    [`@Deprecated.optional()`][]：省略该参数的实参已弃用。
    

  Here's an example of using the `@Deprecated` annotation:

  下面是 `@Deprecated` 注解的例子：

  <?code-excerpt "misc/lib/language_tour/metadata/television.dart (deprecated)"?>
  ```dart highlightLines=3
  class Television {
    /// Use [turnOn] to turn the power on instead.
    @Deprecated('Use turnOn instead')
    void activate() {
      turnOn();
    }
  
    /// Turns the TV's power on.
    void turnOn() {
      // ···
    }
    // ···
  }
  ```

<br> 把声明标为已弃用，表示应当迁移走，
  并用消息说明替代方案和可能的移除日期。

  除了通用的 `@Deprecated` 注解，
  还可以用更具体的注解弃用声明的某些用法。

[`@deprecated`][]
<br> Marks a declaration as deprecated until an unspecified future release.
  Prefer using `@Deprecated` and [providing a deprecation message][].

<br> 把声明标为已弃用，直到某个未指定的未来版本。
  更推荐使用 `@Deprecated` 并[提供弃用说明][providing a deprecation message]。

[`@override`][]
<br> Marks an instance member as an override or implementation of
  a member with the same name from a parent class or interface.
  For examples of using `@override`, check out [Extend a class][].

<br> 把实例成员标为对父类或接口中同名成员的覆盖或实现。
  `@override` 的例子见[扩展类][Extend a class]。

[`@pragma`][]
<br> Provides specific instructions or hints about a declaration to
  Dart tools, such as the compiler or analyzer.

<br> 向 Dart 工具（例如编译器或分析器）提供关于某个声明的具体指示或提示。

The [Dart analyzer][] provides feedback as diagnostics if
the `@override` annotation is needed and when using
members annotated with `@deprecated` or `@Deprecated`.

需要 `@override` 注解时，以及使用标了 `@deprecated` 或 `@Deprecated` 的成员时，
[Dart 分析器][Dart analyzer] 会以诊断的形式给出反馈。

[`@Deprecated`]: {{site.dart-api}}/dart-core/Deprecated-class.html
[`@deprecated`]: {{site.dart-api}}/dart-core/deprecated-constant.html
[`@override`]: {{site.dart-api}}/dart-core/override-constant.html
[`@pragma`]: {{site.dart-api}}/dart-core/pragma-class.html
[providing a deprecation message]: /tools/linter-rules/provide_deprecation_message
[Extend a class]: /language/extend
[Dart analyzer]: /tools/analysis
[`@Deprecated.extend()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.extend.html
[`@Deprecated.implement()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.implement.html
[`@Deprecated.subclass()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.subclass.html
[`@Deprecated.mixin()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.mixin.html
[`@Deprecated.instantiate()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.instantiate.html
[`@Deprecated.optional()`]: {{site.dart-api}}/dart-core/Deprecated/Deprecated.optional.html

## Analyzer-supported annotations

## 分析器支持的注解 {:#2}

Beyond providing support and analysis for the [built-in annotations][],
the [Dart analyzer][] provides additional support and diagnostics for
a variety of annotations from [`package:meta`][].
Some commonly used annotations the package provides include:

除了支持和分析[内置注解][built-in annotations]，
[Dart 分析器][Dart analyzer] 还对 [`package:meta`][] 里的多种注解提供额外支持和诊断。
这个包里常用的注解包括：

[`@visibleForTesting`][]
<br> Marks a member of a package as only public so that
  the member can be accessed from the package's tests.
  The analyzer hides the member from autocompletion suggestions
  and warns if it's used from another package.

<br> 把 package 的成员标为仅因测试需要而公开，
  这样 package 的测试才能访问该成员。
  分析器会从自动补全建议里隐藏该成员，
  如果其他 package 使用了它，则会警告。

[`@awaitNotRequired`][]
<br> Marks variables that have a `Future` type or functions that return a `Future`
  as not requiring the caller to await the `Future`.
  This stops the analyzer from warning callers that don't await the `Future`
  due to the [`discarded_futures`][] or [`unawaited_futures`][] lints.

<br> 把类型为 `Future` 的变量，或返回 `Future` 的函数，标为调用方不必 await 这个 `Future`。
  这样分析器就不会因为 [`discarded_futures`][] 或 [`unawaited_futures`][] lint，
  警告没有 await 这个 `Future` 的调用方。

To learn more about these and the other annotations the package provides,
what they indicate, what functionality they enable, and how to use them,
check out the [`package:meta/meta.dart` API docs][meta-api].

这些注解以及该包提供的其他注解分别表示什么、能启用什么功能、怎么用，
见 [`package:meta/meta.dart` API 文档][meta-api]。

[built-in annotations]: #built-in-annotations
[Dart analyzer]: /tools/analysis
[`@visibleForTesting`]: {{site.pub-api}}/meta/latest/meta/visibleForTesting-constant.html
[`@awaitNotRequired`]: {{site.pub-api}}/meta/latest/meta/awaitNotRequired-constant.html
[`discarded_futures`]: /tools/linter-rules/discarded_futures
[`unawaited_futures`]: /tools/linter-rules/unawaited_futures
[meta-api]: {{site.pub-api}}/meta/latest/meta/meta-library.html

## Custom annotations

## 自定义注解 {:#3}

You can define your own metadata annotations. Here's an example of
defining a `@Todo` annotation that takes two arguments:

可以定义自己的元数据注解。下面定义一个接收两个参数的 `@Todo` 注解：

<?code-excerpt "misc/lib/language_tour/metadata/todo.dart (definition)"?>
```dart
class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

And here's an example of using that `@Todo` annotation:

使用这个 `@Todo` 注解的例子：

<?code-excerpt "misc/lib/language_tour/metadata/misc.dart (usage)"?>
```dart highlightLines=1
@Todo('Dash', 'Implement this function')
void doSomething() {
  print('Do something');
}
```

### Specifying supported targets {:.no_toc}

### 指定支持的目标 {:#4}

To indicate the type of language constructs that
should be annotated with your annotation,
use the [`@Target`][] annotation from [`package:meta`][].

要标明你的注解应该用在哪种语言结构上，
使用 [`package:meta`][] 里的 [`@Target`][] 注解。

For example, if you wanted the earlier `@Todo` annotation to
only be allowed on functions and methods,
you'd add the following annotation:

例如，如果希望前面的 `@Todo` 注解只允许用在函数和方法上，
加上下面这个注解：

<?code-excerpt "misc/lib/language_tour/metadata/todo.dart (target-kinds)"?>
```dart highlightLines=3
import 'package:meta/meta_meta.dart';

@Target({TargetKind.function, TargetKind.method})
class Todo {
  // ···
}
```

With this configuration, the analyzer will warn if `Todo` is used as
an annotation on any declaration besides a top-level function or method.

按这个配置，如果 `Todo` 用在顶层函数或方法以外的声明上，分析器会警告。

[`@Target`]: {{site.pub-api}}/meta/latest/meta_meta/Target-class.html
[`package:meta`]: {{site.pub-pkg}}/meta
