
上一遍文章我们过了一次广度优先算法，算是比较好理解的，因为模式比较固定，使用队列再进行```while()``` 循环，既可以满足大部分时候的需求。这一次我们来开始学习/复习一下我们的深度优先算法。深度优先算法其实在很多地方都可以应用到，其实在我的看法，只要搜索集合相对固定，并且使用到递归的算法都可以算是深度优先。而且在学习完回溯算法的很多题目之后，大家也可以更直观的体验到，很多时候回溯也是**暴力搜索**的一种程序上的实现而已。所以，

>1.回溯
>
>2.深度优先
>
>3.暴力搜索

这三种算法，名字虽然不同，但是都在某些情况下是有很大的共同成分的。大家在看完题目之后可以好好感受一下。


那么进入正题

*******

### 1.理解递归

如果是计算机专业的同学可能可以忽略这一小节。

[图片上传失败...(image-1a1a5f-1518421713974)]


其实递归的方法和一般的方法有什么区别呢？答案是完全没有，递归的方法和一般的方法完全没有区别。一个标准的方法/函数，都是需要在方法/函数栈里面进行调用和返回的。举个栗子。

```java
static void a(){
	b();
}

static void b(){
	c();
}


static void c(){
	System.out.println("methods")
}


public static void main(String[] args){
	a();
}


```


下面这段代码在方法栈中的执行过程如下两图所示。

![方法执行](http://upload-images.jianshu.io/upload_images/1777208-d2b8883a0d780adc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![方法结束](http://upload-images.jianshu.io/upload_images/1777208-c3e31451bf35ff62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图所示，所有的方法在执行结束之后都会```返回,return ```,这个return，代表的是return到该方法的调用者，也就是执行该方法的方法内，也就是上一层中。

理解了这个，递归也就很好理解，同样是使用方法/函数栈，只不过是调用的方法是相同的方法而已。


### 2.理解回溯

理解回溯，我们先从一个例题来看一下。
>我们有一个集合/列表，含有若干整数(不含有重复)，例如:{1，2，3}。 求该集合的全排列。
{1,2,3}
{1,3,2}
{2,1,3}
{2,3,1}
{3,1,2}
{3,2,1}

从直观的感觉来说，第一眼遇到这个题目，我们可以这么去抽象的构思:

>我们按照步骤/状态来抽象的话，我们每一刻都有**一个可用集合**，**一个答案集合**。每一步都需要从可用集合里面抽取一个元素加入到答案集合。在答案集合满了（或者是可用集合空了）的时候，代表我们获取了一个答案，这时需要```向后```，往可用集合内部退回元素。再重新做抽取的步骤，往答案集合中放置元素。

![1开头的集合答案](http://upload-images.jianshu.io/upload_images/1777208-415ac7eb1b9cc852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以看出来，我们每次获取答案都要向上退后一步，回到之前的```状态```，选取不同的元素放入结果集合。这个过程其实就是我们之前所讲的```回溯```。至于怎么样遍历集合，根据题目的要求我们可以有不同的策略，一般的回溯算法都是涉及列表的遍历，```for```循环足矣。

我们来看看代码

```java
public List<List> getPermutation(List<Integer> list){
    List result = new ArrayList<>();
    permutateHelper(result,new ArrayList<>(), list, new HashSet<Integer>());
    return result;
}

private void permutateHelper(List result, List<Integer> temp,List<Integer> list, HashSet<Integer> visited){
    //如果temp，temp答案集合满了，我们加入到最终的结果集合内。
    if(temp.size() == list.size()){
        result.add(new ArrayList(temp));
    }
    else{
        //直接使用for循环进行对原集合的遍历
        for(int i = 0; i< list.size();i++){
            //如果没有visit过，进行递归
            if(!visited.contains(list.get(i))){
                int current = list.get(i);
                temp.add(current);
                visited.add(current);
                //进入下一层递归
                permutateHelper(result,temp,list,visited);
                visited.remove(current);
                //这里需要直接remove掉最后一个元素，因为我们的全部的下一层递归已经结束，所以可以把该层的数字删掉，进入for循环的下一个遍历的开始了。这里这个remove的动作就是我们所谓的“回溯”
                temp.remove(temp.size()-1);      
            }
        }
    }

}

```
从以上代码我们可以看出，回溯算法其实就是普通的递归，但是加上了对集合遍历(for 循环)的过程，大家可以体会一下一个小小的区别。假如在以上的代码中，我们的temp不删除最后一个元素，改成这样：

```java
private void permutateHelper(List result, List<Integer> temp,List<Integer> list, HashSet<Integer> visited){
    if(temp.size() == list.size()){
        result.add(new ArrayList(temp));
    }
    else{
        for(int i = 0; i< list.size();i++){
            if(!visited.contains(list.get(i))){
                int current = list.get(i);
                temp.add(current);
                visited.add(current);
                //进入下一层递归,不删除最后一个元素，每次都创建一个新的temp列表
                permutateHelper(result,new ArrayList<>(temp),list,visited);
                visited.remove(current);
            }
        }
    }

}

```

简单的一行的修改，最后的答案也是对的(先不说这个修改浪费了多少空间)，可是却完全的改变了我们要的**回溯**的本质，没有向前退的这个动作，这个程序就变成了单纯的递归，暴力搜索了。

关于排列组合的题目，还有更加难的，比如集合中有重复元素怎么办，如果不只是求全排列，而是求子集呢？

有兴趣的同学可以看看leetcode上的题目:

>1.[Permutation II](https://leetcode.com/problems/permutations-ii/)
>2.[Subset](https://leetcode.com/problems/subsets/)

相信大家对所谓的回溯已经有点理解了。我们再来一个难一点点的题目。

### 3. 电话键盘

例题来自leetcode的一道关于电话键盘的[题目](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

![Screen Shot 2018-02-12 at 3.14.47 PM.png](http://upload-images.jianshu.io/upload_images/1777208-6d812d264d90b7d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个题目就是说，在手机上按几个数字键，对应可能产生的所有可能的字母的集合。比如在手机上按23，就是从{a,b,c}和{d,e,f}中各取一个放入答案集合中。
>这题和上一题的区别是，搜索集合不再是一个固定的大集合了，而是若干的小集合，每个数字对应一个小集合，满足搜索结果的答案的标准也不一样，不再是以集合的元素数量为标准，而是以我们的输入数字的数量为标准。

我们直接看代码


```java
public List<String> letterCombinations(String digits) {
		if (digits == null || digits.length() == 0) {
			return new ArrayList<>();
		}

                ///先初始化手机按键的数字和字母的关系
		String[] one = { "" };
		String[] two = { "a", "b", "c" };
		String[] three = { "d", "e", "f" };
		String[] four = { "g", "h", "i" };
		String[] five = { "j", "k", "l" };
		String[] six = { "m", "n", "o" };
		String[] seven = { "p", "q", "r", "s" };
		String[] eight = { "t", "u", "v" };
		String[] nine = { "w", "x", "y", "z" };
		String[] zero = { "" };

		HashMap<Integer, String[]> map = new HashMap<>();
		map.put(0, zero);
		map.put(1, one);
		map.put(2, two);
		map.put(3, three);
		map.put(4, four);
		map.put(5, five);
		map.put(6, six);
		map.put(7, seven);
		map.put(8, eight);
		map.put(9, nine);

		ArrayList<String> result = new ArrayList<>();
		int[] allNum = new int[digits.length()];
		for (int i = 0; i < digits.length(); i++) {
			allNum[i] = Integer.parseInt(digits.substring(i, i + 1));
		}
		phoneNumberHelper(result, new StringBuilder(), 0, allNum, map);
		return result;

	}

	private void phoneNumberHelper(ArrayList<String> result, StringBuilder current, int index, int[] allNum,
			HashMap<Integer, String[]> com) {
                //如果找到一个排列，加入答案中
		if (index == allNum.length) {
			result.add(current.toString());
			return;
		} else {
                        //使用for循环，遍历当前该数字的字母集合
			String[] possibilities = com.get(allNum[index]);
			for (int i = 0; i < possibilities.length; i++) {
				phoneNumberHelper(result, current.append(possibilities[i]), index + 1, allNum, com);
                                //一定要把StringBuilder的最后一位删除掉。
				current.deleteCharAt(current.length()-1);
			}
		}
	}


```
可以看出，回溯的题目并不难，在理解了排列组合题目之后，理解这个就简单一些了。我们对比一下和排列组合题目的不同。

>搜索集合在每一个状态都不一样，排列组合在每个步骤都是相同的搜索集合，电话按键根据按下的数字不一样，对应的字母不一样。

对于回溯算法的精髓，大家通过对两个题目的练习可以发现，就在于那个remove的动作，假如上面这个算法我改成这样，不使用StringBuilder，而直接使用Sting.

```java

public List<String> letterCombinations(String digits) {
		if (digits == null || digits.length() == 0) {
			return new ArrayList<>();
		}
		String[] one = { "" };
		String[] two = { "a", "b", "c" };
		String[] three = { "d", "e", "f" };
		String[] four = { "g", "h", "i" };
		String[] five = { "j", "k", "l" };
		String[] six = { "m", "n", "o" };
		String[] seven = { "p", "q", "r", "s" };
		String[] eight = { "t", "u", "v" };
		String[] nine = { "w", "x", "y", "z" };
		String[] zero = { "" };

		HashMap<Integer, String[]> map = new HashMap<>();
		map.put(0, zero);
		map.put(1, one);
		map.put(2, two);
		map.put(3, three);
		map.put(4, four);
		map.put(5, five);
		map.put(6, six);
		map.put(7, seven);
		map.put(8, eight);
		map.put(9, nine);

		ArrayList<String> result = new ArrayList<>();
		int[] allNum = new int[digits.length()];
		for (int i = 0; i < digits.length(); i++) {
			allNum[i] = Integer.parseInt(digits.substring(i, i + 1));
		}
		phoneNumberHelper(result, "", 0, allNum, map);
		return result;

	}

	private void phoneNumberHelper(ArrayList<String> result, String current, int index, int[] allNum,
			HashMap<Integer, String[]> com) {
		if (index == allNum.length) {
			result.add(current);
			return;
		} else {
			String[] possibilities = com.get(allNum[index]);
			for (int i = 0; i < possibilities.length; i++) {
                //不使用StringBuilder，直接使用String连接一个String，这个做法其实和new String()是一样的。创建了一个新的String，
				phoneNumberHelper(result, current + possibilities[i], index + 1, allNum, com);
			}
		}
	}


```

算法大部分都是相同的，但是直接直接使用String连接一个String，这个做法其实和new String()是一样的。创建了一个新的String，和我们的排列组合里面的new ArrayList<>(temp)这段代码一样，虽然最终结果没错，但是却丧失了回溯算法的本质和优势，浪费了空间。一行之差。


### 4.N皇后问题

这一篇的最后一个问题，当然非N皇后问题莫属，题目太经典，我就不浪费篇幅再写一次算法了。我这次就着重分析一下这个怎么把这个问题抽象成回溯问题。怎么把这个问题模型化，通俗点说，怎么把这个问题和排列组合问题找到相同的地方，方便大家理解。

我们每一次在棋盘上放棋子，其实就是从**原集合**，往**答案集合**中加入元素的一个动作。和排列组合问题不同的是，往答案集合里面加入元素这个动作不是每次都是合法的，而排列组合是无论怎么加都对。

举个栗子

![u=4191624954,1499040036&fm=27&gp=0.jpg](http://upload-images.jianshu.io/upload_images/1777208-e0a9627f4adcd30c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在放置第五个棋子的时候，我们在遍历的过程中需要判断第五个可以合法的放置在哪个位置，第一行？不行，因为第一个棋子也在第一行。第二第三第四同理。都通不过我们的检查。所以在for循环中要对之前已经放置的棋子做比较，看看能不能放置到我们想放置的位置，如果不行，那么我们需要**回退**到上一层。

所以N皇后问题最后的难点就在于，怎么表示放置棋子的位置？怎么做所谓的合法检查？这些大家可以自己思考一下再用java实现一下。

当我以前在复习N皇后问题的时候，我有那么一刹那和排列组合问题做了个比较，顿时恍然大悟。原来原理是相同的。


>这次的回溯算法的分析就到此位置，下一次我会做一个更全面的深度优先的算法分析。


祝大家新年快乐！！

![u=3534538955,3983762870&fm=27&gp=0.jpg](http://upload-images.jianshu.io/upload_images/1777208-5be0b9484157059f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




