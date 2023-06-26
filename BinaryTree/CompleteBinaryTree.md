- [] 222
- [] 111
- [] 110
- [] 257
# Full Binary Tree
1. every node has two children

# Complete Binary Tree
1. nearly full binary tree, only some nodes have left children

# Balanced Binary Tree
1. height of children from same node that height difference lower than 1

# 110. Balanced Binary Tree

```cpp
    int getHeight(TreeNode *root)
    {
        if (root == NULL)
            return 0;
        int leftHeight = getHeight(root->left);
        if (leftHeight == -1)
            return -1;
        int rightHeight = getHeight(root->right);
        if (rightHeight == -1)
            return -1;

        return abs(leftHeight - rightHeight) > 1 ? -1 : max(leftHeight, rightHeight) + 1;
    }
    bool isBalanced(TreeNode *root)
    {
        return getHeight(root) == -1 ? false : true;
    }
```

# 222. Count Complete Tree Nodes
1. if left child height is equal to right child height(full sub tree), then number of node is 2^n-1
2. if left child hegiht is not equal to right child hegiht, then go left/right unitl reach full sub tree


```cpp
        if (root == NULL)
            return 0;

        TreeNode *left = root->left;
        TreeNode *right = root->right;
        int leftCount = 0;
        int rightCount = 0;

        while (left)
        {
            left = left->left;
            leftCount++;
        }
        while (right)
        {
            right = right->right;
            rightCount++;
        }
        if (leftCount == rightCount)
        {
            return (2 << leftCount) - 1;
        }

        return countNodes(root->left) + countNodes(root->right) + 1;
```

# 257. Binary Tree Paths



```cpp
    void traversal(TreeNode *root, vector<string> &result, string path)
    {
        path += to_string(root->val);
        if (root->left == NULL && root->right == NULL)
        {
            result.push_back(path);
            return;
        }
        if (root->left)
        {
            path += "->";
            traversal(root->left, result, path);
            path.pop_back();
            path.pop_back();
        }
        if (root->right)
        {
            path += "->";
            traversal(root->right, result, path);
            path.pop_back();
            path.pop_back();
        }
    }
    vector<string> binaryTreePaths(TreeNode *root)
    {
        vector<string> result;
        string path;
        if (root == NULL)
            return result;

        traversal(root, result, path);

        return result;
    }
	
```