# Q&A

*********

### 问题1

- 谢谢庆哥的分享，很赞:kissing_heart::kissing_heart:
- 链式调用方式确实简洁美观，但对响应式编程不了解，问点不专业的问题:joy:。每经过一环，都会返回一个新的改变后的Stream或Observable，如果想保存中间的这些Stream或Observable，是不是只有调用subscribe()方法才能做到？
- 链式调用允许分叉吗？比如说对某文件夹下的'png'后缀文件用UI1展示，'jpg'文件用UI2展示，这种情况是不是要用两条链来处理，也就相当于对初始源做了两次遍历。（如果更多条件呢）
>回答:1. Observable在吸收一个集合之后，无论做任何操作符都会返回一个新的Observable没错，你说的保存应该是说把Observable的元素输出对吧？响应式编程虽然运用了大量函数式编程的概念，但是它不叫函数式编程的一大原因就是它运用了[观察者模式](https://zh.wikipedia.org/wiki/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)，所以Observable相当于一个源，的确只有再subscribe()方法调用之后才能输出。不subscribe()的Observable根本就不会启动。

>回答：2.这个问题非常好，其实再抽象一点田哥的问题就是在于一个我们所谓的链，怎么分流的问题。解决的办法也有很多，比较傻一点的方法就是，分成两个链来做，比如说：
```java
        //第一个Observable，subscribe()之后只加png
        Observable.from( folders )
                .filter(new Func1<File, Boolean>() {
                    @Override
                    public Boolean call(File file) {
                        return file.getName().endsWith(".png");
                    }
                })
                .map(new Func1<File, Bitmap>() {
                    @Override
                    public Bitmap call(File file) {
                        return getBitMapFromFile(file);
                    }
                })
                .subscribe(new Subscriber<Bitmap>() {
                    @Override
                    public void onCompleted() {

                    }

                    @Override
                    public void onError(Throwable e) {

                    }
                  //添加在第一个UI里面
                    @Override
                    public void onNext(Bitmap bitmap) {
                        imageCollectView.addImage( bitmap );
                    }
                });

        //第二个Observable，subscribe()之后只加jpg

        Observable.from( folders )
                .filter(new Func1<File, Boolean>() {
                    @Override
                    public Boolean call(File file) {
                        return file.getName().endsWith(".jpg");
                    }
                })
                .map(new Func1<File, Bitmap>() {
                    @Override
                    public Bitmap call(File file) {
                        return getBitMapFromFile(file);
                    }
                })
                .subscribe(new Subscriber<Bitmap>() {
                    @Override
                    public void onCompleted() {

                    }

                    @Override
                    public void onError(Throwable e) {

                    }
                    //添加在第二个UI里面
                    @Override
                    public void onNext(Bitmap bitmap) {
                        imageCollectView2.addImage( bitmap );
                    }
                });
```
 当然样做的确有点浪费，一是有很多重复的代码，而是每个Observable最后肯定都会迭代一次列表，重复计算太多。。。
 
 
 聪明一点方法就是用concatMap()/或者flatmap()操作符来进行分流。我可以画一个示意图，这两个操作符有啥作用。
 
 ![分流再合并](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/Screen%20Shot%202017-04-01%20at%2012.18.25%20pm.png)
 
这两个操作符又图所示，作用是每一个源Observable发射出一个元素的时候，都会根据flatMap()里面的返回值发射一个新的Observable(),然后每个新的Observable最后又会合并再一起，共享subscribe(),里面的onComplete()回调。看看代码

![flatmap()](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/Screen%20Shot%202017-04-01%20at%2012.28.52%20pm.png)

注意那个doOnNext()操作符比较特殊，它不反悔新的Observable，而是对上一个Observable发射的每个元素做额外处理，可以看成是subscribe()中那个onNext()的替代品。




*********

### 问题2

庆哥，goooooood！！！
我对reactive programming不是很了解，不过看了庆哥的分享觉得这个响应式编程非常赞，和云计算里task组件功能非常相似。
我这里有个问题：庆哥举的reactive 的链式处理都是本地数据处理的例子，那如果放在分布式场景里，该怎么处理？比如说，咱们链式处理的某一环不是简单的对上一个函数的输出结果进行处理，而是需要拿着上一个函数的数据结果去请求其他服务，而这个请求又是异步的，这里就可能涉及异步回调的问题，对于这种场景reactive programming是否有比较好的处理架构？

实际开发过程中没用过reactive programming，所以我也是突发奇想，如果问的不专业，请谅解 ：）
>回答:
仔仔的问题非常好，也是我们在安卓主要应用的一个点。实际上在我们公司的安卓应用中，使用RxJava或者Reactive Programming的最主要原因，就是RxJava对Observable的合并，规约的操作的支持。我们可以把一个Observable想象成一个Task，一个任务。那么不同的Task/Observable之间可能存在依赖，比如我们看下图：

![依赖](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/Screen%20Shot%202017-04-08%20at%2010.29.29%20pm.png)

上图诠释了一个应用场景，我们的程序需要调用四个API，四个HTTP call，并且每一个API的调用的参数都是从上一个API得来。这里就涉及到嵌套调用（嵌套回调），也就是说我们传统的异步编程里面的所说的Nested Callback，一层跌一层。这才安卓里面也经常出现，比如说，通过user id，得到user的信息，通过user的信息再获取user观看视频的历史等等。。。

一般的做法就是使用嵌套回调，程序一般会看起来像这样：

![嵌套](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-Reactive%20Programming/images/Screen%20Shot%202017-04-08%20at%2010.39.03%20pm.png)

这样的写法，如果嵌套多几层会很难看，而且很难修改，用一个术语描述就是
>Call Back Hell

![call back hell](http://icompile.eladkarako.com/wp-content/uploads/2016/01/icompile.eladkarako.com_callback_hell.gif)

在RxJava中，我们可以使用[concatMap()](http://reactivex.io/documentation/operators/concat.html)或者[flatmap()](http://reactivex.io/documentation/operators/flatmap.html)来解决这个问题。

比如说，我们可以把HTTP API call重新写成同步的方法(比如说在Java里面是HttpUrlConnection.java提供同步的HTTP连接)。

```java
 Observable.create(new Observable.OnSubscribe<Object>() {
            @Override
            public void call(Subscriber<? super String> subscriber) {
                HttpURLConnection connection =......
                //这里省去初始化HttpConnection的步骤
                connection.connect();
                Byte[] result = new Byte[128];
                connection.getInputStream().read(result);
                return result.toString();
            }
        })
                //API call 2
                .concatMap(new Func1<String, Observable<String>>() {

                    @Override
                    public Observable<String> call(String response) {
                        //这里的response就是第一步的API call的结果
                        return Observable.create(new Observable.OnSubscribe<Object>() {
                            @Override
                            public void call(Subscriber<? super String> subscriber) {
                                HttpURLConnection connection =......
                                //这里省去初始化HttpConnection的步骤
                                connection.connect();
                                Byte[] result = new Byte[128];
                                connection.getInputStream().read(result);
                                return result.toString();
                            }
                        })
                    }
                })
                //API call 3
                .concatMap(new Func1<String, Observable<String>>() {

                    @Override
                    public Observable<String> call(String response) {
                        //这里的response就是第二步的API call的结果
                        return Observable.create(new Observable.OnSubscribe<Object>() {
                            @Override
                            public void call(Subscriber<? super String> subscriber) {
                                HttpURLConnection connection =......
                                //这里省去初始化HttpConnection的步骤
                                connection.connect();
                                Byte[] result = new Byte[128];
                                connection.getInputStream().read(result);
                                return result.toString();
                            }
                        })
                    }
                })
```

可能看起来有点复杂，但是仔细观察这其中的链式调用你会发现，你可以很轻松的把三个不同的，彼此依赖的API call通过concatMap()连接起来，这样可读性大大增加，而且最关键的是，非常非常的易于修改，比如你想修改依赖的顺序，或者删除依赖，很容易定位代码行，而不是像传统的做法，一层层嵌套去找。同时，使用concatMap或者flatmap不需要担心线程的问题，concatMap只有当上一层的任务完成之后，才会调用下一层task，也就是说不论这两个任务是不是运行在同一个线程，下层的任务只有在上一层执行结束才会开始。

关于RxJava对着方面的支持还有很多，比如有操作符zip()可以把不通API call的结果融合起来组成新的对象，或者merge()让不同的Task全部完成时发出通知。美国非常有名的公司Netflix（就是纸牌屋的那个电视剧的公司）的Head engineer 在一次大会中分享了他们在不同的micro service中是怎么应用RxJava，把不同微服务的API call的结果整合的，感兴趣的可以参考一下他们的视频。。。需要翻墙:

[![IMAGE ALT TEXT HERE](https://i.ytimg.com/vi/_t06LRX0DV0/hqdefault.jpg?custom=true&w=336&h=188&stc=true&jpg444=true&jpgq=90&sp=68&sigh=uXUEnUvmA6tvgMhEwEBAUgCxD-E)](https://www.youtube.com/watch?v=_t06LRX0DV0)



*********