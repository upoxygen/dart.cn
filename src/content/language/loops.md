---
# title: Loops 
title: 循环
# description: Learn how to use loops to control the flow of your Dart code.
description: 了解如何用循环控制 Dart 代码的流程。
prevpage:
  url: /language/pattern-types
  # title: Pattern types
  title: Pattern types
nextpage:
  url: /language/branches
  # title: Branches
  title: Branches
---

This page shows how you can control the flow of your Dart code using loops and
supporting statements:

本页说明如何用循环及相关语句控制 Dart 代码的流程：

-   `for` loops

    `for` 循环
    
-   `while` and `do while` loops

    `while` 和 `do while` 循环
    
-   `break` and `continue`

    `break` 和 `continue`
    

You can also manipulate control flow in Dart using:

还可以用下面这些方式控制 Dart 的流程：

- [Branching][], like `if` and `switch`

  [分支][Branching]，例如 `if` 和 `switch`
  
- [Exceptions][], like `try`, `catch`, and `throw`

  [异常][Exceptions]，例如 `try`、`catch` 和 `throw`
  

## For loops

## For 循环 {:#1}

You can iterate with the standard `for` loop. For example:

可以用标准 `for` 循环迭代。例如：

<?code-excerpt "language/test/control_flow/loops_test.dart (for)"?>
```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```

Closures inside of Dart's `for` loops capture the _value_ of the index.
This avoids a common pitfall found in JavaScript. For example, consider:

Dart 的 `for` 循环里的闭包捕获的是索引的 _值_。
这避免了 JavaScript 里的一个常见陷阱。例如：

<?code-excerpt "language/test/control_flow/loops_test.dart (for-and-closures)"?>
```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}

for (final c in callbacks) {
  c();
}
```

The output is `0` and then `1`, as expected. In contrast, the example
would print `2` and then `2` in JavaScript.

输出是 `0` 然后 `1`，符合预期。相比之下，这个例子在 JavaScript 里会打印 `2` 然后 `2`。

Sometimes you might not need to know the current iteration counter
when iterating over an [`Iterable`][] type, like `List` or `Set`.
In that case, use the `for-in` loop for cleaner code:

遍历 [`Iterable`][] 类型（例如 `List` 或 `Set`）时，有时不需要知道当前迭代计数。
这时用 `for-in` 循环，代码更干净：

<?code-excerpt "language/lib/control_flow/loops.dart (collection)"?>
```dart
for (var candidate in candidates) {
  candidate.interview();
}
```

In the previous example loop, `candidate` is
defined within the loop body and
set to reference one value from `candidates` at a time.
`candidate` is a local [variable][].
Reassigning `candidate` inside the loop body only
changes the local variable for that iteration and
doesn't modify the original `candidates` iterable.

上面的循环里，`candidate` 定义在循环体内，
每次引用 `candidates` 中的一个值。
`candidate` 是局部[变量][variable]。
在循环体内重新赋值 `candidate` 只改变该次迭代的局部变量，
不会修改原来的 `candidates` 可迭代对象。

To process the values obtained from the iterable, 
you can also use a [pattern][] in a `for-in` loop:

处理从可迭代对象得到的值时，也可以在 `for-in` 循环里使用[模式][pattern]：

<?code-excerpt "language/lib/control_flow/loops.dart (collection-for-pattern)"?>
```dart
for (final Candidate(:name, :yearsExperience) in candidates) {
  print('$name has $yearsExperience of experience.');
}
```

:::tip
To practice using `for-in`, follow the
[Iterable collections tutorial](/libraries/collections/iterables).

练习 `for-in`，可以看 [Iterable 集合教程](/libraries/collections/iterables)。
:::

Iterable classes also have a [forEach()][] method as another option:

Iterable 类还有 [forEach()][] 方法作为另一种选择：

<?code-excerpt "language/test/control_flow/loops_test.dart (for-each)"?>
```dart
var collection = [1, 2, 3];
collection.forEach(print); // 1 2 3
```

[variable]: /language/variables

## While and do-while

## While 和 do-while {:#2}

A `while` loop evaluates the condition before the loop:

`while` 循环在循环前求值条件：

<?code-excerpt "language/lib/control_flow/loops.dart (while)"?>
```dart
while (!isDone()) {
  doSomething();
}
```

A `do`-`while` loop evaluates the condition *after* the loop:

`do`-`while` 循环在循环 *之后* 求值条件：

<?code-excerpt "language/lib/control_flow/loops.dart (do-while)"?>
```dart
do {
  printLine();
} while (!atEndOfPage());
```


## Break and continue

## Break 和 continue {:#3}

Use `break` to stop looping:

用 `break` 停止循环：

<?code-excerpt "language/lib/control_flow/loops.dart (while-break)"?>
```dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

Use `continue` to skip to the next loop iteration:

用 `continue` 跳到下一次循环迭代：

<?code-excerpt "language/lib/control_flow/loops.dart (for-continue)"?>
```dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

If you're using an [`Iterable`][] such as a list or set,
how you write the previous example might differ:

如果用的是列表或集合这类 [`Iterable`][]，上面的例子可以写成另一种样子：

<?code-excerpt "language/lib/control_flow/loops.dart (where)"?>
```dart
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
```

## Labels

## 标签 {:#4}

A label is an identifier followed by a colon (`labelName:`)
that you can place before a statement to create a
_labeled statement_. Loops and switch cases are often used as
labeled statements. A labeled statement can be referenced later
in a `break` or `continue` statement as follows:

标签是标识符后面跟一个冒号（`labelName:`），
放在语句前面，构成 _带标签的语句_。循环和 switch 分支常被当作带标签的语句。
之后可以在 `break` 或 `continue` 语句里引用带标签的语句，方式如下：

* `break labelName;`
  Terminates the execution of the labeled statement.
  This is useful for breaking out of a specific outer loop when you're
  within a nested loop.

  `break labelName;`
  终止带标签语句的执行。
  在嵌套循环里要跳出某个指定的外层循环时很有用。
  
* `continue labelName;`
  Skips the rest of the current iteration of the
  labeled statement loop and continues with the next iteration.

  `continue labelName;`
  跳过带标签循环当前迭代的剩余部分，继续下一次迭代。
  

Labels are used to manage control flow. They are often used with
loops and switch cases and allow you to specify which statement to
break out of or continue, rather than affecting the innermost
loop by default.

标签用来管理控制流。它们常和循环、switch 分支一起用，
让你指定要跳出或继续的是哪条语句，而不是默认影响最内层循环。

### Labels in for loop using `break` {:.no_toc}

### 在 for 循环中用 `break` 使用标签 {:#5}

The following code demonstrates the usage of a label called `outerLoop`
in a  `for` loop with a `break` statement:

下面的代码演示在 `for` 循环里用名为 `outerLoop` 的标签配合 `break` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-for-loop-break)"?>
```dart
outerLoop:
for (var i = 1; i <= 3; i++) {
  for (var j = 1; j <= 3; j++) {
    print('i = $i, j = $j');
    if (i == 2 && j == 2) {
      break outerLoop;
    }
  }
}
print('outerLoop exited');
```

In the previous example, when `i == 2` and `j == 2`, the `break outerLoop;`
statement stops both inner and outer loops. As a result, the output is:

上面的例子里，当 `i == 2` 且 `j == 2` 时，`break outerLoop;`
会同时停止内层和外层循环。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 2, j = 2
outerLoop exited
```

### Labels in for loop using `continue` {:.no_toc}

### 在 for 循环中用 `continue` 使用标签 {:#6}

The following code demonstrates the use of a label called `outerLoop`
in a  `for` loop with a `continue` statement:

下面的代码演示在 `for` 循环里用名为 `outerLoop` 的标签配合 `continue` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-for-loop-continue)"?>
```dart
outerLoop:
for (var i = 1; i <= 3; i++) {
  for (var j = 1; j <= 3; j++) {
    if (i == 2 && j == 2) {
      continue outerLoop;
    }
    print('i = $i, j = $j');
  }
}
```

In the previous example, when `i == 2` and `j == 2`, `continue outerLoop;` skips the
rest of the iterations for `i = 2` and moves to `i = 3`. As a result, the output is:

上面的例子里，当 `i == 2` 且 `j == 2` 时，`continue outerLoop;` 会跳过
`i = 2` 剩余的迭代，转到 `i = 3`。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

### Labels in while loop using `break` {:.no_toc}

### 在 while 循环中用 `break` 使用标签 {:#7}

The following code demonstrates the use of a label called `outerLoop` in
a `while` loop with a `break` statement:

下面的代码演示在 `while` 循环里用名为 `outerLoop` 的标签配合 `break` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-while-loop-break)"?>
```dart
var i = 1;

outerLoop:
while (i <= 3) {
  var j = 1;
  while (j <= 3) {
    print('i = $i, j = $j');
    if (i == 2 && j == 2) {
      break outerLoop;
    }
    j++;
  }
  i++;
}
print('outerLoop exited');
```

In the previous example, the program breaks out of both inner and outer `while` loops
when `i == 2` and `j == 2`. As a result, the output is:

上面的例子里，当 `i == 2` 且 `j == 2` 时，程序会跳出内层和外层 `while` 循环。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 2, j = 2
outerLoop exited
```

### Labels in while loop using `continue` {:.no_toc}

### 在 while 循环中用 `continue` 使用标签 {:#8}

The following code demonstrates the use of a label called `outerLoop` in
a `while` loop with a `continue` statement:

下面的代码演示在 `while` 循环里用名为 `outerLoop` 的标签配合 `continue` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-while-loop-continue)"?>
```dart
var i = 1;

outerLoop:
while (i <= 3) {
  var j = 1;
  while (j <= 3) {
    if (i == 2 && j == 2) {
      i++;
      continue outerLoop;
    }
    print('i = $i, j = $j');
    j++;
  }
  i++;
}
```

In the previous example, the iteration for `i = 2` and `j = 2` is skipped and the loop moves
directly to `i = 3`. As a result, the output is:

上面的例子里，`i = 2` 且 `j = 2` 的这次迭代被跳过，循环直接转到 `i = 3`。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

### Labels in do-while loop using `break` {:.no_toc}

### 在 do-while 循环中用 `break` 使用标签 {:#9}

The following code demonstrates the use of a label called `outerLoop` in
a `do while` loop with a `break` statement:

下面的代码演示在 `do while` 循环里用名为 `outerLoop` 的标签配合 `break` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-do-while-loop-break)"?>
```dart
var i = 1;

outerLoop:
do {
  var j = 1;
  do {
    print('i = $i, j = $j');
    if (i == 2 && j == 2) {
      break outerLoop;
    }
    j++;
  } while (j <= 3);
  i++;
} while (i <= 3);

print('outerLoop exited');
```

In the previous example, the program breaks out of both inner and outer loops when `i == 2` and
`j == 2`. As a result, the output is:

上面的例子里，当 `i == 2` 且 `j == 2` 时，程序会跳出内层和外层循环。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 2, j = 2
outerLoop exited
```

### Labels in do-while loop using `continue` {:.no_toc}

### 在 do-while 循环中用 `continue` 使用标签 {:#10}

The following code demonstrates the use of a label called `outerLoop` in
a `do while` loop with a `continue` statement:

下面的代码演示在 `do while` 循环里用名为 `outerLoop` 的标签配合 `continue` 语句：

<?code-excerpt "language/lib/control_flow/loops.dart (label-do-while-loop-continue)"?>
```dart
var i = 1;

outerLoop:
do {
  var j = 1;
  do {
    if (i == 2 && j == 2) {
      i++;
      continue outerLoop;
    }
    print('i = $i, j = $j');
    j++;
  } while (j <= 3);
  i++;
} while (i <= 3);
```

In the previous example, the loop skips `i = 2` and `j = 2` and moves directly to `i = 3`.
As a result, the output is:

上面的例子里，循环跳过 `i = 2` 和 `j = 2`，直接转到 `i = 3`。因此输出是：

```plaintext
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

[exceptions]: /language/error-handling
[branching]: /language/branches
[iteration]: /libraries/dart-core#iteration
[forEach()]: {{site.dart-api}}/dart-core/Iterable/forEach.html
[`Iterable`]: {{site.dart-api}}/dart-core/Iterable-class.html
[pattern]: /language/patterns
