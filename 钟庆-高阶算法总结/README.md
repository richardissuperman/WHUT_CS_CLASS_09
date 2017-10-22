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

