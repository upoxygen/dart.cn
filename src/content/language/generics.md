---
# title: Generics
title: 泛型
# description: Learn about generic types in Dart.
description: 了解 Dart 中的泛型。
prevpage:
  url: /language/collections
  # title: Collections
  title: Collections
nextpage:
  url: /language/typedefs
  # title: Typedefs
  title: 类型别名
---

<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g; / *\/\/\s+ignore:[^\n]+//g; /([A-Z]\w*)\d\b/$1/g"?>

If you look at the API documentation for the basic array type,
[`List`][], you'll see that the
type is actually `List<E>`. The \<...\> notation marks List as a
*generic* (or *parameterized*) type—a type that has formal type
parameters. [By convention][], most type variables have single-letter names,
such as E, T, S, K, and V.

看基本数组类型 [`List`][] 的 API 文档，会发现类型实际上是 `List<E>`。
`\<...\>` 表示 List 是 *泛型*（或 *参数化*）类型，也就是带形式类型参数的类型。
[按惯例][By convention]，大多数类型变量用单字母命名，例如 E、T、S、K 和 V。

## Why use generics?

## 为什么使用泛型？ {:#1}

Generics are often required for type safety, but they have more benefits
than just allowing your code to run:

泛型常常是类型安全所必需的，但好处不只是让代码能跑起来：

* Properly specifying generic types results in better generated code.

  正确指定泛型类型，生成的代码更好。
  
* You can use generics to reduce code duplication.

  可以用泛型减少代码重复。
  

If you intend for a list to contain only strings, you can
declare it as `List<String>` (read that as "list of string"). That way
you, your fellow programmers, and your tools can detect that assigning a non-string to
the list is probably a mistake. Here's an example:

如果希望列表只包含字符串，可以声明为 `List<String>`（读作「字符串列表」）。
这样你、同事和工具都能发现，把非字符串放进列表多半是错的。例如：

```dart tag=fails-sa
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

Another reason for using generics is to reduce code duplication.
Generics let you share a single interface and implementation between
many types, while still taking advantage of static
analysis. For example, say you create an interface for
caching an object:

使用泛型的另一个原因是减少代码重复。
泛型让你在多种类型之间共享同一套接口和实现，同时仍能利用静态分析。
例如，假设你创建了一个缓存对象的接口：

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (object-cache)"?>
```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
```

You discover that you want a string-specific version of this interface,
so you create another interface:

你发现还想要一个专门处理字符串的版本，于是又创建一个接口：

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (string-cache)"?>
```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```

Later, you decide you want a number-specific version of this
interface... You get the idea.

后来你又想要一个专门处理数字的版本……明白了吧。

Generic types can save you the trouble of creating all these interfaces.
Instead, you can create a single interface that takes a type parameter:

泛型可以省掉创建所有这些接口的麻烦。
你可以创建一个带类型参数的接口：

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (cache)"?>
```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```

In this code, T is the stand-in type. It's a placeholder that you can
think of as a type that a developer will define later.

这段代码里，T 是占位类型。你可以把它看成开发者稍后会定义的类型。


## Using collection literals

## 使用集合字面量 {:#2}

List, set, and map literals can be parameterized. Parameterized literals are
just like the literals you've already seen, except that you add
<code>&lt;<em>type</em>></code> (for lists and sets) or
<code>&lt;<em>keyType</em>, <em>valueType</em>></code> (for maps)
before the opening bracket. Here is an example of using typed literals:

List、set 和 map 字面量都可以参数化。参数化字面量和你已经见过的字面量一样，
只是在左括号前加上 <code>&lt;<em>type</em>></code>（用于列表和集合）
或 <code>&lt;<em>keyType</em>, <em>valueType</em>></code>（用于映射）。
下面是使用带类型字面量的例子：

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (collection-literals)"?>
```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines',
};
```


## Using parameterized types with constructors

## 在构造函数中使用参数化类型 {:#3}

To specify one or more types when using a constructor, put the types in
angle brackets (`<...>`) just after the class name. For example:

使用构造函数时要指定一个或多个类型，把类型写在类名后面的尖括号（`<...>`）里。例如：

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-1)"?>
```dart
var nameSet = Set<String>.of(names);
```

The following code creates a `SplayTreeMap` that has
integer keys and values of type `View`:

下面的代码创建一个键为整数、值为 `View` 类型的 `SplayTreeMap`：

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-2)"?>
```dart
var views = SplayTreeMap<int, View>();
```


## Generic collections and the types they contain

## 泛型集合及其包含的类型 {:#4}

Dart generic types are *reified*, which means that they carry their type
information around at runtime. For example, you can test the type of a
collection:

Dart 的泛型类型是 *具体化* 的，也就是运行时仍携带类型信息。
例如可以检查集合的类型：

<?code-excerpt "misc/test/language_tour/generics_test.dart (generic-collections)"?>
```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

:::note
In contrast, generics in Java use *erasure*, which means that generic
type parameters are removed at runtime. In Java, you can test whether
an object is a List, but you can't test whether it's a `List<String>`.

相比之下，Java 的泛型使用 *擦除*，也就是运行时会去掉泛型类型参数。
在 Java 里可以检查对象是不是 List，但不能检查它是不是 `List<String>`。
:::


## Restricting the parameterized type

## 限制参数化类型 {:#5}

When implementing a generic type,
you might want to limit the types that can be provided as arguments,
so that the argument must be a subtype of a particular type.
This restriction is called a bound.
You can do this using `extends`.

实现泛型类型时，可能想限制可作为实参的类型，
使实参必须是某个类型的子类型。
这种限制叫做边界。可以用 `extends` 来做。

A common use case is ensuring that a type is non-nullable
by making it a subtype of `Object`
(instead of the default, [`Object?`][top-and-bottom]).

常见用法是让类型成为 `Object` 的子类型
（而不是默认的 [`Object?`][top-and-bottom]），从而保证它非空。

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (non-nullable)"?>
```dart
class Foo<T extends Object> {
  // Any type provided to Foo for T must be non-nullable.
}
```

You can use `extends` with other types besides `Object`.
Here's an example of extending `SomeBaseClass`,
so that members of `SomeBaseClass` can be called on objects of type `T`:

`extends` 也可以用于 `Object` 以外的类型。
下面的例子扩展 `SomeBaseClass`，这样就可以在类型为 `T` 的对象上调用 `SomeBaseClass` 的成员：

<?code-excerpt "misc/lib/language_tour/generics/base_class.dart (generic)" replace="/extends SomeBaseClass(?=. \{)/[!$&!]/g"?>
```dart
class Foo<T [!extends SomeBaseClass!]> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {
  ...
}
```

It's OK to use `SomeBaseClass` or any of its subtypes as the generic argument:

把 `SomeBaseClass` 或其任何子类型作为泛型实参都可以：

<?code-excerpt "misc/test/language_tour/generics_test.dart (SomeBaseClass-ok)" replace="/Foo.\w+./[!$&!]/g"?>
```dart
var someBaseClassFoo = [!Foo<SomeBaseClass>!]();
var extenderFoo = [!Foo<Extender>!]();
```

It's also OK to specify no generic argument:

也可以不指定泛型实参：

<?code-excerpt "misc/test/language_tour/generics_test.dart (no-generic-arg-ok)" replace="/expect\((.*?).toString\(\), .(.*?).\);/print($1); \/\/ $2/g"?>
```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

Specifying any non-`SomeBaseClass` type results in an error:

指定任何非 `SomeBaseClass` 的类型都会出错：

```dart tag=fails-sa
var foo = [!Foo<Object>!]();
```

### Self-referential type parameter restrictions (F-bounds) {:#f-bounds}

### 自引用类型参数限制（F-bound） {:#6}

When using bounds to restrict parameter types, you can refer the bound
back to the type parameter itself. This creates a self-referential constraint,
or F-bound. For example:

用边界限制参数类型时，可以把边界指回类型参数自身。
这就形成了自引用约束，也叫 F-bound。例如：

<?code-excerpt "misc/test/language_tour/generics_test.dart (f-bound)"?>
```dart
abstract interface class Comparable<T> {
  int compareTo(T o);
}

int compareAndOffset<T extends Comparable<T>>(T t1, T t2) =>
    t1.compareTo(t2) + 1;

class A implements Comparable<A> {
  @override
  int compareTo(A other) => /*...implementation...*/ 0;
}

int useIt = compareAndOffset(A(), A());
```

The F-bound `T extends Comparable<T>` means `T` must be comparable to itself.
So, `A` can only be compared to other instances of the same type.

F-bound `T extends Comparable<T>` 表示 `T` 必须能和自身比较。
因此 `A` 只能和同一类型的其他实例比较。

## Using generic methods

## 使用泛型方法 {:#7}

Methods and functions also allow type arguments:

方法和函数也可以带类型实参：

<!-- {{site.dartpad}}/a02c53b001977efa4d803109900f21bb -->
<!-- https://gist.github.com/a02c53b001977efa4d803109900f21bb -->
<?code-excerpt "misc/test/language_tour/generics_test.dart (method)" replace="/<T.(?=\()|T/[!$&!]/g"?>
```dart
[!T!] first[!<T>!](List<[!T!]> ts) {
  // Do some initial work or error checking, then...
  [!T!] tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
```

Here the generic type parameter on `first` (`<T>`)
allows you to use the type argument `T` in several places:

这里 `first` 上的泛型类型参数（`<T>`）让你可以在几处使用类型实参 `T`：

* In the function's return type (`T`).

  在函数的返回类型（`T`）里。
  
* In the type of an argument (`List<T>`).

  在参数类型（`List<T>`）里。
  
* In the type of a local variable (`T tmp`).

  在局部变量的类型（`T tmp`）里。
  

[`List`]: {{site.dart-api}}/dart-core/List-class.html
[By convention]: /effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters
[top-and-bottom]: /null-safety/understanding-null-safety#top-and-bottom
