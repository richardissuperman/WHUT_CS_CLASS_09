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


### 2.走迷宫问题(最短距离问题)。

说到最短距离，我们第一反应肯定都是想到迪杰特斯拉算法。

![images.png](http://upload-images.jianshu.io/upload_images/1777208-c28db339ba7bc62b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在一个有向图或者无向图中，每一个节点与节点之间都有不同的权值(可以理解为距离)，最后算出每个点与点之间的最短距离与其相应的路径。

这次我们我们要学习的是这种算法的一个特例。也就是如果节点与节点之间权值相等，但是可能存在障碍物的情况。

最经典的就是走迷宫问题了。

![20170601163114443.png](http://upload-images.jianshu.io/upload_images/1777208-86c08b6699fa141b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

假设一个二维整型矩阵代表迷宫，0代表路，1代表墙壁(不能走不能通过),左上角是入口，右下角是出口(保证都为0)。求最少需要多少步可以走到出口。

对于这种问题，我们同样需要用广度优先来处理。为什么呢？

因为对于每一个节点来说，往下走一层都是需要一步(大家距离权值相等)，那么我们在走迷宫的过程其实就是像一个**决策树**一样，每一层都只需要一步来走完，那么终点的步数，其实就是取决于终点这个节点在这个树结构中的第几层。终点在第x层，就代表至少需要x步才能走到。

![20130816190242468.jpg](http://upload-images.jianshu.io/upload_images/1777208-05ac25d87efc6731.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>比如上图，从A出发，到其他节点的分层为
>1层: B，C，D 
>2层: F, E
>3层: H,G
>4层: I
其相应与A的距离也就是他们的层数。

所以在求迷宫问题的距离时，我们可以从起点开始做广度优先的遍历，不停的记录当前的层数，当遍历到终点的时候，查看当前已经遍历的层数，该层数也就是步数了。

```java
public int getMinSteps(int[][] matrix) {

		int row = matrix.length;
		int col = matrix[0].length;
		//迷宫可以走四个方向，这个二维数组代表四个方向的x与y的偏移量
		int[][] direction = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };
		HashSet<Integer> visited = new HashSet<>();
		Queue<Integer> queue = new LinkedList<>();
		//把起点加入到队列
		queue.add(0);
		int level = 0;
		while (!queue.isEmpty()) {
			//把该层的节点全部遍历
			int size = queue.size();
			for (int i = 0; i < size; i++) {
				int current = queue.poll();
				if (!visited.contains(current)) {
					visited.add(current);
					//确定该节点的x与y坐标
					int currentX = current / col;
					int currentY = current % col;
					//假如该点是重点，那么直接返回level
					if (currentX == matrix.length - 1 && currentY == matrix[0].length - 1) {
						return level;
					}
					//如果不是，那么我们分别把它的四个方向的节点都尝试加入到队列尾端，也就是下一层中
					for (int j = 0; j < direction.length; j++) {
						int tempX = currentX + direction[j][0];
						int tempY = currentX + direction[j][1];
						//因为1代表墙壁，我们不能走，只能加数值为0的点
						if (matrix[tempX][tempY] != 1) {
							int code = tempX * col + tempY;
							queue.add(code);
						}
					}
				}
			}
			level++;
		}
		return -1;
	}

```

以上代码就是简单的解决了迷宫问题中的最短路径，同时还可以帮助判断该迷宫到底有没有可行的路径到达出口(以上方法假如没有路径的时候会返回-1)，因为方法在进行while循环的时候，从起点开始所有能遍历的点都遍历过之后，我们还没有找到右下角的点，while循环会结束。



### 3.Multi-End 广度优先搜索(多重点广度优先)。

Multi-End 广度优先搜索和 之前的迷宫问题有点类似，我们只需要把上述条件改一改。

假如在这个迷宫里面有不止一个出口，那么我们从出口(坐标为0，0的点)开始，到达任何一个出口的最短路径该怎么求呢？

有朋友可能会说，假设有K个出口，那么我运行之前走迷宫的方法K次，比较最短路径不就行了。假设我们的节点有M*N个，这样的方式时间复杂度就是O(k*m*n)了。有没有更快一点的方法呢？


我们可以尝试着反向去思考这个问题，我们之前都是以起点为开始点，做广度优先搜索。对于多重点的问题，我们难道不可以用重点们作为起点，加入队列中，再不停更新道路的权值，直到我们找到起点不就行了么。

这个算法我就不具体展开了，有兴趣的朋友可以看看leetcode的[Gates and Wall](http://buttercola.blogspot.sg/2015/09/leetcode-walls-and-gates.html)这题

具体的[解答](https://discuss.leetcode.com/category/358/walls-and-gates)在这里





