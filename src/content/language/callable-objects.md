---
# title: Callable objects
title: 可调用对象
# description: Learn how to create and use callable objects in Dart.
description: 了解如何在 Dart 中创建和使用可调用对象。
showToc: false
prevpage:
  url: /language/extension-types
  # title: Extension types
  title: Extension types
nextpage:
  url: /language/class-modifiers
  # title: Class modifiers
  title: Class modifiers
---

<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore: (stable|beta|dev)[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore: (stable|beta|dev)[^\n]+\n/$1\n/g; /. • (lib|test)\/\w+\.dart:\d+:\d+//g"?>

To allow an instance of your Dart class to be called like a function,
implement the `call()` method.

要让 Dart 类的实例能像函数一样被调用，
实现 `call()` 方法。

The `call()` method allows an instance of any class that defines it to emulate a function.
This method supports the same functionality as normal [functions][]
such as parameters and return types.

定义了 `call()` 的类，其实例可以模拟函数。
这个方法和普通[函数][functions]一样，支持参数和返回类型。

In the following example, the `WannabeFunction` class defines a `call()` function
that takes three strings and concatenates them, separating each with a space,
and appending an exclamation. Click **Run** to execute the code.

下面的例子里，`WannabeFunction` 定义了 `call()`，
接收三个字符串，用空格拼起来，并在末尾加一个感叹号。点击 **Run** 运行代码。

<?code-excerpt "misc/lib/language_tour/callable_objects.dart"?>
```dartpad
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');

void main() => print(out);
```

[functions]: /language/functions
