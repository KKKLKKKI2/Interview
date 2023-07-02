- all need to do again

# subset 
1. restore every element through the path not the end

# 78. Subsets

1. restore every path to result 
```cpp
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums, int startIndex)
    {
        result.push_back(path);
        if (startIndex >= nums.size())
        {

            return;
        }
        for (int i = startIndex; i < nums.size(); i++)
        {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> subsets(vector<int> &nums)
    {

        backtracking(nums, 0);
        return result;
    }

```
# 90. Subsets II

1. use a vector to save previous used element

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums, int startIndex, vector<bool> used)
    {
        result.push_back(path);
        if (startIndex >= nums.size())
        {
            return;
        }
        for (int i = startIndex; i < nums.size(); i++)
        {
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false)
            {
                continue;
            }
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, i + 1, used);
            path.pop_back();
            used[i] = false;
        }
        return;
    }
    vector<vector<int>> subsetsWithDup(vector<int> &nums)
    {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, 0, used);
        return result;
    }
```

# 491. Non-decreasing Subsequences

1. The difference between 90. and this question is the nums list can't sort.
2. use a array to record numbers that used in this layer.
```cpp
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums, int startIndex)
    {
        if (path.size() > 1)
        {
            result.push_back(path);
        }
        int used[201] = {0};
        for (int i = startIndex; i < nums.size(); i++)
        {
            if ((path.size() > 0 && nums[i] < path.back()) || used[nums[i] + 100] == 1)
            {
                continue;
            }

            used[nums[i] + 100] = 1;
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> findSubsequences(vector<int> &nums)
    {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
```
# 46. Permutations

1. use a array to record used numbers
2. since every element will use multiple times, every i will not increase one when backtracking is called
```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums, vector<bool> &used)
    {
        if (path.size() == nums.size())
        {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (used[i] == true)
            {
                continue;
            }
            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
        return;
    }
    vector<vector<int>> permute(vector<int> &nums)
    {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }

```


# 47. Permutations II

1. remove duplicated by "if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false)"
```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums, vector<bool> used)
    {
        if (path.size() == nums.size())
        {
            result.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i++)
        {
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false)
            {
                continue;
            }
            if (used[i] == false)
            {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                used[i] = false;
                path.pop_back();
            }
        }
        return;
    }
    vector<vector<int>> permuteUnique(vector<int> &nums)
    {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end());
        backtracking(nums, used);
        return result;
    }
```