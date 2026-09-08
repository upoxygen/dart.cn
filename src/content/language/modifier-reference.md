---
# title: Class modifiers reference
title: 类修饰符参考
# description: >-
#   The allowed and disallowed combinations of class modifiers.
description: >-
  类修饰符允许与不允许的组合。
prevpage:
  url: /language/class-modifiers
  # title: Class modifiers
  title: Class modifiers
nextpage:
  url: /language/concurrency
  # title: Concurrency in Dart
  title: Concurrency in Dart
---

This page contains reference information for
[class modifiers](/language/class-modifiers).

本页是[类修饰符](/language/class-modifiers)的参考信息。

## Valid combinations

## 有效组合 {:#1}

The valid combinations of class modifiers and their resulting capabilities are:

类修饰符的有效组合，以及各自具备的能力：

| <t>Declaration</t><t>声明</t> | <t>[Construct][]?</t><t>[构造][Construct]？</t> | <t>[Extend][]?</t><t>[扩展][Extend]？</t> | <t>[Implement][]?</t><t>[实现][Implement]？</t> | <t>[Mix in][]?</t><t>[混入][Mix in]？</t> | <t>[Exhaustive][]?</t><t>[穷尽][Exhaustive]？</t> |
|-----------------------------|----------------|-------------|----------------|-------------|-----------------|
| `class`                     | **Yes**        | **Yes**     | **Yes**        | No          | No              |
| `class`                     | **是**         | **是**      | **是**         | 否          | 否              |
| `base class`                | **Yes**        | **Yes**     | No             | No          | No              |
| `base class`                | **是**         | **是**      | 否             | 否          | 否              |
| `interface class`           | **Yes**        | No          | **Yes**        | No          | No              |
| `interface class`           | **是**         | 否          | **是**         | 否          | 否              |
| `final class`               | **Yes**        | No          | No             | No          | No              |
| `final class`               | **是**         | 否          | 否             | 否          | 否              |
| `sealed class`              | No             | No          | No             | No          | **Yes**         |
| `sealed class`              | 否             | 否          | 否             | 否          | **是**          |
| `abstract class`            | No             | **Yes**     | **Yes**        | No          | No              |
| `abstract class`            | 否             | **是**      | **是**         | 否          | 否              |
| `abstract base class`       | No             | **Yes**     | No             | No          | No              |
| `abstract base class`       | 否             | **是**      | 否             | 否          | 否              |
| `abstract interface class`  | No             | No          | **Yes**        | No          | No              |
| `abstract interface class`  | 否             | 否          | **是**         | 否          | 否              |
| `abstract final class`      | No             | No          | No             | No          | No              |
| `abstract final class`      | 否             | 否          | 否             | 否          | 否              |
| `mixin class`               | **Yes**        | **Yes**     | **Yes**        | **Yes**     | No              |
| `mixin class`               | **是**         | **是**      | **是**         | **是**      | 否              |
| `base mixin class`          | **Yes**        | **Yes**     | No             | **Yes**     | No              |
| `base mixin class`          | **是**         | **是**      | 否             | **是**      | 否              |
| `abstract mixin class`      | No             | **Yes**     | **Yes**        | **Yes**     | No              |
| `abstract mixin class`      | 否             | **是**      | **是**         | **是**      | 否              |
| `abstract base mixin class` | No             | **Yes**     | No             | **Yes**     | No              |
| `abstract base mixin class` | 否             | **是**      | 否             | **是**      | 否              |
| `mixin`                     | No             | No          | **Yes**        | **Yes**     | No              |
| `mixin`                     | 否             | 否          | **是**         | **是**      | 否              |
| `base mixin`                | No             | No          | No             | **Yes**     | No              |
| `base mixin`                | 否             | 否          | 否             | **是**      | 否              |

{: .table .table-striped .nowrap}

[Construct]: /language/classes#using-constructors
[Extend]: /language/extend
[Implement]: /language/classes#implicit-interfaces
[Mix in]: /language/mixins
[Exhaustive]: /language/branches#exhaustiveness-checking

## Invalid combinations

## 无效组合 {:#2}

Certain [combinations][] of modifiers aren't allowed:

某些修饰符[组合][combinations]是不允许的：

| <t>Combination</t><t>组合</t> | <t>Reasoning</t><t>原因</t> |
|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `base`, `interface`, and `final`              | All control the same two capabilities (`extend` and `implement`), so are mutually exclusive.                                                    |
| `base`、`interface` 和 `final`                | 它们控制的是同一组能力（`extend` 和 `implement`），因此互斥。                                                                                    |
| `sealed` and `abstract`                       | Neither can be constructed, so are redundant together.                                                                                          |
| `sealed` 和 `abstract`                        | 都不能被构造，放在一起是多余的。                                                                                                                |
| `sealed` with `base`, `interface`, or `final` | `sealed` types already cannot be mixed in, extended or implemented from another library, so are redundant to combine with the listed modifiers. |
| `sealed` 与 `base`、`interface` 或 `final`    | `sealed` 类型本来就不能从另一个库混入、扩展或实现，再和这些修饰符组合是多余的。                                                                  |
| `mixin` and `abstract`                        | Neither can be constructed, so are redundant together.                                                                                          |
| `mixin` 和 `abstract`                         | 都不能被构造，放在一起是多余的。                                                                                                                |
| `mixin` and `interface`, `final`, or `sealed` | A `mixin` or `mixin class` declaration is intended to be mixed in, which the listed modifiers prevent.                                          |
| `mixin` 与 `interface`、`final` 或 `sealed`   | `mixin` 或 `mixin class` 声明本意就是被混入，而这些修饰符会阻止混入。                                                                            |
| `enum` and any modifiers                      | `enum` declarations can't be extended, implemented, mixed in, and can always be instantiated, so no modifiers apply to `enum` declarations.     |
| `enum` 与任何修饰符                           | `enum` 声明不能被扩展、实现或混入，而且总能实例化，所以修饰符对 `enum` 不适用。                                                                  |
| `extension type` and any modifiers            | `extension type` declarations can't be extended or mixed in, and can only be implemented by other `extension type` declarations.                |
| `extension type` 与任何修饰符                 | `extension type` 声明不能被扩展或混入，而且只能被其他 `extension type` 实现。                                                                    |

{: .table .table-striped .nowrap}

[combinations]: /language/class-modifiers#combining-modifiers
