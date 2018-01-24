上次我们在结束二叉树的题目分析之前，做了一个简单的二叉树层序遍历(**广度优先搜索**)的模板代码的学习，我们应该还能记得，广度优先要使用```队列```，AKA -> **Queue**这个数据结构来做。用Java的伪代码我们再复习一遍

```java
public void levelTraverse(TreeNode root){
	if(root == null){
    	return;
    }
    //初始化队列
	Queue queue = new LinkedList();
    //把根节点加入队列
    queue.add(root);
    //开始遍历队列
    while(!queue.isEmpty()){
    	TreeNode current = queue.poll();
        System.out.println(current.toString());
        //只要当前节点的左右节点不为空，那么我们就可以把其加入到队列的尾部，等待下一次遍历，我们continue这个while循环
        if(current.left != null){
        	queue.add(current.left);
        }
        if(current.right != null){
        	queue.add(current.right);
        }
    }

}

```

>从以上模板代码我们可以看出，对于广度优先搜索，其核心就在于使用队列Queue来做一个while循环，在循环内除了对当前节点的处理之外，还需要将当前节点的孩子节点放入队列的尾部。这样我们就实现了简单的广度优先搜索。

就是这么的简单！

![images.jpeg](http://upload-images.jianshu.io/upload_images/1777208-462747a342c2d042.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> #### 那么，问题来了，核心的部分既然这么简单，广度优先搜索的应用又有哪些呢？

今天文章的重点就是，哪些普遍的问题可以用广度优先搜索来解决。

### 1.几度好友问题？

![108555505.jpg](http://upload-images.jianshu.io/upload_images/1777208-b30959d2539c3023.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


熟练玩耍各种社交网站的朋友都会发现网站经常都会给你推荐可能认识的好友，还会"友情"提示该推荐好友到底通过什么途径推荐。这里我们重点介绍几度好友这种推荐模式。

顾名思义，几度好友的意思代表的就是该位用户和你中间相隔了有多少度，也就是多少个人。曾经哈佛大学的心理学教授Stanley Milgram 提出了一个叫[六度分隔理论](https://baike.baidu.com/item/%E5%85%AD%E5%BA%A6%E5%88%86%E9%9A%94%E7%90%86%E8%AE%BA)每个人和另外随机的一个陌生人的距离只隔着6个人。也就是说你和特朗普之间可能也就是隔着6个人哦。

好了，交代了这么多背景，我们可以开始思索一个问题了，假如说我们已经有了好友相关信息，推荐系统怎么找到对应度数的好友呢？

举个栗子。人人网现在要推荐给一个用户他的三度以内的好友放在推荐栏里面，我们怎么获取？

先把用户的数据结构贴出来

```java
public class User{

	//这个friends是该用户的直接好友，也就是一度好友、
	private List<User> friends;
    private String name;
    
    //获取好友列表
    public List<User> getFriends(){
    	return Collections.unmodifiableList(friends);
    }
    
    public String getUserName(){
    	return name;
    }
    
}

```

假设我们已经有了这样的一个好友结构在我们的内存里面(**当然在实际的场景里面，我们不可能把一个社交网络的所有用户信息存在Ram里面，这不现实，不过找几度好友的原理肯定一样，只不过在分布式场景里面获取用户信息的过程要复杂很多**)，每个User都有一个叫friends的List，保存他的直接好友。

根据以上的条件我们可以这么思考，**我们需要去求的几度好友的这个几度，是不是其实就是层序遍历的那个_层_呢**?x度难度不就是x层么？有了这个讯息，我们就知道其实根据上面的广度优先的模板代码稍微修改一下，我们就可以得到第x层(x度)以内的好友了。

```java
public List<User> getXDegreeFriends(User user, int degree){
      List<User> results = new ArrayList<User>();
      Queue<User> queue = new LinkedList<User>();
      queue.add(user);
      //用于记录已经遍历过的User，因为A是B的好友，那么B也一定是A的好友，他们互相存在于对方的friends列表中。
      HashSet<User> visited = new HashSet<User>();
      //用一个counter记录当前的层数。
      int count = degree;
      //这里结束while循环的两个条件，一个是层数，一个是queue是否为空，因为万一该当前用户压根就没有那么多层的社交网络，比如他压根就没有朋友。
      while(count<=1 && !queue.isEmpty()){
          int queueSize = queue.size();
          for(int i = 0 ; i < queueSize; i++){
              User currentUser = queue.poll();
              //假如该用户已经遍历过，那么不做任何处理。
              if(!visited.contains(currentUser)){
                  results.add(currentUser);
                  queue.addAll(currentUser.getFriends();
                  visited.add(currentUser);
              }
          }
          count--;
      }

      return results;
}
```
就是这么简单！通过队列Queue，我们成功的把一个User的x度好友全部包在一个队列里面然后返回，这样我们就完成了一个简单的推荐好友方法！！。

同时一个小细节是我们使用HashSet这个数据结构来去重，为什么我们要多做这么一步呢？无论是广度还是深度优先搜索，这一步都可以说是重中之重，，因为我们在遍历节点的时候，会遇到重复已经遍历过的节点。比如：

![Screen Shot 2018-01-24 at 8.52.36 PM.png](http://upload-images.jianshu.io/upload_images/1777208-623edfd47e6c8807.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

A用户和B用户互为好友，所以他们的```getFriends()``` 方法会返回对方。

假设我们在代码中没有使用去重的数据结构的话，第一步放入A的时候，我们返回B加入队列，第二步我们调用B的```getFriends()```的时候又会返回A。。。。所以程序就会无限制的走下去了，同时层数也不正确了。所以我们遍历过的节点，一定要通过某种方式保存其相关信息防止重复遍历。







