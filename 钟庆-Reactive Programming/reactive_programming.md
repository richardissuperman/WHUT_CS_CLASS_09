# Reactive Functional Programming 入门


我相信大部分同学现在或多或少都听说过reactive programming(响应式编程)或者functional programming（函数式编程），尤其是做移动端开发的同学们，响应式函数编程已经成了2016年最酷炫的代名词。

然而，对于毫无函数式编程基础的同学来说，想要直接学习响应式编程的库，例如[RxJava](https://github.com/ReactiveX/RxJava),或者[RxPython](https://github.com/ReactiveX/RxPY),可能会有些难度。如果你学过Scala，一个基于JVM的函数式语言，你可能会发现很多在响应式编程中很眼熟的syntax，像map(),flatmap(),filter(),reduce()等等，这或许会给你带来一些学习响应式编程的优势。但是在这个分享里面，我打算从头开始，从最基本的函数式编程的基础来解释起，让大家对其有一些启蒙的认识。幸运的是，我们不需要直接从Reactive的库开始学习起，Java 8 已经对“函数式”编程有所支持，感兴趣的同学可以先大概浏览一下Java 8中的Stream API，和我们接下来介绍的东西会很像。

![functional programming](https://github.com/richardissuperman/sy0901/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/download.jpeg)

同时，需要提醒的是，在Reactive programming的框架中，基本涵盖了现有的所有流行的编程语言，java我们有RxJava，python有RxPy，等等。这次分享中，我将以RxJava作为例子。

* Languages
* Java: RxJava
* JavaScript: RxJS
* C#: Rx.NET
* C#(Unity): UniRx
* Scala: RxScala
* Clojure: RxClojure
* C++: RxCpp
* Lua: RxLua
* Ruby: Rx.rb
* Python: RxPY
* Go: RxGo
* Groovy: RxGroovy
* JRuby: RxJRuby
* Kotlin: RxKotlin
* Swift: RxSwift
* PHP: RxPHP
* Elixir: reaxive
* Dart: RxDart


对于初学者来说，函数式编程的优势基本上可以归结为两点：

> * 1.函数可以作为参数，传递到其他函数，类中。函数也因此被称为"第一等公民"（first class object）
> * 2.函数式编程把程序中的函数的调用像真正的数学中的函数一样对待。数学的函数中，我们没有全局变量(global parameter)所有函数的输入都仅仅依赖于上一个函数的输出。
