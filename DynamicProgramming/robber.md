# Do Again
 337


# Key

# 198

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.size() - 1];
    }
};
```
# 213
Last home is connected to first home means there are two situations
1. considerate first home. The range is first home to last but two home.
2. considerate second home. The range is second home to last but one home.
3. larger number between two situations is the answer.

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        int result1 = robRange(nums, 0, nums.size() - 2); // 情况二
        int result2 = robRange(nums, 1, nums.size() - 1); // 情况三
        return max(result1, result2);
    }
    // 198.打家劫舍的逻辑
    int robRange(vector<int>& nums, int start, int end) {
        if (end == start) return nums[start];
        vector<int> dp(nums.size());
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start], nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
};
```

# 337

1. each node has two result, rob this node or not.
2. rob this node so the result of this node is cur + not rob left + not rob right
3. not rob this node so the result of node is max(not rob left, rob left) + max(not rob right, rob right)
4. return

```cpp
class Solution
{
public:
    vector<int> robber(TreeNode *cur)
    {
        if (cur == NULL)
            return vector<int>{0, 0};
        vector<int> left = robber(cur->left);
        vector<int> right = robber(cur->right);

        int val1 = cur->val + left[0] + right[0];

        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2, val1};
    }
    int rob(TreeNode *root)
    {
        vector<int> result = robber(root);
        return max(result[0], result[1]);
    }
};
```