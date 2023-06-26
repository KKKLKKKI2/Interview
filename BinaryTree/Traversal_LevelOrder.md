# level-order traversal(BFS)
1. use queue to store node


## main function
```cpp
struct TreeNode()
{
	int val;
	TreeNode* left;
	TreeNode* right;
	TreeNode(int x): val(x), left(NULL), right(NULL){}
};
vector<vector<int>> main(TreeNode* root)
{
	vector<int> result
	queue<TreeNode*> q;
	if(root)q.push(root);
	
	while(!q.empty())
	{
		int size = q.size();
		vector<int> tmp;
		for(int i = 0; i< size; i++)
		{
			TreeNode* node = q.front();
			q.pop();
			tmp.push_back(node->val);
			if(node->left)q.push(node->left);
			if(node->right)q.push(node->right);
		}
		result.push_back(tmp);
	}
	return result;
}
```

## 102.Binary Tree Level Order Traversal

1. basic

### code
1. recursion
```cpp
    void order(TreeNode* cur, vector<vector<int>>& result, int depth)
    {
        if (cur == nullptr) return;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int depth = 0;
        order(root, result, depth);
        return result;
    }
```
## 107. Binary Tree Level Order Traversal II
1. reverse result in basic code

## 199. Binary Tree Right Side View
1. save every last node in same queue order

## 637. Average of Levels in Binary Tree
1. calucuate average level-by-level

## 429. N-ary Tree Level Order Traversal
1. same as basic, traversal every children in same node.

## 515. find-largest-value-in-each-tree-row
1. compare every number in same depth

## 116. populating-next-right-pointers-in-each-node
1. linked list. Link every node that in same depth

## 117.
1. same as 116;

## 104. Maximum Depth of Binary Tree
1. BFS: depth++ when every new depth is found
2. DFS: recursion return depth + 1 when root isn't null
## 111. Minimum Depth of Binary Tree
1. return depth when every left node and right node are NULL
