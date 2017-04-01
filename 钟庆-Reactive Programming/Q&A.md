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
>回答:答案




*********



