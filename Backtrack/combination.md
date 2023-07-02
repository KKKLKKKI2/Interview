# Key

1. Recursion is backtrack
2. Permutation has order, combination doesn't have order. 



# 216. Combination Sum III

## No trimming

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int k, int n, int startIndex, int sum)
    {
        if (sum == n && path.size() == k)
        {
            result.push_back(path);
            return;
        }
        if (path.size() > k)
            return;

        for (int i = startIndex; i <= 9; i++)
        {
            path.push_back(i);
            backtracking(k, n, i + 1, sum + i);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n)
    {
        result.clear();
        path.clear();
        backtracking(k, n, 1, 0);
        return result;
    }
```

## Trimming

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int k, int n, int startIndex, int sum)
    {
        if (sum > n)
            return;
        if (sum == n && path.size() == k)
        {
            result.push_back(path);
            return;
        }
        if (path.size() > k)
            return;

        for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++)
        {
            path.push_back(i);
            backtracking(k, n, i + 1, sum + i);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n)
    {
        result.clear();
        path.clear();
        backtracking(k, n, 1, 0);
        return result;
    }
```

# 39. Combination Sum
1. Since element in path can be duplicate, index send to recursion function don't need add one.
## No trimming

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &candidates, int target, int sum, int startIndex)
    {
        if (sum == target)
        {
            result.push_back(path);
            return;
        }
        if (sum > target)
        {
            return;
        }
        for (int i = startIndex; i < candidates.size(); i++)
        {
            path.push_back(candidates[i]);
            sum += candidates[i];
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum(vector<int> &candidates, int target)
    {
        result.clear();
        path.clear();
        backtracking(candidates, target, 0, 0);
        return result;
    }
```

## trimming

1. used equals to false means that number in the list is used at previous backtracking.

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &candidates, int target, int sum, int startIndex)
    {
        if (sum == target)
        {
            result.push_back(path);
            return;
        }
        if (sum > target)
        {
            return;
        }
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++)
        {
            path.push_back(candidates[i]);
            sum += candidates[i];
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum(vector<int> &candidates, int target)
    {
        result.clear();
        path.clear();
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);

        return result;
    }
```

# 40. Combination Sum II

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex, vector<bool>& used) {
        if (sum == target) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > 0 && candidates[i] == candidates[i - 1] && used[i - 1] == false) {
                continue;
            }
            sum += candidates[i];
            path.push_back(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used); // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            used[i] = false;
            sum -= candidates[i];
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);
        path.clear();
        result.clear();
        // 首先把给candidates排序，让其相同的元素都挨在一起。
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0, used);
        return result;
    }
};


```