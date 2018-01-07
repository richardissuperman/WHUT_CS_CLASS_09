


今年可谓是跌宕起伏的一年，幸好结局还算是圆满。开年的时候由于和公司CTO有过节，被"打入冷宫"，到下半年开始找工作，过程还是蛮艰辛。先分享一下offer的情况


国内的有
>1.阿里口碑(offer)
>
>2.Wish(offer)
>
>3.Booking(Offer)
>
>4.今日头条(Offer)
>
>5.Airbnb(北京)被拒



在面试的过程中我深深的感受到，对于一个优秀的安卓开发来说，首先摆在第一位的还是他/她作为一个软件工程师的基本素养。无论你是做前端还是后端，最后定义你的优秀程度的还是作为软件工程师的基本素养，学习能力和编程能力，还有设计能力。我自己在现在的公司也做过面试官，发现新加坡的大部分码农(东南亚的码农)，对基础的编程能力实在是有所欠缺，熟练的使用API却不能理解为什么。

![](http://mmbiz.qpic.cn/mmemoticon/DhduwiaBa7lc7Ce8EBOGnFLkxgcvG88gsEjA990rfxViaLNIHOaFFCw7lCo5IfkVq4rWUibTwWZJ9k/0)
很多同学会在长久以往的业务逻辑开发中慢慢迷失，逐渐的把写代码变成了一种习惯，而没有再去思考自己代码的优化，结构的调整。这个现象不止是安卓开发的小伙伴有，任何大公司的朋友都会遇到。所以我这一系列的文章打算深入的讲解一下对于安卓程序员面试中可能遇到的算法。也希望能培养大家多思考，业余时间多动手写好代码，优质代码的习惯。

******************

那么第一篇我打算着重讲一下二叉树的问题。

### 1.二叉树的递归（深度优先）处理



![s](http://upload-images.jianshu.io/upload_images/1777208-9c767c10de3c992e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

相信大家以前在学习算法与数据结构的时候都遇到过。比如说，打印二叉树前序，中序，后序的字符串这种问题。一般来说我们会选择使用递归的形式来打印，比如说

```java
/**
** 二叉树节点
**/
public class TreeNode{

	TreeNode left;
    TreeNode Right;
    int value;
}


//中序
public void printInoderTree(TreeNode root){
	//base case
	if(root == null){
    	return;
    }
    //递归调用printTree
    printTree(root.left);
    System.out.println(root.val);
    printTree(root.right);

}


//中序
public void printPreoderTree(TreeNode root){
	//base case
	if(root == null){
    	return;
    }
    //递归调用printTree
    System.out.println(root.val);
    printTree(root.left);
    printTree(root.right);

}

```

一开始上学的时候，我这几段代码都是背下来的，完全没有理解其中的奥妙。对于二叉树的递归操作，其实正确的理解方式

> *  **把每次递归想象成对其子集(左右子树)的一个操作，假设该递归已经可以处理好左右子树，那么根据已经处理好的左右子树在调整根节点。**

这样的思想其实和分而治之 **分治法** 相似，就是把一个大问题先分成小问题，再去解决。我们还是以二叉树的中序打印为例子。


>因为中序打印我们需要以左中右的顺序打印二叉树，以下图为例子我们分解一下问题。

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/inorder_hightlights.gif?raw=true)



上面这个gif详细的解释了怎么叫分而治之，首先，我们假设A节点的左右子树分开而且已经打印完毕，那么只剩下A节点需要单独处理，那么久打印它。对于B子树来说，我们以同样的思维处理。所以动图里面是B子树先铺平，然后轮到A节点，最后到C子树。

最后我们需要考虑一下这个递归的结束条件。我们假设A节点左右子树都为空，null,那么在调用该方法的时候我们需要在Node为空的时候直接返回不做任何操作。该条件我们一般称为递归的**Base Case**。每个递归都是这样，先想好我们怎么把问题``分治``， 再考虑``base case``是哪些，怎么处理，我们的递归就结束了。

>问题来了，我们明明要讲深度优先，为什么讲起递归了。两者的联系是什么？

其实递归对于很多数据结构来说，就是深度优先，比如二叉树，图。因为在递归的过程中，我们就是在一层一层的往下走，比如对于二叉树的中序打印来说，我们递归树的左节点，除非左节点为空，我们会一直往下走，这本身就是**深度优先**了。所以``一般来说``，对于深度优先，我们都会用递归来解决，因为写起来最方便。当然我们深度优先如果不想用递归，还可以使用``栈(Stack)``来解决，我们在以后的文章来讲(不过大家需要知道的是，递归本身就是使用方法栈的一种操作，联想一下我们常常听到的StackOverFlow,你应该能明白其中的奥妙了吧)。

*************


好！相信我已经勾起了大家对大学算法课的记忆了！那么我们来巩固一下。使用分治思想+递归，我们就已经可以解决大部分二叉树的问题了。 我们来看一道题目->

### 1.1 翻转二叉树

这道题是一个经典的题目，Mac上著名软件[HomeBrew](https://brew.sh/)的作者曾经在面试Google的时候被问到了，还没做出来，因此最后被拒。。。。于是他在个人推特上抱怨到:

>Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.

最后大家的关注点就慢慢从作者被拒本身转移到了题目上了。。。那我们看看这道题到底有多难。

****
翻转前

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/Screen%20Shot%202017-12-24%20at%202.46.39%20PM.png?raw=true)



翻转后
![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/Screen%20Shot%202017-12-24%20at%202.46.39%20PM.png?raw=true)


看起来好像很麻烦的样子，每个子树本身都被翻转一遍。但是我们使用分治的思维，假如说我们有个函数，专门翻转二叉树的。假如我们把B子树翻转好，再把C子树翻转好，那么我们要做的岂不就是简单的把A节点的左赋给C(原来是B)，再把A节点的右赋给B(原来是C)。这个问题是不是就解决了？


![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.gif?raw=true)

对于B和C我们可以用同样的分治思维去递归解决。用一段代码来描述一下


```java
public TreeNode reverseBinaryTree(TreeNode root){
	//先处理base case，当root ==null 时，什么都不需要做,返回空指针
    if(root == null){
    	return null;
    }
    else{
    	//把左子树翻转
    	TreeNode left = reverseBinaryTree(root.left);
        //把右子树翻转
        TreeNode right = reverseBinaryTree(root.right);
        //把左右子树分别赋值给root节点，但是是翻转过来的顺序
        root.left = right;
        root.right = left;
        //返回根节点
        return root;
    }
}

```

根据这个例子，再加上中序打印的题目，我们应该已经可以很轻松的理解到了，对于二叉树的题目或者算法，```分而治之``` 和 ```递归``` 的核心思想了，就是把左右子树分开处理，最后在把结果合并(把处理好的左右子树对应根节点进行处理)。

那么接下来我们来一个复杂一点点的题目


*******
### 1.2 把二叉树铺平

这个题目我们需要把一个二叉树变成一个类似于链表的结构，所有的子节点都移到右节点去，看图为例。

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/Screen%20Shot%202017-12-29%20at%205.53.29%20PM.png?raw=true)

转变之后

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/Screen%20Shot%202017-12-29%20at%205.55.26%20PM.png?raw=true)

从图中我们可以看出来，把二叉树铺平的这个过程，是先把**左子树铺平**，链接到根节点的右节点上面，再把**右子树铺平**,链接到已经铺平的左子树的最后一个节点上。最后返回根节点。那么我们从一个宏观的角度来说，需要做的就是先把左右子树铺平。

假设我们有一个方法叫```flatten()```，它会把一个二叉树铺平最后返回根节点

```java 
public TreeNode flatten(TreeNode root){    
}

```

那么从宏观的角度，我们对**铺平**这个操作，已经做完了！！！接下来就是第二步，还是以一个动画来阐述这个过程。

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/flatten.gif?raw=true)

最终代码如下，附上注释

```java 
public TreeNode flatten(TreeNode root){  
	
    //base case
    if(root == null){
    	return null;
    }
    else{
    	//用递归的思想，把左右先铺平
    	TreeNode left = flatten(root.left);
        TreeNode right = flatten(root.right);
        //把左指针和右指针先指向空。
        root.left = null;
        root.right = null;
        //假如左子树生成的链表为空，那么忽略它，把右子树生成的链表指向根节点的右指针
        if(left == null){
        	root.right = right;
            return root;
        }
        //如果左子树生成链表不为空，那么用while循环获取最后一个节点，并且它的右指针要指向右子树生成的链表的头节点
        root.left = left;
        TreeNode lastLeft = left;
        while(lastLeft != null && lastLeft.right != null){
        	lastLeft = lastLeft.right;
        }
        lastLeft.right = right;
        
        return root;
    }

}

```



至此，我们已经做完了这道题了，希望大家最后能好好理解我们所谓的分而治之的思想和二叉树中对左右子树递归的处理。大部分的二叉树算法题也就是围绕着这个思想为中心，只要从宏观上能把对左右子树处理的逻辑想清楚，那么就不难解决了。

### 1.3 安卓开发中遇到的树形结构？

那么对于安卓开发中，我们会不会遇到类似的问题呢？或者说安卓开发中会遇到树形结构的算法么？

答案是肯定有！


我们都知道在安卓系统里面，每个```ViewGroup```里面又会包含多个或者零个```View```,每一个```View``` 或者 ```ViewGroup``` 都有一个整型的Id，那么每次我们在使用```ViewGroup```的```findViewById(int id)```的时候，我们是以什么方式来查找并返回在当前ViewGroup下面，我们要查找的View呢？

这个也是我非常喜欢对来我司应聘的求职者的问题，不过很遗憾，目前为止能完完整整写出来的就一个。。。。(再次可见东南亚开发者的水平，不忍吐槽)

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQBFdfkIHlIavSVwduGlgDUrgRy_nvr1CQ1FS-uXslERqc0ikSFzA)


那么题目来了

请完成以下方法

```java

//返回一个在vg下面的一个View，id为方法的第二个参数
public static View find(ViewGroup vg, int id){


}

```

可以使用的方法有:

* ``` View -> getId() ``` 返回一个int 的 id
* ``` ViewGroup -> getChildCount() ``` 返回一个int的孩子数量
* ``` ViewGroup -> getChildAt(int index) ``` 返回一个孩子，返回值为View。




这个题目就可以说非常经典了，以往的树形结构的题目，我们都是做一个二叉树的处理，除了左就是右，但是这里我们每个ViewGroup都可能有多个孩子，每个孩子既可能是ViewGroup，也可能只是View(ViewGroup是View的子类，这里是一个知识点！)

我这里就不做过多的解释了，直接贴代码，而且安卓系统本身也是用这种方式进行View的查找的。

```java

//返回一个在vg下面的一个View，id为方法的第二个参数
public static View find(ViewGroup vg, int id){
	if(vg == null) return null;
    int size = vg.getChildCount();
    //循环遍历所有孩子
    for(int i = 0 ; i< size ;i++){
    	View v = vg.getChildAt(i);
        //如果当前孩子的id相同，那么返回
        if(v.getId == id) return v;
        //如果当前孩子id不同，但是是一个ViewGroup，那么我们递归往下找
        if(v instance of ViewGroup){
        	//递归
        	View temp = find((ViewGroup)v,id);
            //如果找到了，就返回temp，如果没有找到，继续当前的for循环
            if(temp != null){
            	return temp;
            }
        }
    }
    //到最后还没用找到，代表该ViewGroup vg 并不包含一个有该id的孩子，返回空
    return null;
}

```



### 2.二叉树的层序处理(广度优先)


![](http://omu7tit09.bkt.clouddn.com/15006053431729.jpg)


说到广度优先，大部分同学可能会想到图，不过毕竟树结构本身就是一种特殊的图。所以一般说树，尤其是二叉树的广度优先我们指的一般是层序遍历。

比如说树

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/Screen%20Shot%202017-12-29%20at%205.53.29%20PM.png?raw=true)

层序打印的结果就是```A->B->C->D->D->E->F->G```


对于层序遍历的相关算法，真理只有一个！

![](http://p3.ifengimg.com/auto/wemedia/2016/0725/c5d73c52661d47d3a9590c94fff5d69f_q70.jpeg)

**就是用```队列(Queue)```！**



道理很简单，每次遍历当前节点的时候，把该节点从队列拿出来，并且把它的子节点全部加入到队列中。over~

上一个简单的打印代码

```java
public void printTree(TreeNode root){
	if(root == null){
    	return;
    }
	Queue queue = new LinkedList();
    queue.add(root);
    while(!queue.isEmpty()){
    	TreeNode current = queue.poll();
        System.out.println(current.toString());
        if(current.left != null){
        	queue.add(current.left);
        }
        if(current.right != null){
        	queue.add(current.right);
        }
    }

}

```

这段代码很简单，利用队列先进先出的性质，我们可以一层层的打印二叉树的节点们。


所以对于二叉树的层序遍历来说，一般都会使用队列，这都是套路。因此，二叉树的层序遍历相对来说比较简单，大家下次见到二叉树的层序遍历相关的面试题，先大胆的和面试官说出你打算使用队列，肯定没错！

![](http://5b0988e595225.cdn.sohucs.com/images/20170913/639d7f3aa08a462f98ea5909a5cfc618.jpg)




最后对于层序遍历来说我们再来一个比较具有代表性的题目！



### 2.1 链接二叉树的Next节点


这个题目要求大家在拥有一个二叉树节点的左右节点指针之余，还要帮它找到它的next指针指向的节点。

大概是这样：

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/Screen%20Shot%202017-12-29%20at%205.53.29%20PM.png?raw=true)

在上面这个图中，红色的箭头代表next指针的指向

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/btree/Screen%20Shot%202017-12-29%20at%205.53.29%20PM%20copy.png?raw=true)

**逻辑很简单，每一个的节点的next指向同一层中的下一个节点，不过如果该节点是当前层的最后一个节点的话，不设置next，或者说next为空。**


其实这个题目就是典型的层序遍历，使用队列就可以轻松解决，每次poll出来一个节点，判断是不是当前层的最后一个，如果不是，把其next设置成queue中的下一个节点就ok了。至于怎么判断当前节点是哪一层呢？我们有个小技巧，使用当前queue的size做for循环，且看代码


```java

public void nextSibiling(TreeNode node){

	if(node == null){
    	return;
    }
	
    Queue queue = new LinkedList();
    queue.add(node);
    //这个level没有实际用处，但是可以告诉大家怎么判断当前node是第几层。
    int level = 0;
    while(!queue.isEmpty()){
    	int size = queue.size();
        //用这个for循环，可以保证for循环里面对queue不管加多少个子节点，我只处理当前层里面的节点
        for(int i = 0;i<size;i++){
        		//把当前第一个节点拿出来
            	TreeNode current = queue.poll();
                //把子节点加到queue里面
                if(current.left != null){
                	queue.add(current.left);
                }
                if(current.right != null){
                    queue.add(current.right);
                }
                
        		if(i != size -1){
               
                	//peek只是获取当前队列中第一个节点，但是并不把它从队列中拿出来
                	current.next = queue.peek();
                
                }
            }
        }
        
        level++;
    }
}

```



>二叉树的知识点我就大概讲这些，下次的文章我会接着详细的讲深度优先和广度优先的算法。深度优先是一个非常非常宽泛而且难以完全掌握的知识点，我会用详细的篇幅来覆盖所有的深度优先的基本题型，包括对树，图的深度优先搜索，集合的回朔等等。

### 题目链接

 * [铺平二叉树](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/) 
 * [翻转二叉树](https://leetcode.com/problems/invert-binary-tree/) 














# 高阶算法总结(数据结构篇)


### 并查集Union Find

合并集合，使用某一个点作为该集合的key。

模板代码

```java
//查找的代码。
/**这是使用递归的做法来做压缩路径。
**/
public int find(int x){
	if(father[x] == x){
    	return x;
    }
    else{
    	father[x] = find(father[x])
        return father[x];
    }
}

```

```java
//合并的算法，配合上压缩路径可以做到O(1)
public void union(int x, int y){
	int first = find(x);
    int second = find(y);
    if(first != second){
    	//把y的父节点指向x的父节点,注意这里不是把father[y] = first.
    	father[second] = first;
    }
}

```

>一般来说并查集适合做频繁插入节点的计算，如果只是一次运算其实和BFS宽度优先没有本质上速度的提升，比如[number of island](https://leetcode.com/problems/number-of-islands/description/)就没区别，但是[number of island II](https://leetcode.com/problems/number-of-islands-ii/description/)就需要用并查集来提高效率了.有些没必要用并查集的就用BFS/DFS还比较方便理解。





### Trie（字典树）

一种适合做频繁查询，和做前缀查询的(AutoComplete Search)的数据结构，相比起普通的用哈希表保存字符串的方式，这种数据结构会占用更少的空间。

```java
//插入的代码
public void insert(String word){
	Node currentNode = head;
    char[] words = word.toCharArray();
    for(int i = 0; i < words.length; i++){
    	char currentChar = words[i];
        //如果当前节点已经有改孩子了，也就是这个char已经存在，那么直接跳到该节点
        if(node.has(currentChar)){
        	currentNode = node.getChild(currentChar);
        }
        else{
        	//如果没有，那么创建该节点，然后再跳到该节点。
        	Node newNode = new Node(currentChar);
            node.addChild(newNode);
            currentNode = newNode;
        }
    }
    //结尾需要给最后一个叶子节点打上tag。
    currentNode.hasWord = true;

}

```



```java
//查询的代码
public void search(String word){
	char[] chars = word.toCharArray();
    Node current = head;
    for(int i = 0; i < chars.length ;i++){
    	if(current.getChild(chars[i])!=null){
        	current = current.getChild(chars[i]);
        }
        else{
        	return false;
        }
    }
    //返回叶子节点之后其实还需要判断hasWord这个值是否为true，如果是的话那么是一个完整的词汇，不然仅仅是preflix前缀
    return current;

}

```
例题

* 1.[ Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

***********


# 高阶算法总结(双指针篇)



### 求第K大问题，快速选择(Quick Select)

>快速选择用于求第K大的问题，可以用于求中位数。使用快速排序quick select的partition思想，每次把数组分割成两堆从而求解。Partition的代码是关键，如何在数组中进行分割，交换是代码的核心部分。

```java
public int partition(int[] nums){
    //使用第一个数字作为pivot点，可以使用随机算法选择，这样效果更好
	int pivot = nums[0];
    int start = 0;
    int end = nums.length-1;
    //开始分割
    //跳过所有大于pivot的点，找到从右向左第一个比pivot小的数字
    
    while(start<end){
    	while(start<end && nums[end]>=pivot){
    		end--;
    	}
    	nums[start] = nums[end];
    
    	while(start<end && nums[start]<=pivot){
    		start++;
    	}
    	nums[end] = nums[start];
    }
    nums[start] = pivot
    return start;
}

```

该代码确定了partition的范围为全局，其实我们在方法的签名中可以限定partition的范围，这样可以用于接下来的操作，比如快速排序等等。有了这个**partition**方法,我们可以轻松的求解第K大问题。


### Sliding window 问题

>Sliding window问题一般是在一个数组或者集合里面，通过维护一个一定大小（或者不确定的大小）的的窗口，计算在该窗口内的一些情况，比如说合，或者最小值，或者最最大值，或者中位数等等。这种题目一般分三种做法。

* 1.双指针类型
* [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/) 求移动窗口内的和，满足大于或者小于一个数K。

可以用该模板代码求解

```java
//用这个for+while型模板会好写一些。
for(int i = 0;i < nums.length ;i++){
	int j = 0;
    while(j<nums.length){
    	j++;
        if(满足条件){
        	//如果满足一些情况的话，比如和大于或者小于某个值
        }
        else{
        	break;
        }
    }
}

```

* 2.用PriorityQueue解决
* [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/description/)
* [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/description/)

>维护一个K大小的窗口，其实是两个PriorityQueue(一个最大堆一个最小堆)加一个中位数，每次新加入一个数nums[i],先检查一下看应该放入哪边的PriorityQueue，然后更新中位数。

```java
//larger是一个最小堆，也就是正常的序列。
PriorityQueue larger = ....;
//smaller是最大堆，也就是反向的序列。因为每次我都需要比较larger里面最小和smaller里面最大，因为他们最靠近mid
PriorityQueue smaller = ....;

//实际情况其实更复杂。。。这里随便写写
for(int i = 0;i< nums[i];i++){
	//要先把最后一个数移除，这里操作也是logK
    larger/smaller.remove(nums[i-k]);
	if(nums[i]>mid){
    	mid = nums[i];
        //这里操作也是logK
        smaller.add(mid);
    }else{
    	//这里操作也是logK
    	larger.add(mid);
        mid = nums[i];
    }
}

```

* 3.需要用HashMap/HashSet记录后端信息从而更新状态的题目
* [Longest Substring Without Repeating Characters](http://www.lintcode.com/en/problem/longest-substring-without-repeating-characters/)
* [Find All Anagrams in a String](https://leetcode.com/problems/group-anagrams/description/)
* [Minimum Window Substring](http://lintcode.com/en/problem/minimum-window-substring/)

```java
//结合双指针的for循环加while loop，加上HashSet
 public int lengthOfLongestSubstring(String s) {
		// write your code here
		if(s == null || s.length() == 0) {
			return 0;
		}
		char chars[]  = s.toCharArray();
		HashSet<Character> set = new HashSet<>();
		int res = Integer.MIN_VALUE;
		int j = 0;
		for(int i = 0; i< chars.length ;i++) {
			while(j<chars.length) {
				if(!set.contains(chars[j])) {
					set.add(chars[j]);
					if(j-i+1>res) {
						res = j-i+1;
					}
					j++;
				}
				else {
					break;
				}
			}
			set.remove(chars[i]);
		}
		return res;
	}
```

然后Find All Anagrams in a String 和Minimum Window Substring其实是一模一样的题目,只不过anagram就是一个限定了长度的minimum window substring而已，这里每次做比较的时候，其实比较的是两个hashmap，如果我们移动窗口的hashmap包含了目标的hashmap，比如：

>我们要查找"abc"(a->1,b->1,c->1),但是target是"abac"(a->2,b->1,c->1)，那么就是符合标准的。因为target hashmap每一个entry，source都有，而且大于等于其数量，以为这该窗口包含了target string。
>比较的时候可以不需要用hashmap，可以用数组

```java
 boolean valid(int []sourcehash, int []targethash) {
        
        for(int i = 0; i < 256; i++) {
            if(targethash[i] > sourcehash[i])    
                return false;
        }
        return true;
    }

```

优化的做法:
可以不需要用hashmap或者数组遍历，可以使用counter解决：




### 动态规划问题

简单类型的动态规划，属于典型的前者的状态决定后者的状态，当前的state(往往是什么最大值最小值之类的)state[i]，可以是由state[i-1]-----state[0]决定的，所以一般这种算法最差也是O(n^2).

>例题1.house robber，每次不能偷相邻的
>状态转移方程为：

```java
//每次需要比较前一个和前两个加当前值(模拟只偷不相邻这个过程)
f[i] = max(f[i-1], f[i-2] + A[i-1]);
```

>例题2.编辑距离
>两个数字的编辑距离也是动态规划，可以用正序动态规划来做。



![](http://www.dreamxu.com/books/dsa/dp/images/2014-11-05_133201.svg)


第二种类型是记忆化搜索动态规划，这类动态规划需要配合深度优先或者广度优先，（一般是对一个地图，例如二维数组进行搜索），然后通过另一个数组进行记录。

>例题3 [ Longest Increasing continuous Subsequence ](https://aaronice.gitbooks.io/lintcode/content/dynamic_programming/longest_increasing_continuous_subsequence_ii.html)


需要通过记录已经走过的路径的最大值。保存在hashmap或者一个数组里面




    public int longestIncreasingContinuousSubsequenceII(int[][] A) {
        if (A == null || A.length == 0 || A[0].length == 0) {
            return 0;
        }
        M = A.length;
        N = A[0].length;
        dp = new int[M][N];
        flag = new int[M][N];

        int ans = 0;

        for (int i = 0; i < M; i++) {
            for (int j = 0; j < N; j++) {
                dp[i][j] = search(i, j, A);
                ans = Math.max(ans, dp[i][j]);
            }
        }

        return ans;
    }

    // Memorized search, recursive
    public int search(int x, int y, int[][] A) {
    
		//这里是关键
        if (flag[x][y] != 0) {
            return dp[x][y];
        }

        int ans = 1; // Initialize dp[x][y] = 1 if it's local min compare to 4 directions
        int nx, ny;
        for (int i = 0; i < dx.length; i++) {
            nx = x + dx[i];
            ny = y + dy[i];
            if (insideBoundary(nx, ny) && (A[x][y] > A[nx][ny])) {
                ans = Math.max(ans, search(nx, ny, A) + 1);
            }
        }

        flag[x][y] = 1;
        dp[x][y] = ans;

        return ans;
    }

    public boolean insideBoundary(int x, int y) {
        return (x >=0 && x < M && y >= 0 && y < N);
    }
}


```








