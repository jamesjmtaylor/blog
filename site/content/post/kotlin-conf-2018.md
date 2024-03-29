---
title: Kotlin Conf 2018
date: '2018-10-07T07:00:33-07:00'
---
![Kotlin Conference](/img/blog/kotlinConf.jpeg)

As a software developer at Steelcase I'm able to attend a conference a year.  This year I purchased tickets to the second ever Kotlin Conference!  Over the course of 3 days I learned an immense amount about Kotlin, coroutines, and multiplatform projects.  What follows is a massive brain dump of all the notes and highlights that I took during the conference.

## Coroutines

* Kotlin equivalent of C# async/await. (suspend = async; await is implied); can declare scope as main and coroutine will avoid blocking UI thread
  .
* Async alternatives include AsyncTask, Executors, Loaders (deprecated), Promises (API 24+), Threads, and Libraries (i.e. RxJava)
* Not currently supported by Robolectric
  .  Testing still possible with “runBlocking {…}” lambda which forces synchronous execution.
* You can explicitly prepend “async” to a suspend function call in order to immediately return your response type wrapped in a ”Deferred” object.
* You can unwrap a “Deferred” object by call “.wait”
  .
* For a list of “Deferred” you can unwrap them in 1 of 2 ways:
  * deferredList.map { it.await() } sequentially executes waits
  * awaitAll(deferredList) executes waits in parallel.
* Requires: 
  * kotlin_version = '1.3.0-rc-116’,
  * org.jetbrains.kotlinx:kotlinx-coroutines-core:0.30.1 
  * org.jetbrains.kotlinx:kotlinx-coroutines-android:0.30.1

## Multiplatform Projects

* For interoperability you can compile a Kotlin Library (.klib) into a native iOS ObjC Library (with code completion in Xcode!) or call it directly from Android.
* You can also import existing C libraries into either platform. 
* Issues with iOS MPP
  * No generics, protected access is broken, enums still somewhat broken.
  * No iOS bitcode support (bitcode must be turned off for the entire iOS project)
  * No step-through debugging of shared libraries in Xcode (Android Studio is fine)
  * Threading is platform specific.  Coroutines are not ready for MPP yet
  * No Date object support since Date objects are platform specific.
  * No mocking or dependency injection libraries are available for shared libraries.
  * Crashlytics stack trace reporting give only method names, no line numbers.
  * Overall poor documentation and esoteric setup.
* [More information available here](https://kotlinlang.org/docs/tutorials/native/mpp-ios-android.html). 

## Kotlin Unit Testing

* Best practice is to use immutability, non-nullability, and non-static constants as much as possible.
* Use of data classes provide ’equals()’ implementation for free, allowing you to easily compare an expected object with the generated object, increasing testing accuracy over attribute comparisons.
* Data classes also provide ‘copy()’ implementation for free, allowing you to generate a deep copy of an object and selectively specify attributes that you want to change in the copy.
* Recommended test stack of JUnit5, MockK, and Kluent.
* JUnit5 provides the following:
  * Allows use of backticks for test names with spaces, i.e. ”fun \`user clicks and updates list\`()”
  * Allows use of @Nested inner classes for better organization of unit tests.
  * Allows combination of @ParameterizedTest and Stream.of(…) with a custom Data Class TestClass(actual,expected) for one line assertions that give specific failures
* MockK implicitly applies “open” keyword to classes under test (Kotlin classes are final by default)
* Kluent provides fluent assertions.

## Kotlin for Graphics by Romain Guy

* Use KTX for purposes of ColorInt extensions to enable clean bit-shifting
* Use Kotlin operators for vector & matrix intersection (“or”), symmetric difference (“xor”), and union (“and”)
* Use the “contains()” operator  for geometric class extension functions for clean collision detection
* Use range operators for creating & executing clean & concise geometry extension functions.
* Geometries should be immutable and functions pure.
* Use aggregate functions like “all(…)” (which applies a flatmap of the && operator) and “any(…)” (which applies a flatmap of the || operator) for cleaner logic
* Use inline classes wherever possible as zero-cost abstractions (just be aware of functions that cause autoboxing operations that nullify this benefit, i.e. ==).
