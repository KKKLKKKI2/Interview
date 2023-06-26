# 226. Invert Binary Tree

## DFS preorder

```cpp
        stack<TreeNode *> st;
        if (root)
            st.push(root);

        while (!st.empty())
        {
            TreeNode *node = st.top();
            if (node != NULL)
            {
                st.pop();
                if (node->right)
                    st.push(node->right);
                if (node->left)
                    st.push(node->left);
                st.push(node);
                st.push(NULL);
            }
            else
            {
                st.pop();
                node = st.top();
                st.pop();
                swap(node->left, node->right);
            }
        }

        return root;
```
## DFS inorder

```cpp
        stack<TreeNode *> st;
        if (root)
            st.push(root);

        while (!st.empty())
        {
            TreeNode *node = st.top();
            if (node != NULL)
            {
                st.pop();
				if (node->left)
                    st.push(node->left);
                st.push(node);
                st.push(NULL);									
                if (node->right)
                    st.push(node->right);
            }
            else
            {
                st.pop();
                node = st.top();
                st.pop();
                swap(node->left, node->right);
            }
        }

        return root;
```


## BFS

```cpp
        queue<TreeNode *> q;
        if (root)
            q.push(root);

        while (!q.empty())
        {
            int size = q.size();
            TreeNode *node;
            for (int i = 0; i < size; i++)
            {
                node = q.front();
                q.pop();
                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
                swap(node->left, node->right);
            }
        }
        return root;
```

## Recursion
``` cpp
        if (root = NULL)
            return root;
        swap(root->left, root->right)
            invertTree(root->left);
        invertTree(root->right);
        return root;
```