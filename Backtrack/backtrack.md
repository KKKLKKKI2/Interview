# Key

1. Recursion is backtrack
2. Permutation has order, combination doesn't have order. 

Big O:
[link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E5%91%A8%E6%80%BB%E7%BB%93/20201112%E5%9B%9E%E6%BA%AF%E5%91%A8%E6%9C%AB%E6%80%BB%E7%BB%93.md)

# 77. Combinations

## No trimming

```cpp
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int n, int k, int startIndex)
    {
        if (path.size() == k)
        {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n; i++)
        {
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
        return
    }
    vector<vector<int>> combine(int n, int k)
    {
        result.clear();
        path.clear();
        backtracking(n, k, 1);
        return result;
    }
```

## Trimming optimization

```cpp
        for (int i = startIndex; i <= n - (k-path.size())+1; i++)
        {
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
```

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

#	17. Letter Combinations of a Phone Number


```cpp
    vector<string> result;
    string path;
    string letters[10] = {
        "",     // 0
        "",     // 1
        "abc",  // 2
        "def",  // 3
        "ghi",  // 4
        "jkl",  // 5
        "mno",  // 6
        "pqrs", // 7
        "tuv",  // 8
        "wxyz", // 9
    };
    void backTracking(string digits, int index)
    {
        if (path.size() == digits.size())
        {
            result.push_back(path);
            return;
        }
        int digit2Index = digits[index] - '0';
        string s = letters[digit2Index];
        for (int i = 0; i < s.size(); i++)
        {
            path.push_back(s[i]);
            backTracking(digits, index + 1);
            path.pop_back();
        }
        return;
    }
    vector<string> letterCombinations(string digits)
    {
        result.clear();
        path.clear();
        if (digits.size() == 0)
            return result;
        backTracking(digits, 0);
        return result;
    }
```

