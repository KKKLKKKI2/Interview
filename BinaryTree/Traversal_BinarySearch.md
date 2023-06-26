# Do it again
- [] 98 (basic)
- [] 235
- [] 236
- [] 450
## Key:
 


## 617. Merge Two Binary Trees

1.	if root1->left is NULL, root1->left = root2->left and so on.

### Recursion

```cpp
    TreeNode *mergeTrees(TreeNode *root1, TreeNode *root2)
    {
        if (!root1)
            return root2;
        if (!root2)
            return root1;

        TreeNode *cur = new TreeNode(0);
        cur->val = root1->val + root2->val;
        cur->left = mergeTrees(root1->left, root2->left);
        cur->right = mergeTrees(root1->right, root2->right);

        return cur;
    }
```
### iteration

```cpp
    TreeNode *mergeTrees(TreeNode *root1, TreeNode *root2)
    {
        if (!root1)
            return root2;
        if (!root2)
            return root1;
        queue<TreeNode *> q;
        q.push(root1);
        q.push(root2);

        while (!q.empty())
        {
            TreeNode *n1 = q.front();
            q.pop();
            TreeNode *n2 = q.front();
            q.pop();
            n1->val += n2->val;
            if (n1->left && n2->left)
            {
                q.push(n1->left);
                q.push(n2->left);
            }
            if (n1->right && n2->right)
            {
                q.push(n1->right);
                q.push(n2->right);
            }
            if (!n1->left && n2->left)
            {
                n1->left = n2->left;
            }
            if (!n1->right && n2->right)
            {
                n1->right = n2->right;
            }
        }
        return root1;
    }
```

# 700. BST

## Recursion

```cpp
    TreeNode *searchBST(TreeNode *root, int &val)
    {
		if(root ==NULL) return NULL;
		if(root->val == val) return root;
		else if(root->val > val) searchBST(root->left,val);
		else if(root->val < val) searchBST(root->right,val);
		return NULL;
    }
```

## Iteration

```cpp
    TreeNode *searchBST(TreeNode *root, int &val)
    {
		while(root!=NULL)
		{
			if(root->val == val) return root;
			if(root->val > val) root=root->left;
			else if(root->val < val) root= root->right;
			else return NULL;
		}
		return NULL;
	}
```

# 98. validate-Binary-search-TreeNode
inorder: left - compare - right
1. pre record deepest left leaf value
2. compre every node with pre
3. not only current node but also sub node needs to be BST

## Recursion
```cpp
    TreeNode *pre = NULL;
    bool isValidBST(TreeNode *root)
    {
        if (root == NULL)
            return true;
        bool left = isValidBST(root->left);
        if (pre != NULL && pre->val >= root->val)
            return false;
        pre = root;

        bool right = isValidBST(root->right);

        return left && right;
    }
	
```

## iteration
```cpp
        if (root == NULL)
            return true;
        stack<TreeNode *> st;
        TreeNode *pre = NULL;
        TreeNode *cur = root;
        while (cur != NULL || !st.empty())
        {
            if (cur != NULL)
            {
                st.push(cur);
                cur = cur->left;
            }
            else
            {
                cur = st.top();
                st.pop();
                if (pre != NULL && pre->val >= cur->val)
                {
                    return false;
                }
                pre = cur;

                cur = cur->right;
            }
        }

        return true;
```

# 530. Minimum Absolute Difference in BST
inorder traversal: left - calculate - right
1. calculate difference between previous and current
## Recursion
```cpp
    int result = INT_MAX;
    TreeNode *pre = NULL;
    void traversal(TreeNode *cur)
    {
        if (cur == NULL)
            return;
        traversal(cur->left);
        if (pre != NULL)
            result = min(result, cur->val - pre->val);
        pre = cur;
        traversal(cur->right);
    }
    int getMinimumDifference(TreeNode *root)
    {
        traversal(root);
        return result;
    }
```

## Iteration

```cpp
        stack<TreeNode *> st;
        TreeNode *cur = root;
        TreeNode *pre = NULL;
        int result = INT_MAX;
        while (cur != NULL || !st.empty())
        {
            if (cur != NULL)
            {
                st.push(cur);
                cur = cur->left;
            }
            else
            {
                cur = st.top();
                st.pop();
                if (pre != NULL)
                {
                    result = min(result, cur->val - pre->val);
                }
                pre = cur;

                cur = cur->right;
            }
        }
        return result;
```

501. Find Mode in Binary Search Tree
1. if cur == pre count++
2. if count == maxCount result.push_back(cur->val)
3. if count > maxCount result.clear(); result.push_back(cur->val)

## Recursion

```cpp
    int count = 0;
    int maxCount = 0;
    TreeNode *pre = NULL;
    vector<int> result;

    void searchBST(TreeNode *cur)
    {
        if (cur == NULL)
            return;
        searchBST(cur->left);

        if (pre == NULL)
        {
            count = 1;
        }
        else if (pre->val == cur->val)
        {
            count++;
        }
        else if (pre->val != cur->val)
        {
            count = 1;
        }

        if (count == maxCount)
        {
            result.push_back(cur->val);
        }
        if (count > maxCount)
        {
            maxCount = count;
            result.clear();
            result.push_back(cur->val);
        }
        pre = cur;

        searchBST(cur->right);
        return;
    }
    vector<int> findMode(TreeNode *root)
    {
        result.clear();
        searchBST(root);
        return result;
    }
```

## Iteration
```cpp
    vector<int> findMode(TreeNode *root)
    {
        int count = 0;
        int maxCount = 0;
        TreeNode *pre = NULL;
        TreeNode *cur = root;
        vector<int> result;
        result.clear();
        stack<TreeNode *> st;

        while (cur != NULL || !st.empty())
        {
            if (cur != NULL)
            {
                st.push(cur);
                cur = cur->left;
            }
            else
            {
                cur = st.top();
                st.pop();
                if (pre == NULL)
                {
                    count = 1;
                }
                else if (pre->val == cur->val)
                {
                    count++;
                }
                else if (pre->val != cur->val)
                {
                    count = 1;
                }

                if (count == maxCount)
                {
                    result.push_back(cur->val);
                }
                if (count > maxCount)
                {
                    maxCount = count;
                    result.clear();
                    result.push_back(cur->val);
                }
                pre = cur;
                cur = cur->right;
            }
        }
        return result;
    }
```
# 236. Lowest Common Ancestor of a Binary Tree

postorder traversal
1. left means result from left, so do the right.
2. if left != NULL and right != NULL means find common ancestor

```cpp
        if (root == NULL || root == p || root == q)
            return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        if (left != NULL && right != NULL)
            return root;
        if (left == NULL && right != NULL)
            return right;
        else if (left != NULL && right == NULL)
            return left;
        else
            return NULL;
```

# 235. Lowest Common Ancestor of a Binary Search Tree
all nodes are unique
1. common ancester must be bigger than q(p) or smaller than q(p)
2. traversal left if root are bigger than q and p, and so on.
## Recursion
```cpp
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root->val > p->val && root->val > q->val) {
            return lowestCommonAncestor(root->left, p, q);
        } else if (root->val < p->val && root->val < q->val) {
            return lowestCommonAncestor(root->right, p, q);
        } else return root;
    }
```
## Iteration
```cpp
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(root) {
            if (root->val > p->val && root->val > q->val) {
                root = root->left;
            } else if (root->val < p->val && root->val < q->val) {
                root = root->right;
            } else return root;
        }
        return NULL;
    }
```

# 701. Insert into a Binary Search Tree

1. traversal by binary search.
2. if root is null means find the position to add node.
## Recursion

```cpp
    TreeNode *insertIntoBST(TreeNode *root, int val)
    {
        if (root == NULL)
        {
            TreeNode *newNode = new TreeNode(val);
            return newNode;
        }
        if (root->val > val)
            root->left = insertIntoBST(root->left, val);
        if (root->val < val)
            root->right = insertIntoBST(root->right, val);

        return root;
    }
```

## Iteration

```cpp
	       if (root == NULL)
        {
            TreeNode *node = new TreeNode(val);
            return node;
        }
        TreeNode *cur = root;

        while (cur)
        {
            if (cur->val > val)
            {
                if (cur->left == NULL)
                {
                    TreeNode *node = new TreeNode(val);
                    cur->left = node;
                    break;
                }
                cur = cur->left;
            }
            else if (cur->val < val)
            {
                if (cur->right == NULL)
                {
                    TreeNode *node = new TreeNode(val);
                    cur->right = node;
                    break;
                }
                cur = cur->right;
            }
        }
        return root;
```



# 450. Delete Node in a BST
BST: handle five situations
1. NULL: retrun NULL
2. no children: delete current node
3. left child only: return left child
4. right child only: return right child
5. both children: cut left child and connect to very left of right child

```cpp
 TreeNode *deleteNode(TreeNode *root, int key)
    {
        if (root == NULL)
            return root;
        if (root->val == key)
        {
            if (!root->left && !root->right)
            {
                delete root;
                return NULL;
            }
            if (root->left && !root->right)
            {
                TreeNode *node = root->left;
                delete root;
                return node;
            }
            if (!root->left && root->right)
            {
                TreeNode *node = root->right;
                delete root;
                return node;
            }
            if (root->left && root->right)
            {
                TreeNode *cur = root->right;
                while (cur->left)
                {
                    cur = cur->left;
                }
                TreeNode *tmp = root;
                cur->left = tmp->left;
                tmp = tmp->right;
                delete root;
                return tmp;
            }
        }
        if (root->val > key)
            root->left = deleteNode(root->left, key);
        if (root->val < key)
            root->right = deleteNode(root->right, key);
        return root;
    }
```

# 669. Trim a Binary Search Tree
1. trim left child and connect right to parent if root->val < low
2. trim right child and connect left to parent if root->val > high
## Recursion
```cpp
    TreeNode *trimBST(TreeNode *root, int low, int high)
    {
        if (root == NULL)
            return root;

        if (root->val < low)
        {
            TreeNode *right = trimBST(root->right, low, high);
            return right;
        }
        if (root->val > high)
        {
            TreeNode *left = trimBST(root->left, low, high);
            return left;
        }

        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);

        return root;
    }
```

## Iteration

1. set root into range low < root < high
2. trim all left child smaller than low and go left
3. trim all right child larger than high and go right
```cpp
        while (root != NULL && (root->val < low || root->val > high))
        {
            if (root->val < low)
                root = root->right;
            else
                root = root->left;
        }
        TreeNode *cur = root;
        while (cur)
        {
            while (cur->left && cur->left->val < low)
            {
                cur->left = cur->left->right;
            }
            cur = cur->left;
        }
        cur = root;

        while (cur)
        {
            while (cur->right && cur->right->val > high)
            {
                cur->right = cur->right->left;
            }
            cur = cur->right;
        }
        return root;
```

# 108. Convert Sorted Array to Binary Search Tree

1. separate list into three parts: left, mid and right

## Recursion

```cpp
    TreeNode *traversal(vector<int> &nums, int left, int right)
    {
        if (left > right)
            return NULL;
        int mid = left + (right - left) / 2;
        TreeNode *node = new TreeNode(nums[mid]);

        node->left = traversal(nums, left, mid - 1);
        node->right = traversal(nums, mid + 1, right);
        return node;
    }
    TreeNode *sortedArrayToBST(vector<int> &nums)
    {
        return traversal(nums, 0, nums.size() - 1);
    }
```


## Iteration

```cpp
        if (nums.size() == 0)
            return nullptr;

        TreeNode *root = new TreeNode(0);
        queue<TreeNode *> nodeQ;
        nodeQ.push(root);
        queue<int> leftQ;
        leftQ.push(0);
        queue<int> rightQ;
        rightQ.push(nums.size() - 1);

        while (!nodeQ.empty())
        {
            TreeNode *cur = nodeQ.front();
            nodeQ.pop();
            int left = leftQ.front();
            leftQ.pop();
            int right = rightQ.front();
            rightQ.pop();
            int mid = left + (right - left) / 2;
            cur->val = nums[mid];

            if (left <= mid - 1)

            {
                cur->left = new TreeNode(0);
                leftQ.push(left);
                rightQ.push(mid - 1);
                nodeQ.push(cur->left);
            }
            if (right >= mid + 1)
            {
                cur->right = new TreeNode(0);
                nodeQ.push(cur->right);
                leftQ.push(mid + 1);
                rightQ.push(right);
            }
        }

        return root;
```

# 538. Convert BST to Greater Tree

traversal order: right - mid - left

## Recursion

```cpp
	int pre = 0;
private:
	void traversal(TreeNode* root)
	{
		traversal(root->right);
		root->val+=pre;
		pre = root->val;
		traversal(root->left);
	}
public:
    TreeNode *convertBST(TreeNode *root)
    {
        traversal(root);
        return root;
    }
```

## Iteration

```cpp
    void traversal(TreeNode *cur)
    {
        stack<TreeNode *> st;
        int pre = 0;
        while (cur != NULL || !st.empty())
        {
            if (cur != NULL)
            {
                st.push(cur);
                cur = cur->right;
            }
            else
            {
                cur = st.top();
                st.pop();
                cur->val += pre;
                pre = cur->val;
                cur = cur->left;
            }
        }
    }

public:
    TreeNode *convertBST(TreeNode *root)
    {
        traversal(root);
        return root;
    }
```