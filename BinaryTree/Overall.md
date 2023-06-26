# Type of tree
1. full binary tree: n denote depth,  total 2^n - 1 nodes
2. complete binary tree: except last layer is not full, every other child are full. Not full child are one the left hand side.
3. binary search tree: right node bigger than root, left node smaller than root.
4. balanced tree: height difference of two child not larger than 1

# Must know
1. map, set, multimap and multiset are implement by binary search tree
2. unordered_set and unordered_map are implemnet by hash table

# traversal
1. inorder: left, [middle], right
2. preorder: middle, [left], right
3. postorder: left, [right], middle

# definition of tree
```cpp
	struct TreeNode{
		int val;
		TreeNode* left;
		TreeNode* right;
		TreeNode(int x): val(x), left(NULL), right(NULL) {}
	}
```

# Height
1. Height: nodes that start from some node to some leaf
2. use postorder traversal

# Depth
1. Depth: nodes that start from root to deepest leaf
2. use preorder traversal