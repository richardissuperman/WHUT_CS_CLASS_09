
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

最让我开心的是拿到了硅谷的offer！

>FaceBook Menlo Park总部的offer
>
>Amazon 西雅图总部 offer


在面试的过程中我深深的感受到，对于一个优秀的安卓开发来说，首先摆在第一位的还是他/她作为一个软件工程师的基本素养。无论你是做前端还是后端，最后定义你的优秀程度的还是作为软件工程师的基本素养，学习能力和编程能力，还有设计能力。我自己在现在的公司也做过面试官，发现新加坡的大部分码农(东南亚的码农)，对基础的编程能力实在是有所欠缺，熟练的使用API却不能理解为什么。

![](http://mmbiz.qpic.cn/mmemoticon/DhduwiaBa7lc7Ce8EBOGnFLkxgcvG88gsEjA990rfxViaLNIHOaFFCw7lCo5IfkVq4rWUibTwWZJ9k/0)

很多同学会在长久以往的业务逻辑开发中慢慢迷失，逐渐的把写代码变成了一种习惯，而没有再去思考自己代码的优化，结构的调整。这个现象不止是安卓开发的小伙伴有，任何大公司的朋友都会遇到。所以我这一系列的文章打算深入的讲解一下对于安卓程序员面试中可能遇到的算法。也希望能培养大家多思考，业余时间多动手写好代码，优质代码的习惯。

******************

那么第一篇我打算着重讲一下广度优先搜索和深度优先搜索。

### 1.深度优先搜索




![s](http://omu7tit09.bkt.clouddn.com/15006043873870.jpg)

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

其实递归对于很多数据结构来说，就是深度优先，比如二叉树，图。因为在递归的过程中，我们就是在一层一层的往下走，比如对于二叉树的中序打印来说，我们递归树的左节点，除非左节点为空，我们会一直往下走，这本身就是**深度优先**了。所以``一般来说``，对于深度优先，我们都会用递归来解决，因为写起来最方便。当然我们深度优先如果不想用递归，还可以使用``栈(Stack)``来解决，我们放在文章的最后来讲。

*************


好！相信我已经勾起了大家对大学算法课的记忆了！那么我们来巩固一下。使用分治思想+递归，我们就已经可以解决大部分二叉树的问题了。 我们来看一道题目->

### 1.2 翻转二叉树

这道题是一个经典的题目，Mac上著名软件[HomeBrew](https://brew.sh/)的作者曾经在面试Google的时候被问到了，还没做出来，因此最后被拒。。。。于是他在个人推特上保佑到:

>Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.

最后大家的关注点就慢慢从作者被拒本身转移到了题目上了。。。那我们看看这道题到底有多难。

****
翻转前

![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/Screen%20Shot%202017-12-24%20at%202.46.39%20PM.png?raw=true)



翻转后
![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/Screen%20Shot%202017-12-24%20at%202.46.39%20PM.png?raw=true)


看起来好像很麻烦的样子，每个子树本身都被翻转一遍。但是我们使用分治的思维，假如说我们有个函数，专门翻转二叉树的。假如我们把B子树翻转好，再把C子树翻转好，那么我们要做的岂不就是简单的把A节点的左赋给C(原来是B)，再把A节点的右赋给B(原来是C)。这个问题是不是就解决了？

对于B和C我们可以用同样的分治思维去递归解决。


![](https://github.com/richardissuperman/WHUT_CS_CLASS_09/blob/master/%E9%92%9F%E5%BA%86-%E9%AB%98%E9%98%B6%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93/images/%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.gif?raw=true)


















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








