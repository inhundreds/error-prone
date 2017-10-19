---
title: ClassNewInstance
summary: Class.newInstance() bypasses exception checking; prefer getDeclaredConstructor().newInstance()
layout: bugpattern
tags: FragileCode
severity: WARNING
providesFix: REQUIRES_HUMAN_ATTENTION
---

<!--
*** AUTO-GENERATED, DO NOT MODIFY ***
To make changes, edit the @BugPattern annotation or the explanation in docs/bugpattern.
-->

## The problem
The documentation for [`Class#newInstance`][javadoc] includes the following
warning:

[javadoc]: https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance

> Note that this method propagates any exception thrown by the nullary
> constructor, including a checked exception. Use of this method effectively
> bypasses the compile-time exception checking that would otherwise be performed
> by the compiler. The `Constructor.newInstance` method avoids this problem by
> wrapping any exception thrown by the constructor in a (checked)
> `InvocationTargetException`.

Always prefer `myClass.getConstructor().newInstance()` to calling
`myClass.newInstance()` directly. The `Class#newInstance` method is slated for
[deprecation in JDK 9](https://bugs.openjdk.java.net/browse/JDK-6850612).

Note that migrating to `Constructor#newInstance` requires handling two new
exceptions: [`IllegalArgumentException`][iae] and [`InvocationTargetException`]
[ite].

[iae]: https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html
[ite]: https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html

## Suppression
Suppress false positives by adding an `@SuppressWarnings("ClassNewInstance")` annotation to the enclosing element.