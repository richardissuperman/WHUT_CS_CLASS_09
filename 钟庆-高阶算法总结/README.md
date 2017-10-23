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

* 需要用HashMap/HashSet记录后端信息从而更新状态的题目
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