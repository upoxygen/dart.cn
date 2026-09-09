---
# title: Asynchronous programming
title: 异步编程
# description: Information on writing asynchronous code in Dart.
description: 关于在 Dart 中编写异步代码的介绍。
shortTitle: 异步编程
prevpage:
  url: /language/concurrency
  # title: Concurrency
  title: Concurrency
nextpage:
  url: /language/isolates
  # title: Isolates
  title: Isolates
---

<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g; / *\/\/\s+ignore:[^\n]+//g; /([A-Z]\w*)\d\b/$1/g"?>

Dart libraries are full of functions that
return [`Future`][] or [`Stream`][] objects.
These functions are _asynchronous_:
they return after setting up
a possibly time-consuming operation
(such as I/O),
without waiting for that operation to complete.

Dart 库里有很多函数返回 [`Future`][] 或 [`Stream`][] 对象。
这些函数是 _异步_ 的：
它们在搭好一个可能耗时的操作（例如 I/O）之后就返回，
不等那个操作完成。

The `async` and `await` keywords support asynchronous programming,
letting you write asynchronous code that
looks similar to synchronous code.

`async` 和 `await` 关键字支持异步编程，
让你写出看起来很像同步代码的异步代码。


## Handling Futures

## 处理 Future {:#1}

When you need the result of a completed Future,
you have two options:

需要已完成 Future 的结果时，有两种做法：

* Use `async` and `await`, as described here and in the
  [asynchronous programming tutorial](/libraries/async/async-await).

  使用 `async` 和 `await`，本文以及[异步编程教程](/libraries/async/async-await)里都有说明。
  
* Use the Future API, as described in the
  [`dart:async` documentation](/libraries/dart-async#future).

  使用 Future API，见 [`dart:async` 文档](/libraries/dart-async#future)。
  

Code that uses `async` and `await` is asynchronous,
but it looks a lot like synchronous code.
For example, here's some code that uses `await`
to wait for the result of an asynchronous function:

使用 `async` 和 `await` 的代码是异步的，但看起来很像同步代码。
例如，下面用 `await` 等待异步函数的结果：

<?code-excerpt "misc/lib/language_tour/async.dart (await-look-up-version)"?>
```dart
await lookUpVersion();
```

To use `await`, code must be in an `async` function—a
function marked as `async`:

要使用 `await`，代码必须在 `async` 函数里，也就是标了 `async` 的函数：

<?code-excerpt "misc/lib/language_tour/async.dart (checkVersion)" replace="/async|await/[!$&!]/g"?>
```dart
Future<void> checkVersion() [!async!] {
  var version = [!await!] lookUpVersion();
  // Do something with version
}
```

:::note
Although an `async` function might perform time-consuming operations, 
it doesn't wait for those operations. 
Instead, the `async` function executes only
until it encounters its first `await` expression.
Then it returns a `Future` object,
resuming execution only after the `await` expression completes.

虽然 `async` 函数可能执行耗时操作，
但它不会等待那些操作。
`async` 函数只执行到第一个 `await` 表达式，
然后返回一个 `Future` 对象，
等到 `await` 表达式完成后再继续执行。
:::

Use `try`, `catch`, and `finally` to handle errors and cleanup
in code that uses `await`:

在使用 `await` 的代码里，用 `try`、`catch` 和 `finally` 处理错误和清理：

<?code-excerpt "misc/lib/language_tour/async.dart (try-catch)"?>
```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```

You can use `await` multiple times in an `async` function.
For example, the following code waits three times
for the results of functions:

一个 `async` 函数里可以多次使用 `await`。
例如，下面的代码三次等待函数结果：

<?code-excerpt "misc/lib/language_tour/async.dart (repeated-await)"?>
```dart
var entrypoint = await findEntryPoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

In <code>await <em>expression</em></code>,
the value of <code><em>expression</em></code> is usually a Future;
if it isn't, then the value is automatically wrapped in a Future.
This Future object indicates a promise to return an object.
The value of <code>await <em>expression</em></code> is that returned object.
The await expression makes execution pause until that object is available.

在 <code>await <em>expression</em></code> 里，
<code><em>expression</em></code> 的值通常是 Future；
如果不是，这个值会自动包进一个 Future。
这个 Future 对象表示承诺返回一个对象。
<code>await <em>expression</em></code> 的值就是那个返回的对象。
await 表达式会让执行暂停，直到那个对象可用。

**If you get a compile-time error when using `await`,
make sure `await` is in an `async` function.**
For example, to use `await` in your app's `main()` function,
the body of `main()` must be marked as `async`:

**使用 `await` 时如果出现编译期错误，确认 `await` 在 `async` 函数里。**
例如，要在应用的 `main()` 里使用 `await`，`main()` 的函数体必须标为 `async`：

<?code-excerpt "misc/lib/language_tour/async.dart (main)" replace="/async|await/[!$&!]/g"?>
```dart
void main() [!async!] {
  checkVersion();
  print('In main: version is ${[!await!] lookUpVersion()}');
}
```

:::note
The preceding example uses an `async` function (`checkVersion()`)
without waiting for a result—a practice that can cause problems
if the code assumes that the function has finished executing.
To avoid this problem,
use the [unawaited_futures linter rule][].

上面的例子调用了 `async` 函数（`checkVersion()`）却没有等待结果。
如果代码假定这个函数已经执行完，就会出问题。
要避免这个问题，使用 [unawaited_futures lint 规则][unawaited_futures linter rule]。
:::

For an interactive introduction to using futures, `async`, and `await`,
see the [asynchronous programming tutorial](/libraries/async/async-await).

关于 futures、`async` 和 `await` 的交互式入门，见[异步编程教程](/libraries/async/async-await)。


## Declaring async functions

## 声明 async 函数 {:#2}

An `async` function is a function whose body is marked with
the `async` modifier.

`async` 函数是函数体标了 `async` 修饰符的函数。

Adding the `async` keyword to a function makes it return a Future.
For example, consider this synchronous function,
which returns a String:

给函数加上 `async` 关键字，会让它返回 Future。
例如，考虑这个返回 String 的同步函数：

<?code-excerpt "misc/lib/language_tour/async.dart (sync-look-up-version)"?>
```dart
String lookUpVersion() => '1.0.0';
```

If you change it to be an `async` function—for example,
because a future implementation will be time consuming—the
returned value is a Future:

如果改成 `async` 函数，例如因为以后的实现会很耗时，返回值就变成 Future：

<?code-excerpt "misc/lib/language_tour/async.dart (async-look-up-version)"?>
```dart
Future<String> lookUpVersion() async => '1.0.0';
```

Note that the function's body doesn't need to use the Future API.
Dart creates the Future object if necessary.
If your function doesn't return a useful value,
make its return type `Future<void>`.

注意，函数体不必使用 Future API。
必要时 Dart 会创建 Future 对象。
如果函数没有有用的返回值，把返回类型写成 `Future<void>`。

For an interactive introduction to using futures, `async`, and `await`,
see the [asynchronous programming tutorial](/libraries/async/async-await).

关于 futures、`async` 和 `await` 的交互式入门，见[异步编程教程](/libraries/async/async-await)。

{% comment %}
TODO #1117: Where else should we cover generalized void?
{% endcomment %}


## Handling Streams

## 处理 Stream {:#3}

When you need to get values from a Stream,
you have two options:

需要从 Stream 取值时，有两种做法：

* Use `async` and an _asynchronous for loop_ (`await for`).

  使用 `async` 和 _异步 for 循环_（`await for`）。
  
* Use the Stream API, as described in the
  [`dart:async` documentation](/libraries/dart-async#stream).

  使用 Stream API，见 [`dart:async` 文档](/libraries/dart-async#stream)。
  

:::note
Before using `await for`, be sure that it makes the code clearer and that you
really do want to wait for all of the stream's results. For example, you
usually should **not** use `await for` for UI event listeners, because UI
frameworks send endless streams of events.

使用 `await for` 之前，先确认它能让代码更清楚，并且你确实想等待流的全部结果。
例如，通常 **不要** 对 UI 事件监听器使用 `await for`，因为 UI 框架会发送无尽的事件流。
:::

An asynchronous for loop has the following form:

异步 for 循环的形式如下：

<?code-excerpt "misc/lib/language_tour/async.dart (await-for)"?>
```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

The value of <code><em>expression</em></code> must have type Stream.
Execution proceeds as follows:

<code><em>expression</em></code> 的值必须是 Stream 类型。
执行过程如下：

1. Wait until the stream emits a value.

   等待流发出一个值。
   
2. Execute the body of the for loop,
   with the variable set to that emitted value.

   执行 for 循环体，变量设为刚发出的那个值。
   
3. Repeat 1 and 2 until the stream is closed.

   重复 1 和 2，直到流关闭。
   

To stop listening to the stream,
you can use a `break` or `return` statement,
which breaks out of the for loop
and unsubscribes from the stream.

要停止监听流，可以使用 `break` 或 `return` 语句，
这会跳出 for 循环并取消对流的订阅。

**If you get a compile-time error when implementing an asynchronous for loop,
make sure the `await for` is in an `async` function.**
For example, to use an asynchronous for loop in your app's `main()` function,
the body of `main()` must be marked as `async`:

**实现异步 for 循环时如果出现编译期错误，确认 `await for` 在 `async` 函数里。**
例如，要在应用的 `main()` 里使用异步 for 循环，`main()` 的函数体必须标为 `async`：

<?code-excerpt "misc/lib/language_tour/async.dart (number-thinker)" replace="/async|await for/[!$&!]/g"?>
```dart
void main() [!async!] {
  // ...
  [!await for!] (final request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```

For more information about Dart's asynchronous programming support,
check out the [`dart:async`](/libraries/dart-async) library documentation.

关于 Dart 异步编程支持的更多信息，见 [`dart:async`](/libraries/dart-async) 库文档。

[`Future`]: {{site.dart-api}}/dart-async/Future-class.html
[`Stream`]: {{site.dart-api}}/dart-async/Stream-class.html
[unawaited_futures linter rule]: /tools/linter-rules/unawaited_futures
