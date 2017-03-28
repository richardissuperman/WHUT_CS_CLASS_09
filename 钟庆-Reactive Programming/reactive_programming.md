# Reactive Functional Programming 入门


我相信大部分同学现在或多或少都听说过reactive programming(响应式编程)或者functional programming（函数式编程），尤其是做移动端开发的同学们，响应式函数编程已经成了2016年最酷炫的代名词。

然而，对于毫无函数式编程基础的同学来说，想要直接学习响应式编程的库，例如[RxJava](https://github.com/ReactiveX/RxJava),或者[RxPython](https://github.com/ReactiveX/RxPY),可能会有些难度。如果你学过Scala，一个基于JVM的函数式语言，你可能会发现很多在响应式编程中很眼熟的syntax，像map(),flatmap(),filter(),reduce()等等，这或许会给你带来一些学习响应式编程的优势。但是在这个分享里面，我打算从头开始，从最基本的函数式编程的基础来解释起，让大家对其有一些启蒙的认识。幸运的是，我们不需要直接从Reactive的库开始学习起，Java 8 已经对“函数式”编程有所支持，感兴趣的同学可以先大概浏览一下Java 8中的Stream API，和我们接下来介绍的东西会很像。

![functional programming](https://github.com/richardissuperman/sy0901/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/download.jpeg)

同时，需要提醒的是，在Reactive programming的框架中，基本涵盖了现有的所有流行的编程语言，java我们有RxJava，python有RxPy，等等。这次分享中，我将以RxJava作为例子。

#### Languages
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

其中第二个优势似乎有些缥缈，大家有空可以钻研一下WikiPedia对其定义的原文（上面的是我们翻译的版本）
>**It treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data- Wikipedia**


********************
### talk is cheap, lets see the code

我们先探索一下第一个优势，在Java 8里面已经开始支持Lambda 表达式，也就是把“函数”当参数的写法了（当然这个特性可能在其他语言中早就有了，这里只是用java作为例子阐述）:

```java
   interface NewAction{
		void call();
	}

	public static void execute(NewAction action){
		action.call();
	}
	public static void main(String[] args) {
		//1.原始做法 - 需要显示的创建NewAction对象
		execute( new NewAction() {
			
			@Override
			public void call() {
				// TODO Auto-generated method stub
				System.out.println("action start");
			}
		} );
		
		//2.在Java8 中 - 直接将函数体放入参数中，使其变得像函数式编程一样啦！
		execute(()->System.out.println("action start"));
	}
```

让我们看看注释下面两种不同的做法，他们最后的结果都是打印出来一行字符串**action start** 而已，但是第二种做法明显简练一些，我们不再需要显示的创建NewAction对象的实例了。第二种做法中，当**对象实例只有一个方法的时候**，我们只需要传入函数体就ok了。但这这不是什么黑科技，java 8也没有颠覆原有的设计，因为这仅仅是java 8的编译器允许你这样写而已，使其看起来像传入一个函数作为参数而已，在底层的，被java编译过的代码中其实java还是创建了一个NewAction对象。
所以，说道对第一个优势的支持时，java还是略显稚嫩，支持范围并不广。但是始终也是一种进步啦！

:smile::smile::smile::smile::smile::smile::smile::smile::smile:

***************************

第二种优势显然抽象一些，但是我们还是以实例开始!
:heart_eyes::heart_eyes::heart_eyes::heart_eyes::heart_eyes::heart_eyes::heart_eyes:

比方说，当我们有一个方法调用序列:

funcA()->funcB()->funcC();

funcC()依赖于funcB()的执行结果，funcB()依赖于funcA()的执行结果，他们之间没有交叉依赖。

听起来好像不难理解吧，但是实际操作中，软件工程师们往往会忘记一些基本的规则，犯一些“小错误“，而且我相信我们之中的大多数都犯过这样的错误:
>错误的引入全局变量，影响方法/函数链的执行


```java
   private boolean flag;

	void A(){
		flag = true;
		//do something
		B();
	}
	void B(){
		C();
	}
	void C(){
		if(flag){
			//do something
		}
	}
```

在上面的例子中，本来应该依赖于B方法的C，现在执行取决于A了。这种事情在一大堆工程师协作的时候经常会出现。本来设定好的方法执行链会逐渐被一些外部的变量改变。当然这也是面向对象编程的一个通病。
