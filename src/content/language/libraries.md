---
# title: Libraries & imports
title: 库与导入
shortTitle: 库
# description: Guidance on importing and implementing libraries.
description: 关于导入和实现库的说明。
prevpage:
  url: /language/metadata
  # title: Metadata
  title: 元数据
nextpage:
  url: /language/classes
  # title: Classes
  title: Classes
---

The `import` and `library` directives can help you create a
modular and shareable code base. Libraries not only provide APIs, but
are a unit of privacy: identifiers that start with an underscore (`_`)
are visible only inside the library. *Every Dart file (plus its parts) is a
[library][]*, even if it doesn't use a [`library`](#library-directive) directive.

`import` 和 `library` 指令帮你做出模块化、可共享的代码库。
库不只提供 API，也是隐私的单位：以下划线（`_`）开头的标识符只在库内可见。
*每个 Dart 文件（加上它的 part）都是一个[库][library]*，即使没有使用 [`library`](#library-directive) 指令。

Libraries can be distributed using [packages](/tools/pub/packages).

库可以通过 [package](/tools/pub/packages) 分发。

Dart uses underscores instead of access modifier keywords
like `public`, `protected`, or `private`.
While access modifier keywords from other languages
provide more fine-grained control,
Dart's use of underscores and library-based privacy
provides a straightforward configuration mechanism,
helps enable an efficient implementation of [dynamic access][],
and improves tree shaking (dead code elimination).

Dart 用下划线代替 `public`、`protected`、`private` 这类访问修饰关键字。
其他语言的访问修饰关键字能做更细的控制，
而 Dart 用下划线和基于库的隐私，配置更直接，
也便于高效实现[动态访问][dynamic access]，
并改善 tree shaking（死代码消除）。

[library]: /resources/glossary#library
[dynamic access]: /effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking

## Using libraries

## 使用库 {:#1}

Use `import` to specify how a namespace from one library is used in the
scope of another library.

用 `import` 指定一个库的命名空间如何在另一个库的作用域里使用。

For example, Dart web apps generally use the [`dart:js_interop`][]
library, which they can import like this:

例如，Dart Web 应用通常使用 [`dart:js_interop`][] 库，可以这样导入：

<?code-excerpt "misc/test/language_tour/browser_test.dart (dart-js-interop-import)"?>
```dart
import 'dart:js_interop';
```

The only required argument to `import` is a URI specifying the
library.
For built-in libraries, the URI has the special `dart:` scheme.
For other libraries, you can use a file system path or the `package:`
scheme. The `package:` scheme specifies libraries provided by a package
manager such as the pub tool. For example:

`import` 唯一必需的参数是指定库的 URI。
内置库的 URI 使用特殊的 `dart:` 方案。
其他库可以用文件系统路径或 `package:` 方案。
`package:` 方案指定由包管理器（例如 pub）提供的库。例如：

<?code-excerpt "misc/test/language_tour/browser_test.dart (package-import)"?>
```dart
import 'package:test/test.dart';
```

:::note
*URI* stands for uniform resource identifier.
*URLs* (uniform resource locators) are a common kind of URI.

*URI* 是统一资源标识符。
*URL*（统一资源定位符）是一种常见的 URI。
:::

### Specifying a library prefix

### 指定库前缀 {:#2}

If you import two libraries that have conflicting identifiers, then you
can specify a prefix for one or both libraries. For example, if library1
and library2 both have an Element class, then you might have code like
this:

如果导入的两个库有冲突的标识符，可以为其中一个或两个指定前缀。
例如 library1 和 library2 都有 Element 类，代码可以写成：

<?code-excerpt "misc/lib/language_tour/libraries/import_as.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```

Import prefixes with the [wildcard][] name `_` are non-binding,
but will provide access to the non-private extensions in that library.

用[通配符][wildcard]名字 `_` 做导入前缀不会绑定名字，
但可以访问该库中的非私有扩展。

[wildcard]: /language/variables#wildcard-variables

### Importing only part of a library

### 只导入库的一部分 {:#3}

If you want to use only part of a library, you can selectively import
the library. For example:

如果只想用库的一部分，可以选择性导入。例如：

<?code-excerpt "misc/lib/language_tour/libraries/show_hide.dart (imports)" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

#### Lazily loading a library {:#lazily-loading-a-library}

#### 惰性加载库 {:#4}

*Deferred loading* (also called *lazy loading*)
allows a web app to load a library on demand,
if and when the library is needed.
Use deferred loading when you want to meet one or more of the following needs.

*延迟加载*（也叫 *惰性加载*）让 Web 应用在需要时才加载库。
想满足下面一项或多项需求时，可以使用延迟加载。

* Reduce a web app's initial startup time.

  缩短 Web 应用的初始启动时间。
  
* Perform A/B testing—trying out
  alternative implementations of an algorithm, for example.

  做 A/B 测试，例如尝试某个算法的另一种实现。
  
* Load rarely used functionality, such as optional screens and dialogs.

  加载很少用到的功能，例如可选的页面和对话框。
  

That doesn't mean Dart loads all the deferred components at start time.
The web app can download deferred components via the web when needed.

这并不表示 Dart 会在启动时加载所有延迟组件。
Web 应用可以在需要时通过网络下载延迟组件。

The `dart` tool doesn't support deferred loading for targets other than web.
If you're building a Flutter app,
consult its implementation of deferred loading in the Flutter guide on
[deferred components][flutter-deferred].

`dart` 工具除了 Web 以外的目标不支持延迟加载。
如果在做 Flutter 应用，
请看 Flutter 指南里关于[延迟组件][flutter-deferred]的实现。

[flutter-deferred]: {{site.flutter-docs}}/perf/deferred-components

To lazily load a library, first import it using `deferred as`.

要惰性加载库，先用 `deferred as` 导入它。

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (import)" replace="/hello\.dart/package:greetings\/$&/g"?>
```dart
import 'package:greetings/hello.dart' deferred as hello;
```

When you need the library, invoke
`loadLibrary()` using the library's identifier.

需要这个库时，用库的标识符调用 `loadLibrary()`。

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (load-library)"?>
```dart
Future<void> greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

In the preceding code,
the `await` keyword pauses execution until the library is loaded.
For more information about `async` and `await`,
check out [asynchronous programming](/language/async).

上面的代码里，`await` 会暂停执行，直到库加载完成。
关于 `async` 和 `await`，见[异步编程](/language/async)。

You can invoke `loadLibrary()` multiple times on a library without problems.
The library is loaded only once.

对同一个库多次调用 `loadLibrary()` 没有问题。
库只会加载一次。

Keep in mind the following when you use deferred loading:

使用延迟加载时注意：

* A deferred library's constants aren't constants in the importing file.
  Remember, these constants don't exist until the deferred library is loaded.

  延迟库里的常量，在导入文件里不是常量。
  这些常量要等延迟库加载后才存在。
  
* You can't use types from a deferred library in the importing file.
  Instead, consider moving interface types to a library imported by
  both the deferred library and the importing file.

  导入文件里不能使用延迟库中的类型。
  可以把接口类型挪到一个库里，让延迟库和导入文件都导入它。
  
* Dart implicitly inserts `loadLibrary()` into the namespace that you define
  using <code>deferred as <em>namespace</em></code>.
  The `loadLibrary()` function returns
  a [`Future`](/libraries/dart-async#future).

  Dart 会把 `loadLibrary()` 隐式放进你用 <code>deferred as <em>namespace</em></code> 定义的命名空间。
  `loadLibrary()` 返回一个 [`Future`](/libraries/dart-async#future)。
  

### The `library` directive {:#library-directive}

### `library` 指令 {:#5}

To specify library-level [doc comments][] or [metadata annotations][],
attach them to a `library` declaration at the start of the file.

要写库级别的[文档注释][doc comments]或[元数据注解][metadata annotations]，
把它们加在文件开头的 `library` 声明上。

<?code-excerpt "misc/lib/effective_dart/docs_good.dart (library-doc)"?>
```dart
/// A really great test library.
@TestOn('browser')
library;
```

## Implementing libraries

## 实现库 {:#6}

See
[Create Packages](/tools/pub/create-packages)
for advice on how to implement a package, including:

如何实现 package，见
[创建 Package](/tools/pub/create-packages)，包括：

* How to organize library source code.

  如何组织库的源码。
  
* How to use the `export` directive.

  如何使用 `export` 指令。
  
* When to use the `part` directive.

  何时使用 `part` 指令。
  
* How to use conditional imports and exports to implement
  a library that supports multiple platforms.

  如何用条件导入和导出，实现支持多个平台的库。
  

[`dart:js_interop`]: {{site.dart-api}}/dart-js_interop/dart-js_interop-library.html
[doc comments]: /effective-dart/documentation#consider-writing-a-library-level-doc-comment
[metadata annotations]: /language/metadata
