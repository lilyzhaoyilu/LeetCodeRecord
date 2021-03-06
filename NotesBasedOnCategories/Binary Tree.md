# Binary Tree and BFS DFS

二叉树的高度是 h，也就是 logN。也就是说每条根到叶子的线路的时长为 logN。

需要把一个数一直除以 2 直到 1 的时间也是 logN

学习 https://github.com/azl397985856/leetcode/blob/master/thinkings/tree.md

- [Breadth First Search](#BFS-Breadth-First-Search)
- [Depth First Search](#Depth-First-Search)
- 题型分类
  - [搜索类题](#搜索类题)
  - [构建类题](#构建类题)
  - [修改类题](#修改类题)
- [二叉搜索树](#二叉搜索树)
  - [二叉搜索树特质一：便于查找](#二叉搜索树特质一便于查找)
  - [二叉搜索树特质二：中序遍历有序](#二叉搜索树特质二中序遍历有序)
  - [二叉搜索树衍生完全二叉树](#二叉搜索树衍生完全二叉树)
- [技巧和一些题型](#技巧和一些题型)
  - [技巧和一些题型-路径](#技巧和一些题型-路径)
  - [技巧和一些题型-序列化和反序列化](#技巧和一些题型-序列化和反序列化)
  - 边界

## BFS Breadth First Search

**BFS 的核心在于求最短问题时候可以提前终止。**  
层次遍历是一种不需要提前终止的 BFS 的副产物。
BFS 也不是树独有的。  
BFS 使用的是 queue， queue 是 FIFO

#### 复杂度

时间复杂度：O(n + m)
n 是点数, m 是边数
空间复杂度：O(n)

#### BFS 遍历（有层

```JavaScript
var BFSBinaryTree = function(root) {
  if(!root) return null;
  const queue = [root]
  while(queue.length > 0){
    let curLevelSize = queue.length
    for(let i = 0; i < curLevelSize; i++){
      const curNode = queue.shift();

      curNode.left && queue.push(curNode.left)
      curNode.right&& queue.push(curNode.right)
    }
  }

  return 我也不知道要return啥呢
};

```

#### BFS 遍历 双色标记 - 前中后序

新节点为白色，遇到白色的标记成为灰色并且将灰色的左右节点入栈  
如果遇到灰色，则直接将节点值输出
**用stack写标记颜色的顺序跟recursion写的正好相反就行了**

```JavaScript
var TwocolorInorder = function(root){
  if(!root) return [];

  const res = [];
  const stack = [[ 'WHITE', root]]

  while(stack.length){
    const [color, node] = stack.pop();
    if(!node) continue;

    if(color == 'WHITE'){
      stack.push(['WHITE', node.right])
      stack.push(['GRAY', node]) //中序遍历，调整他来调整遍历
      stack.push(['WHITE', node.left])
    }else{
      res.push(node.val)
    }
  }
  return res;
}

```

```JavaScript
var TwoColorPreorder = function(root) {
  if(!root) return [];

  const res = [];
  const stack = [[ 'WHITE', root]]

  while(stack.length){
    const [color, node] = stack.pop();
    if(!node) continue;

    if(color == 'WHITE'){
      stack.push(['WHITE', node.right])
      stack.push(['WHITE', node.left])
      stack.push(['GRAY', node]) //前序遍历，调整他来调整遍历 因为模拟出栈pop LIFO 所以前序放在最后 就是反过来了
    }else{
      res.push(node.val)
    }
  }
  return res;
};
```

## Depth First Search

DFS 使用栈，LIFO

前序遍历：打印节点，遍历所有左节点，然后遍历右节点；  
中序遍历：暂存节点，遍历所有左节点并且打印他们，打印暂存的中间节点，遍历右节点；  
后序遍历：暂存节点，遍历所有左节点并且打印他们，遍历所有右节点并且打印他们，打印剩下的暂存节点；  
- [DFS 通用](DFS-通用模板)  
- [树 DFS 模板](树-DFS模板)  
- [二叉树 DFS 模板](树-DFS模板)

#### DFS 通用模板

```JavaScript
const visited = {}
function dfs(i) {
	if (满足特定条件）{
		// 返回结果 or 退出搜索空间
	}

	visited[i] = true // 将当前状态标为已搜索
	for (根据i能到达的下个状态j) {
		if (!visited[j]) { // 如果状态j没有被搜索过
			dfs(j)
		}
	}
}
```

#### 树 DFS 模板 
因为树是无环图，所以可以不用visit

```JavaScript
function dfs(root) {
	if (满足特定条件）{
		// 返回结果 or 退出搜索空间
	}
	for (const child of root.children) {
        dfs(child)
	}
}
```

#### 二叉树 DFS 模板

```JavaScript
function dfs(root) {
	if (满足特定条件）{
		// 返回结果 or 退出搜索空间
	}
  ///前序遍历主逻辑，比如打印root.val
    dfs(root.left)
    dfs(root.right)
  //后序遍历主逻辑，比如打印
}
```

### 搜索类题

树的搜索类题只能用 BFS 或者 DFS 解；  
核心点：开始点，结束点，目标

### 构建类题

(二叉树搜索Lucifer)[https://lucifer.ren/blog/2020/02/08/%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%93%E9%A2%98/]
二叉树构建类题分三种：

1. 给你两种 DFS 的遍历的结果数组，让你构建出原始的树结构。
2. 给你一个 BFS 的遍历的结果数组，让你构建出原始的树结构。
3. 给你描述一种场景，让你构造一个符合条件的二叉树。

### 修改类题

1. 增加、删除节点或修改节点值/指向
2. 方便计算，给自己加指针(比如 parent)

### 二叉搜索树Bianry Search Tree

**二叉搜索树**
特质：

- 若左子树不空，则左子树上所有节点的值均小于它的根节点的值；
- 若右子树不空，则右子树上所有节点的值均大于它的根节点的值；
- 左、右子树也分别为二叉排序树；
- 没有键值相等的节点。

Binary Search Tree is a node-based binary tree data structure which has the following properties:   
   
The left subtree of a node **contains**(子节点的也算) only nodes with keys lesser than the node’s key.   
The right subtree of a node contains only nodes with keys greater than the node’s key.   
The left and right subtree each must also be a binary search tree.   




- 中序遍历  
二叉搜索树的中序遍历结果是一个有序列表。对先序遍历结果排序，排序结果就是中序遍历结果。
也可以根据先序遍历和中序遍历确定是同一颗树。


#### 二叉搜索树特质一：便于查找

当查找一个数的时候，每次排除大约一半的可能性（像二分法），所以搜索过程的时间复杂度就是 O(logN)。  
但树相对于数组而言，在添加和删除的时候时间都是 O(Height)；如果是平衡二叉搜索树，那么时间复杂度就是 O(logN)。而数组的添加和删除时间复杂度为 O(N)。

根据定义
如果一个node在左子树，那么它的取值范围(currentMin, root.val)   
如果一个node在右子树，那么它的取值范围(root.val, currentMax)



#### 二叉搜索树特质二：中序遍历有序
**二叉搜索树的中序遍历的结果是一个有序数组。**  
[中序遍历LC94更多遍历方法](https://github.com/lilyzhaoyilu/LeetCode-Notes/blob/master/Basic200II/LC94.%20Binary%20Tree%20Inorder%20Traversal.md)    

#### 中序遍历BFS模板
```JavaScript
var inorderTraversal = function(root) {
    const res = []
    if(!root) return res
    let node = root
    const stack = []

    while(stack.length || node){
        while(node){
            stack.push(node)
            node = node.left
        }
        node = stack.pop()
        res.push(node.val)
        node = node.right 
    }

    return res
};

```

等价拆解中序遍历题： 94（模板），98，173,250 


比如 98. 验证二叉搜索树   
1. 用定义法，递归检查每个node是否在它的取值范围内
2. 用拆解中序遍历法

再比如 99. 恢复二叉搜索树，官方难度为困难。题目大意是给你二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。 我们可以先中序遍历发现不是递增的节点，他们就是被错误交换的节点，然后交换恢复即可。这道题难点就在于一点，即错误交换可能错误交换了中序遍历的相邻节点或者中序遍历的非相邻节点，这是两种 case，需要分别讨论。



#### 二叉搜索树：衍生完全二叉树

1. 符合搜索二叉树的性质
2. 插入都是先填满最左 available spot
   我们可以给完全二叉树编号，这样父子之间就可以通过编号轻松求出。比如我给所有节点从左到右从上到下依次从 1 开始编号。那么已知一个节点的编号是 i，那么其左子节点就是 `2 * i + 1`，右子节点就是 `2 * i + 2`，父节点就是 `(i + 1) / 2`。   
3. 完全二叉树 complete binary tree: 满树节点数字是 `2^h - 1` 每一层是 `2 ^ level(rootLevel = 0)`

## 技巧和一些题型

#### 技巧和一些题型-路径

[LC124](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
[LC113]

#### 距离

[LC834]
[LC863]

#### 技巧和一些题型-序列化和反序列化

LC449 - DFS
LC889 BFS

#### 技巧和一些题型-构造二叉树

学习https://lucifer.ren/blog/2020/02/08/%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%93%E9%A2%98/

#### 双递归

[LC562]

#### 边界

1008

- 搜索
  - 空节点
  - 叶子节点 -构建
  - 参数扩展边界

#### 参数扩展法

- 携带父亲或者爷爷的信息
- 携带路径和
- 携带

1325
