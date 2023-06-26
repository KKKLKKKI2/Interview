# Recursion
1. Define function
```cpp
void traversal(TreeNode* cur, vector<int>& vec)
```
2. define terminate condition
```cpp
if(root == NULL) return;
```
3. define traversal
```cpp
	traversal(root->left, result); // left
	result.push_back(root->val);   // middle
	traversal(root->right, result);// right
```
## main function
```cpp
struct TreeNode()
{
	int val;
	TreeNode* left;
	TreeNode* right;
	TreeNode(int x): val(x), left(NULL), right(NULL){}
};
vector<int> main(TreeNode* root)
{
	vector<int> result;
	traversal(root, result);
	return result;	
}
```

## Inorder traversal function
```cpp
void traversal(TreeNode* root, vector<int>&result)
{
	if(root == NULL) return;
	traversal(root->left, result);
	result.push_back(root->val);
	traversal(root->right, result);
}
```
## Preorder traversal function
```cpp
void traversal(TreeNode* root, vector<int>&result)
{
	if(root == NULL) return;
	result.push_back(root->val);
	traversal(root->left, result);
	traversal(root->right, result);
}
```
## Postorder traversal function
```cpp
void traversal(TreeNode* root, vector<int>&result)
{
	if(root == NULL) return;
	traversal(root->left, result);
	traversal(root->right, result);
	result.push_back(root->val);
}
```

# Iteration

## Inorder traversal function
1. Traversal nodes and node dealing are not same node. Ex: traversal left node but dealing with middle node.
```cpp
void main(TreeNode* root)
{
	vector<int> result;
	stack<TreeNode*> st;
	TreeNode* cur = root;
	
	while(cur != NULL || !st.empty())
	{
		if(cur != NULL)
		{
			st.push(cur);
			cur = cur->left; // left
		}
		else
		{
			cur = st.top();
			st.pop;
			result.push_back(cur->val); //middle
			st.push(cur->right); // right
			cur = cur->right;
		}
	}
	return result;
}
```

## preorder traversal function
1. traversal and dealing are the same
```cpp
void main(TreeNode* root)
{
	vector<int> result;
	stack<TreeNode*> st;
	
	if(root==NULL) return result;
	st.push(root);
	
	while(!st.empty)
	{
		TreeNode* tmp = st.top();
		st.pop();
		result.push_back(tmp->val); // middle
		if(tmp->right) st.push(tmp->right); // left: first in stack means last to pop out;
		if(tmp->left) st.push(tmp->left); // right
	}
	return result;
}
```
## postorder traversal function
1. traversal and dealing are the same
reverse MRL become LRM(postorder)
Middle-Right-Left =reverse=> Left-Right-Middle

```cpp
void main(TreeNode* root)
{
	vector<int> result;
	stack<TreeNode*> st;
	
	if(root==NULL) return result;
	st.push(root);
	
	while(!st.empty)
	{
		TreeNode* tmp = st.top();
		st.pop();
		result.push_back(tmp->val); // middle
		if(tmp->left)  st.push(tmp->left); // right: first in stack means last to pop out
		if(tmp->right)  st.push(tmp->right); // left
	}
	
	reverse(result.begin(), result.end();
	return result;
}
```


# Unify method(inorder)
1. change order
2. push result into vector when meet node == NULL
```cpp
void main(TreeNode* root)
{
	vector<int> result;
	stack<TreeNode*> st;
	if(root!=NULL)st.push(root);

	while(!st.empty())
	{
		TreeNode *node = st.top();
		if(node!=NULL)
		{
			if(node->right)st.push(node->right); // inorder
			st.push(node);
			st.push(NULL);
			if(node->left)st.push(node->left);
		}
		else
		{
			st.pop();
			node = st.top();
			st.pop();
			result.push_back(node->val);
		}
	}
	return result;

}
```
