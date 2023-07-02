# 131. Palindrome Partitioning


```cpp
    vector<vector<string>> result;
    vector<string> path;
    void backtracking(string &s, int startIndex)
    {
        if (startIndex >= s.size())
        {
            result.push_back(path);
            return;
        }

        for (int i = startIndex; i < s.size(); i++)
        {
            if (isPalindrome(s, startIndex, i))
            {
                string tmp = s.substr(startIndex, i - startIndex + 1);
                path.push_back(tmp);
            }
            else
            {
                continue;
            }

            backtracking(s, i + 1);
            path.pop_back();
        }
        return;
    }
    bool isPalindrome(string &s, int left, int right)
    {
        for (int i = left, j = right; i < j; i++, j--)
        {
            if (s[i] != s[j])
                return false;
        }
        return true;
    }
    vector<vector<string>> partition(string s)
    {
        result.clear();
        path.clear();
        backtracking(s, 0);
        return result;
    }
```

# 93. Restore IP Addresses

```cpp
 vector<string> result;
    void backtracking(string &s, int startIndex, int dotNum)
    {
        if (dotNum == 3)
        {
            if (isValid(s, startIndex, s.size() - 1))
            {
                result.push_back(s);
            }
        }

        for (int i = startIndex; i < s.size(); i++)
        {
            if (isValid(s, startIndex, i))
            {
                s.insert(s.begin() + i + 1, '.');
                dotNum++;
                backtracking(s, i + 2, dotNum);
                dotNum--;
                s.erase(s.begin() + i + 1);
            }
            else
                break;
        }
        return;
    }
    bool isValid(string &s, int start, int end)
    {
        if (start > end)
        {
            return false;
        }
        if (s[start] == '0' && start != end)
        {
            return false;
        }
        int num = 0;
        for (int i = start; i <= end; i++)
        {
            if (s[i] > '9' || s[i] < '0')
            {
                return false;
            }
            num = num * 10 + s[i] - '0';
            if (num > 255)
                return false;
        }
        return true;
    }
    vector<string> restoreIpAddresses(string s)
    {
        backtracking(s, 0, 0);
        return result;
    }


```