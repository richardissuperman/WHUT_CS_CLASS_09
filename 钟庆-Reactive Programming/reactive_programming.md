# Reactive Functional Programming 入门


我相信大部分同学现在或多或少都听说过reactive programming(响应式编程)或者functional programming（函数式编程），尤其是做移动端开发的同学们，响应式函数编程已经成了2016年最酷炫的代名词。

然而，对于毫无函数式编程基础的同学来说，想要直接学习响应式编程的库，例如[RxJava](https://github.com/ReactiveX/RxJava),或者[RxPython](https://github.com/ReactiveX/RxPY),可能会有些难度。如果你学过Scala，一个基于JVM的函数式语言，你可能会发现很多在响应式编程中很眼熟的syntax，像map(),flatmap(),filter(),reduce()等等，这或许会给你带来一些学习响应式编程的优势。但是在这个分享里面，我打算从头开始，从最基本的函数式编程的基础来解释起，让大家对其有一些启蒙的认识。幸运的是，我们不需要直接从Reactive的库开始学习起，Java 8 已经对“函数式”编程有所支持，感兴趣的同学可以先大概浏览一下Java 8中的Stream API，和我们接下来介绍的东西会很像。

![functional programming](https://github.com/richardissuperman/sy0901/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/download.jpeg)

同时，需要提醒的是，在Reactive programming的框架中，基本涵盖了现有的所有流行的编程语言，java我们有RxJava，python有RxPy，等等。这次分享中，我将先以入门的Java 8 Stream API让大家先了解函数式编程的一些精髓，再以RxJava作为例子。

下面的列表是每种不同语言对应的Reactive Programming框架：

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

那么这次分享分为以下几个部分：

- [1] 函数式编程的优势
- [2] 函数式编程的风格
- [3] Java 8对于函数式编程的支持
- [4] RxJava的基础与优势



## 函数式编程的优势?

对于初学者来说，函数式编程的优势基本上可以归结为两点：


> * 1.函数可以作为参数，传递到其他函数，类中。函数也因此被称为"第一等公民"（first class object）
> * 2.函数式编程把程序中的函数的调用像真正的数学中的函数一样对待。数学的函数中，我们没有全局变量(global parameter)所有函数的输入都仅仅依赖于上一个函数的输出。

其中第二个优势似乎有些缥缈，大家有空可以钻研一下WikiPedia对其定义的原文（上面的是我们翻译的版本）
>**It treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data- Wikipedia**


********************
### talk is cheap, lets see the code

![少废话](https://github.com/richardissuperman/sy0901/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/1-N1TcWNYwID3AbvUf_mBiFA.jpeg)

## 函数式编程的风格?

### 风格1

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

### 风格2

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

我们可以尝试修改一下程序，把全局变量去掉，


```java
   void A(){
		boolean flag = true;
		//do something
		B(flag);
	}
	void B(boolean flag){
		C(flag);
	}
	void C(boolean flag){
		if(flag){
			//do something
		}
	}
```

嗯，现在的确看起来不依赖全局变量了，这也是执行一个方法链应该做到的。但是我们可以稍加修改，把这段程序写的更像函数式编程/响应式编程的风格。

```java
    public static void main(String[] args) {
    //方法执行编程真正的"链式调用了！"
		new Exe(true)
		.A()
		.B()
		.C();
	}
	
	static class Exe {
		boolean flag;
		public Exe(boolean flag){
			this.flag = flag;
			//do something
		}
		Exe A(){
			boolean flag = true;
			//do something
			return new Exe(flag);
		}
		Exe B(){
			return new Exe(flag);
		}
		Exe C(){
			if(flag){
				//do something
			}
			return new Exe(flag);
		}
	}
```

以上的代码诠释了一段面向对象编程的代码，如何变成像函数式编程一样的 <tr>**链式方法调用**</tr>。
但是有的同学会问，你这里面还不是在Exe 类里面定义了一个flag全局变量，这不是自己打自己脸么？


恩，严格的讲，一个类里面的全局变量并不是不允许，这也和函数式编程的中心思想不冲突，关键是如何使用。在上面这个例子中，每一次我调用A(),B()或者C()方法的时候，我都生成了一个新的Exe对象，在返回一个新对象之后，再调用新的Exe对象的方法，也就是说flag这个"全局变量”的作用域，在生成新的Exe之后就没有了，我们下一个方法调用还是仅仅依靠上一个方法的输出。

现在是时候review一下Java 8的Stream API了(还记得嘛，这个会和Reactive Programming框架有很多相似操作符的API集合)。
:grin::grin::grin::grin::grin::grin::grin:




## Java 8对于函数式编程的支持

在传统的面向对象编程中，如何迭代(iterate)一个集合(collection)一直都是一个头疼的问题。

比如我们有一下业务:
有一堆Transcation，我们需要把某种类型的Transcation提取出来，按照Transcation value的大小排序之后，把每个Transcation的ID按照顺序收集到另外一个List中。
>- [1] **从源Transcation list中(一个没有排序的list)收集Type为GROCERY的Transcation**
>- [2] **把上面的结果排序**
>- [3] **提取上面的Transcation的id，并且按照顺序存放到新的list里面**
>- [4] **返回上一个list**

按照传统的方式我们可能会：

```java
        List<Transaction> groceryTransactions = new Arraylist<>();
        //step 1 , 收集所有type为 GROCERY的transcation
		for (Transaction t : transactions) {
			if (t.getType() == Transaction.GROCERY) {
				groceryTransactions.add(t);
			}
		}
        //step2, 排序，按照value的大小
		Collections.sort(groceryTransactions, new Comparator() {
			public int compare(Transaction t1, Transaction t2) {
				return t2.getValue().compareTo(t1.getValue());
			}
		});
        //step3, 生成一个新的列表，把transcation id一个个放进去
		List<Integer> transactionIds = new ArrayList<>();
		for (Transaction t : groceryTransactions) {
			transactionsIds.add(t.getId());
		}

```

这样的写法太过时啦！而且可读性不高，试想一下新来的同事在接手你的项目的时候的表情。
那肯定是这样的：

![头都大了](https://github.com/richardissuperman/sy0901/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/1-RBFwLCiQ0G1aypbqhZxVbA.png)


但是使用新的java 8的“函数式编程” api，我们可以把程序写成这样:

```java
List<Integer> transactionsIds = 
			     transactions.stream()
			                .filter(t -> t.getType() == Transaction.GROCERY)
			                .sorted(comparing(Transaction::getValue))
			                .map(Transaction::getId)
                            //这一步会把Stream对象重新变成list
			                .collect(toList());
```

我们在调用一个集合的stream()方法后，就可以把一个集合变成Stream对象，然后对这个stream调用不同的操作符(这里用操作符(operators)而不是函数/方法，是尽量和数学里面的加减乘除的操作符概念一致)，可以使源Stream改变成不同的Stream。比如：

```java
List<Integer> transactionsIds = 
			     transactions.stream()
            //使用filter()操作符，可以把源Stream里面不符合filter()里面条件的内容自动删除,返回一个新的Stream
			                .filter(t -> t.getType() == Transaction.GROCERY)
			               
```

*********

```java
List<Integer> transactionsIds = 
			     transactions.stream()
                 //使用sorted()可以把源Stream排序，返回一个新的Stream
                            .sorted(comparing(Transaction::getValue))
			               
```

>提示:如果大家不能理解Stream怎么进行链式调用的，可以将Java 8 Stream对象类比为刚刚我们讲到的那个Exe class。每次调用Stream的一个操作符，都会返回一个新的Stream，这个Stream包含了对应的元素。

所以，像Java 8 Stream API这样的函数式编程风格，可以帮助我们更方便的处理集合(Collections)，但也不仅仅与此。


## RxJava的基础与优势

`RxJava`,是[Reactive Programming](http://reactivex.io/languages.html)框架对Java语言支持的一个类库，是众多Reactive Programming支持语言中的一种。在2016年之所以能大放异彩，超越其他Rx语言框架的原因，毫无疑问是因为安卓。
RxJava给程序带来的可读性，简洁性还有一些异步特性(`线程切换`)都在安卓平台上得到了充分的发挥，百分百切合了安卓`子线程处理，主线程接受结果`的模型。
>在安卓开发中，所有和构建UI界面相关的代码都必须在主线程中进行，但是在主线程中执行太多无关UI构建的代码，比如数据库连接，网络连接等，会严重的影响主线程对UI的渲染(造成UI的卡顿，不流畅)，于是通常的做法都是另开一个线程池，提取线程对网络进行连接，数据库连接等等，等获取到结果再把结果放进主线程接受并做相应的UI变化。iOS应该也大同小异。



那么RxJava到底难吗。。。一点也不，RxJava和上文提到的Java 8中的Stream API基本上有异曲同工之妙，只不过多增加了线程切换的功能。我们来看一段代码(怎么下载RxJava的类库我就不写了。。。自己可以上官网看):

```java
        ArrayList<Integer> source = new ArrayList<Integer>();
        source.add(1);
        source.add(2);
        ....
        ....
        source.add(100);

        //把source转变成一个Observable对象
        Observable.from( source )
                //1.把上面的Observable变成一个剔除掉小于50的集合的Observable
                .filter(new Func1<Integer, Boolean>() {
                    @Override
                    public Boolean call(Integer integer) {
                        return integer>=50;
                    }
                })
                //2.把上面生成的Observable再处理,每个元素由整形变成字符串类型
                .map(new Func1<Integer, String>() {
                    @Override
                    public String call(Integer integer) {
                        return "String to "+integer;
                    }
                })
                //3.只取前十个元素,构成新的Observable
                .take(10)
                //4.只有对Observable调用subscribe()方法,Observable才会将其包含的元素一个个发射出来
                .subscribe(new Subscriber<String>() {
                    //当所有元素发射完毕,onComplete会被调用
                    @Override
                    public void onCompleted() {
                        System.out.print("所有元素发射完成");
                    }
                    //出现错误时的回调,这里不做多讨论
                    @Override
                    public void onError(Throwable e) {

                    }
                    //每个元素依次在这个onNext方法中发射
                    @Override
                    public void onNext(String s) {
                        System.out.print(s);
                    }
                });
```
在RxJava中，一个Observable，就是上面每次调用操作符方法(filter,map,take)返回的对象，就和Java 8 API中的Stream对象一样，都可以看做一个包含了集合的处理器一样，每一次调用操作符，都会返回响应的一个新的Observable。新的Observable包含了对上一个Observable的改变(可以仔细的读一下注释)。而且对Observable的处理都是通过操作符来完成，不需要我们手动用迭代器，或者for循环来做。

上面的程序是一个简单的实例，在函数式编程的风格下，所有方法都是链式调用，非常的美观，可读性大大增强，在强大的操作符的支持下，每位开发者都可以轻松读懂这段程序是在做什么。

可能上面的例子比较弱智，那么我们可以来点证明，看看RxJava，或者说Reactive Programming可以怎样把一段复杂的程序变得简单，易读（一下截图是我在公司做presentation的时候的slides，我就不修改英文了。。。）。

![naive approach](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/1-bsjg87HqAw4vrjyajRdRMQ.png)

在上面这个例子中，我们尝试去完成了一下步骤:
- [1]遍历一个文件夹列表
- [2]遍历每个文件夹的每个文件
- [3]挑选出后缀名为"png"的文件，并且把他们转换成安卓中的Bitmap对象
- [4]把这些Bitmap(图片)对象添加到UI中（注意activity.runOnUiThread()方法就是简单的把任务添加到主线程中的方法而已，属于安卓平台特有的方法，可以忽略）


上面的例子够复杂么？可能不同人看法不一样。但是我敢保证的是，这几行代码非常的难修改。如果说我们现在要求之取前10张图片呢？（我猜很有可能我们就必须用一个全局变量来提前终止for循环了）。或者说我们想对文件转换成Bitmap对象这一步，让其在指定的线程池中进行。。。。虽然这些要求很操蛋，但是在现实的开发过程中，很多移动开发者都遇到过这些坑。用以往的new Thread().start()的方式来开启线程，注定是屌丝的做法，可读性极差。

让我们看看RxJava如何完美的解决！

![rxjava](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/1-fwmOEJrIvOTbXczT22ZFuw.png)

>- [1]flatMap()这个操作符把多个folder下面的file文件集中到同一个集合中。


>- [2]用filter操作符把不是“png”的文件剔除出上面的Observable。


>- [3]map()操作符把png 文件转换成安卓中的Bitmap对象,这里胜利getBitmapFromFile()方法的实现。


>- [4]subscribeOn()操作符可以把链式方法中，所有在它之前的操作符中的操作放入subscribeOn()方法参数所代表的线程池中。例子里面是Schedulers.newThread()线程池。是Rxjava默认的四个线程池中的一种。


>- [5]observeOn()操作符可以把链式方法中，所有在它之后的的操作符的操作放入参数所代表的线程中运行，本例子中是放入到AndroidScheduler.mainThread()，顾名思义就是放入主线程中。



可能有些操作符的用法大家还不是很熟悉，但是没关系，以后我在QA环节慢慢补充，这次分享主要的目的是让大家对函数式编程和响应编程有所了解，知道它的作用。

总结一下就是Reactive Programming框架带来了两点好处:
>1.代码都在一条链条上，操作符语义明确，给程序带来了前所未有的可读性
>
>2.线程的切换变得异常方便


这次分享就到此为止，感兴趣的同学可以在QA.md中提问，我会尽力回答。也可以再分享一些在公司项目中对RxJava的应用，可能会牵扯到一些负责的操作符，不过我个人认为非常有意思。
