Leetcode刷题总结




### Design类题目

[Design TinyURL](https://leetcode.com/problems/design-tinyurl/)

[Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

[Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/description/)

[Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/description/)

[LFU Cache](https://leetcode.com/problems/lfu-cache/description/)

[Design Twitter](https://leetcode.com/problems/design-twitter/description/)

### Two Pointer

[Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)

[3Sum](https://leetcode.com/problems/3sum/description/)

[Two Sum](https://leetcode.com/problems/two-sum/description/)

[Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/)

[Sort Colors](https://leetcode.com/problems/sort-colors/description/)

[Reverse Linked List](https://leetcode.com/problems/roman-to-integer/description/)

[Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/description/)

[Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

[Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

[Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)

[Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)

[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

[Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

[ Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/description/)

[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)
### Dynamic Programming

[Decode Ways](https://leetcode.com/problems/decode-ways/description/)

[Decode Ways II](https://leetcode.com/problems/decode-ways-ii/description/)

[Word Break](https://leetcode.com/problems/word-break/description/)

[Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/description/)

[Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

[Maximal Square](https://leetcode.com/problems/maximal-square/description/)

[Paint House](https://leetcode.com/problems/paint-house/description/)


[Paint House II](https://leetcode.com/problems/paint-house-ii/description/)

[ Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

[Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/description/)

[ Target Sum](https://leetcode.com/problems/target-sum/description/)


[Wildcard Matching](https://leetcode.com/problems/wildcard-matching/description/)

[Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)

[Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain/description/)

[Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/description/)

### Tree

[Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)


[Binary Tree Vertical Order Traversal ](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

[Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

[Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/description/)


[Add and Search Word - Data structure design](https://leetcode.com/problems/add-and-search-word-data-structure-design/description/)

[ Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)

[Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/description/)


[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/word-search/description/)

[Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

[Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/)

[Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

[Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)

[Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)

[Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/description/)

[Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

[Boundary of Binary Tree](https://leetcode.com/problems/boundary-of-binary-tree/description/)

[Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)

[Construct Binary Tree from String](https://leetcode.com/problems/construct-binary-tree-from-string/description/)

[Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/description/)

[Equal Tree Partition](https://leetcode.com/problems/equal-tree-partition/description/)

[Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/description/)

[Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)
>这题和算水平距离的那题不一样的地方是，水平距离重叠的部分算是一列，所以可以下一行的node的id可以用lastvalue+1/lastvalue-1,这里必须要用2*lastvalue/2*lastvalue+1.

### DFS/BFS

[Remove Invalid Parentheses](https://leetcode.com/problems/remove-invalid-parentheses/description/)

[Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/description/)

[Expression Add Operators](https://leetcode.com/problems/expression-add-operators/description/)

[Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/description/)

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

[Clone Graph](https://leetcode.com/problems/clone-graph/)


[Walls and Gates](https://leetcode.com/problems/walls-and-gates/description/)
>像这种多点BFS，就是多点更新距离的BFS，就直接把终点放进队列里面，每次更新相邻节点的距离值就好（更新的值一定是最优的），不需要每一个终点扩散做bfs，或者每个空点向终点做BFS，因为这样涉及值的更新(可能和另一个节点比距离更小，要更新)。

[Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/description/)

[Alien Dictionary](https://leetcode.com/problems/alien-dictionary/description/)

[Word Ladder](https://leetcode.com/problems/word-ladder/description/)

[Course Schedule II](https://leetcode.com/problems/course-schedule-ii/description/)

[Cut Off Trees for Golf Event](https://leetcode.com/problems/cut-off-trees-for-golf-event/description/)

[Minesweeper](https://leetcode.com/problems/minesweeper/description/)

[Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

[Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

### Union Find

[Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

[Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description/)

[Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/description/)

### BackTracking

[Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

[Subsets](https://leetcode.com/problems/subsets/description/)

[Subsets II](https://leetcode.com/problems/subsets-ii/description/)

[Word Search](https://leetcode.com/problems/word-search/description/)

### Binary Search

[First Bad Version](https://leetcode.com/problems/first-bad-version/description/)

[Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

[Pow(x, n)](https://leetcode.com/problems/powx-n/description/)

[Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)

[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

### String 操作

[Integer to English Words](https://leetcode.com/problems/integer-to-english-words/description/)


### HashMap

[Maximum Size Subarray Sum Equals k](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/description/)

[Group Anagrams](https://leetcode.com/problems/group-anagrams/description/)

[Brick Wall](https://leetcode.com/problems/brick-wall/description/)

[Contiguous Array](https://leetcode.com/problems/contiguous-array/description/)

[Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

[Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/description/)

[Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

[K-diff Pairs in an Array](https://leetcode.com/problems/k-diff-pairs-in-an-array/description/)


### Bit操作

[Add Binary](https://leetcode.com/problems/add-binary/description/)

[Multiply Strings](https://leetcode.com/problems/multiply-strings/description/)

[Total Hamming Distance](https://leetcode.com/problems/total-hamming-distance/description/)

[Hamming Distance](https://leetcode.com/problems/hamming-distance/description/)

### 排序

[Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)

[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/description/)

[Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)


[One Edit Distance](https://leetcode.com/problems/one-edit-distance/description/)

[Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)


[Meeting Rooms](https://leetcode.com/problems/meeting-rooms/description/)

[Insert Interval](https://leetcode.com/problems/insert-interval/description/)

[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

[Maximum Swap](https://leetcode.com/problems/maximum-swap/description/)

[ H-Index](https://leetcode.com/problems/h-index/description/)

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)

[Trapping Rain Water II](https://leetcode.com/problems/trapping-rain-water-ii/description/)

[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

[Third Maximum Number](https://leetcode.com/problems/third-maximum-number/description/)

[Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/description/)

[Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/description/)

### Stack
[ Exclusive Time of Functions](https://leetcode.com/problems/exclusive-time-of-functions/)

[Simplify Path](https://leetcode.com/problems/simplify-path/description/)

[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

[Min Stack](https://leetcode.com/problems/min-stack/description/)

[Baseball Game](https://leetcode.com/problems/baseball-game/description/)


## Other

[Roman to Integer](https://leetcode.com/problems/roman-to-integer/description/)

[Count and Say](https://leetcode.com/problems/count-and-say/description/)

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)

[Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

[Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/description/)

[Rotate Image](https://leetcode.com/problems/rotate-image/description/)

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)


[ Super Washing Machines](https://leetcode.com/problems/super-washing-machines/description/)

[Reverse Words in a String II](https://leetcode.com/problems/reverse-words-in-a-string-ii/description/)

[Rotate Array](https://leetcode.com/problems/rotate-array/description/)

[Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/description/)

[Count Primes](https://leetcode.com/problems/count-primes/description/)

[String to Integer ](https://leetcode.com/problems/string-to-integer-atoi/description/)

[Gray Code](https://leetcode.com/problems/gray-code/description/)

[Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/description/)

[Optimal Division](https://leetcode.com/problems/optimal-division/description/)

[Rotate Function](https://leetcode.com/problems/rotate-function/description/)

[Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/description/)

[Solve the Equation](https://leetcode.com/problems/solve-the-equation/description/)

### 疑难杂症

[ Sparse Matrix Multiplication](https://leetcode.com/problems/sparse-matrix-multiplication/description/)

[Read N Characters Given Read4 II - Call multiple times](https://leetcode.com/problems/read-n-characters-given-read4-ii-call-multiple-times/description/)

[Read N Characters Given Read4](https://leetcode.com/problems/read-n-characters-given-read4/)

[Find the celebrity](https://leetcode.com/problems/find-the-celebrity/)

[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)

[Random Pick Index](https://leetcode.com/problems/random-pick-index/)

[The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/description/)

[LRU Cache](https://leetcode.com/problems/lru-cache/)

[Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/description/)

[Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

[Text Justification](https://leetcode.com/problems/text-justification/description/)

[H-Index II](https://leetcode.com/problems/h-index-ii/description/)

[Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/description/)