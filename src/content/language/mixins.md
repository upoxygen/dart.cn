---
# title: Mixins
title: Mixin
# description: Learn how to add to features to a class in Dart.
description: 了解如何在 Dart 中给类增加功能。
prevpage:
  url: /language/extend
  # title: Extend a class
  title: 扩展类
nextpage:
  url: /language/enums
  # title: Enums
  title: 枚举
---

<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g; / *\/\/\s+ignore:[^\n]+//g; /([A-Z]\w*)\d\b/$1/g"?>

Mixins are a way of defining code that can be reused in multiple class hierarchies.
They are intended to provide member implementations en masse. 

Mixin 用来定义可在多个类层次里复用的代码。
它们的目的是成批提供成员实现。

To use a mixin, use the `with` keyword followed by one or more mixin
names. The following example shows two classes that use (or, are subclasses of)
mixins:

使用 mixin 时，写 `with` 关键字，后面跟一个或多个 mixin 名字。
下面的例子有两个类使用了 mixin（或者说是 mixin 的子类）：

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (musician-and-maestro)" replace="/(with.*) \{/[!$1!] {/g"?>
```dart
class Musician extends Performer [!with Musical!] {
  // ···
}

class Maestro extends Person [!with Musical, Aggressive, Demented!] {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

To define a mixin, use the `mixin` declaration. 
In the rare case where you need to define both a mixin _and_ a class, you can use
the [`mixin class` declaration](#class-mixin-or-mixin-class).

定义 mixin 时使用 `mixin` 声明。
少数情况下既要 mixin 也要类，可以使用 [`mixin class` 声明](#class-mixin-or-mixin-class)。

Mixins and mixin classes cannot have an `extends` clause,
and must not declare any generative constructors.

Mixin 和 mixin class 不能有 `extends` 子句，也不能声明任何生成式构造函数。

For example:

例如：

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (musical)"?>
```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

## Specify members a mixin can call on itself

## 指定 mixin 可以在自身上调用的成员 {:#1}

Sometimes a mixin depends on being able to invoke a method or access fields,
but can't define those members itself (because mixins can't use constructor
parameters to instantiate their own fields).

有时 mixin 需要调用某个方法或访问字段，
但自己又不能定义这些成员（因为 mixin 不能用构造函数参数来初始化自己的字段）。

The following sections cover different strategies for ensuring any subclass
of a mixin defines any members the mixin's behavior depends on. 

下面几节介绍不同做法，确保 mixin 的任何子类都定义了 mixin 行为所依赖的成员。

### Define abstract members in the mixin

### 在 mixin 里定义抽象成员 {:#2}

Declaring an abstract method in a mixin forces any type that uses
the mixin to define the abstract method upon which its behavior depends. 

在 mixin 里声明抽象方法，会强制任何使用该 mixin 的类型定义这个行为所依赖的抽象方法。

```dart
mixin Musician {
  void playInstrument(String instrumentName); // Abstract method.

  void playPiano() {
    playInstrument('Piano');
  }
  void playFlute() {
    playInstrument('Flute');
  }
}

class Virtuoso with Musician { 

  @override
  void playInstrument(String instrumentName) { // Subclass must define.
    print('Plays the $instrumentName beautifully');
  }  
} 
```

#### Access state in the mixin's subclass

#### 访问 mixin 子类中的状态 {:#3}

Declaring abstract members also allows you to access state on the subclass
of a mixin, by calling getters which are defined as abstract on the mixin:

声明抽象成员，还可以通过调用 mixin 上定义为抽象的 getter，访问 mixin 子类上的状态：

```dart
/// Can be applied to any type with a [name] property and provides an
/// implementation of [hashCode] and operator `==` in terms of it.
mixin NameIdentity {
  String get name;

  @override
  int get hashCode => name.hashCode;

  @override
  bool operator ==(other) => other is NameIdentity && name == other.name;
}

class Person with NameIdentity {
  final String name;

  Person(this.name);
}
```

### Implement an interface

### 实现接口 {:#4}

Similar to declaring the mixin abstract, putting an `implements` clause on the
mixin while not actually implementing the interface will also ensure any member
dependencies are defined for the mixin.

和把 mixin 声明为抽象类似，在 mixin 上写 `implements` 子句、但并不真正实现该接口，
同样能确保 mixin 所依赖的成员都被定义。

```dart
abstract interface class Tuner {
  void tuneInstrument();
}

mixin Guitarist implements Tuner {
  void playSong() {
    tuneInstrument();

    print('Strums guitar majestically.');
  }
}

class PunkRocker with Guitarist {

  @override
  void tuneInstrument() {
    print("Don't bother, being out of tune is punk rock.");
  }
}
```

### Use the `on` clause to declare a superclass

### 用 `on` 子句声明超类 {:#5}

The `on` clause exists to define the type that `super` calls are resolved against.
So, you should only use it if you need to have a `super` call inside a mixin. 

`on` 子句用来定义 `super` 调用要解析到的类型。
所以只有 mixin 内部需要 `super` 调用时才该使用它。

The `on` clause forces any class that uses a mixin to also be a subclass
of the type in the `on` clause.
If the mixin depends on members in the superclass,
this ensures those members are available where the mixin is used:

`on` 子句强制任何使用该 mixin 的类，同时也是 `on` 子句中那个类型的子类。
如果 mixin 依赖超类中的成员，这样就能保证使用 mixin 的地方这些成员可用：

```dart
class Musician {
  musicianMethod() {
    print('Playing music!');
  }
}

mixin MusicalPerformer [!on Musician!] {
  performerMethod() {
    print('Performing music!');
    super.musicianMethod();
  }
}

class SingerDancer extends Musician with MusicalPerformer { }

main() {
  SingerDancer().performerMethod();
}
```

In this example, only classes that extend or implement the `Musician` class
can use the mixin `MusicalPerformer`. Because `SingerDancer` extends `Musician`,
`SingerDancer` can mix in `MusicalPerformer`.

这个例子里，只有扩展或实现了 `Musician` 类的类才能使用 mixin `MusicalPerformer`。
因为 `SingerDancer` 扩展了 `Musician`，所以 `SingerDancer` 可以混入 `MusicalPerformer`。

## `class`, `mixin`, or `mixin class`?

## `class`、`mixin` 还是 `mixin class`？ {:#6}

:::version-note
The `mixin class` declaration requires a [language version][] of at least 3.0.

`mixin class` 声明至少需要 [语言版本][language version] 3.0。
:::

A `mixin` declaration defines a mixin. A `class` declaration defines a [class][].
A `mixin class` declaration defines a class that is usable as both a regular class
and a mixin, with the same name and the same type.

`mixin` 声明定义一个 mixin。`class` 声明定义一个[类][class]。
`mixin class` 声明定义一个既能当普通类、也能当 mixin 的类，名字和类型都相同。

```dart
mixin class Musician {
  // ...
}

class Novice with Musician { // Use Musician as a mixin
  // ...
}

class Novice extends Musician { // Use Musician as a class
  // ...
}
```

Any restrictions that apply to classes or mixins also apply to mixin classes:

适用于类或 mixin 的限制，也适用于 mixin class：

- Mixins can't have `extends` or `with` clauses, so neither can a `mixin class`.

  Mixin 不能有 `extends` 或 `with` 子句，`mixin class` 也不能有。
  
- Classes can't have an `on` clause, so neither can a `mixin class`.

  类不能有 `on` 子句，`mixin class` 也不能有。
  

[language version]: /language/versioning
[class]: /language/classes
[class modifiers]: /language/class-modifiers
