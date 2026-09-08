---
# title: Keywords
title: 关键字
# description: Keywords in Dart.
description: Dart 中的关键字。
showToc: false
---

{% assign ckw = '&nbsp;<sup>1</sup>' %}
{% assign bii = '&nbsp;<sup>2</sup>' %}
{% assign unr = '&nbsp;<sup>3</sup>' %}

The following table lists the words
that the Dart language reserves for its own use.
These words can't be used as identifiers unless otherwise noted.
Even when allowed, using keywords as identifiers can confuse other
developers reading your code and should be avoided.
To learn more about identifier usage, click on the term.

下表列出 Dart 语言保留给自己使用的词。
除非另有说明，这些词不能用作标识符。
即使允许，把关键字当标识符也会让读代码的人困惑，应当避免。
想了解标识符用法，点击对应的词。

<table class="table table-striped">

{% tablerow keyword in keywords cols: 4 %}
<a href="{{keyword.link}}">{{keyword.term}}</a>
{%- case keyword.type %}
{% when 'bit' %}{{bii}}
{% when 'context' %}{{ckw}}
{% when 'unrestricted' %}{{unr}}
{% endcase %}
{% endtablerow %}
</table>

{{ckw}} This keyword can be used as an identifier
        depending on **context**.

{{ckw}} 这个关键字能否用作标识符，取决于 **上下文**。

{{bii}} This keyword can't be used as the name of a type
        (a class, a mixin, an enum, an extension type, or a type alias),
        the name of an extension, or as an import prefix.
        It can be used as an identifier in all other circumstances.

{{bii}} 这个关键字不能用作类型名
        （类、mixin、枚举、扩展类型或类型别名）、
        扩展名，或导入前缀。
        其他情况下可以用作标识符。

{{unr}} This keyword can be used as an identifier without restriction.

{{unr}} 这个关键字可以不受限制地用作标识符。
