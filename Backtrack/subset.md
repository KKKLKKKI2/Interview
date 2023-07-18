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

# 332. Reconstruct Itinerary
1. unordered_map<string, map<string,int>> save("from","to","times)
2. backtracking function needs return boolean for checking the result.size() == ticketNum+1?

```cpp
    unordered_map<string, map<string, int>> targets;
    bool backtracking(int ticketNum, vector<string> &result)
    {
        if (result.size() == ticketNum + 1)
        {
            return true;
        }
        for (pair<const string, int> &target : targets[result[result.size() - 1]])
        {
            if (target.second > 0)
            {
                target.second--;
                result.push_back(target.first);
                if (backtracking(ticketNum, result))
                    return true;
                result.pop_back();
                target.second++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>> &tickets)
    {
        vector<string> result;
        result.clear();
        targets.clear();
        for (vector<string> &vec : tickets)
        {
            targets[vec[0]][vec[1]]++;
        }
        result.push_back("JFK");
        backtracking(tickets.size(), result);
        return result;
    }
```

# 51. N-Queens
1. every row, col, and diagonal should not have two queens
2. when n == row, push one condition of chessboard to result


```cpp
    vector<vector<string>> result;
    void backtracking(int n, int row, vector<string> &chessboard)
    {
        if (row == n)
        {
            result.push_back(chessboard);
            return;
        }
        for (int col = 0; col < n; col++)
        {
            if (isValid(row, col, chessboard, n))
            {
                chessboard[row][col] = 'Q';
                backtracking(n, row + 1, chessboard);
                chessboard[row][col] = '.';
            }
        }
    }
    bool isValid(int row, int col, vector<string> &chessboard, int n)
    {
        for (int j = row; j >= 0; j--)
        {
            if (chessboard[j][col] == 'Q')
                return false;
        }
        for (int j = row - 1, i = col + 1; j >= 0 && i < n; j--, i++)
        {
            if (chessboard[j][i] == 'Q')
                return false;
        }
        for (int j = row - 1, i = col - 1; j >= 0 && i >= 0; i--, j--)
        {
            if (chessboard[j][i] == 'Q')
                return false;
        }
        return true;
    }
    vector<vector<string>> solveNQueens(int n)
    {
        result.clear();
        vector<string> chessboard(n, string(n, '.'));
        backtracking(n, 0, chessboard);
        return result;
    }
```

# 37. Sudoku Solver

```cpp
class Solution {
private:
bool backtracking(vector<vector<char>>& board) {
    for (int i = 0; i < board.size(); i++) {        // 遍历行
        for (int j = 0; j < board[0].size(); j++) { // 遍历列
            if (board[i][j] == '.') {
                for (char k = '1'; k <= '9'; k++) {     // (i, j) 这个位置放k是否合适
                    if (isValid(i, j, k, board)) {
                        board[i][j] = k;                // 放置k
                        if (backtracking(board)) return true; // 如果找到合适一组立刻返回
                        board[i][j] = '.';              // 回溯，撤销k
                    }
                }
                return false;  // 9个数都试完了，都不行，那么就返回false
            }
        }
    }
    return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
}
bool isValid(int row, int col, char val, vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) { // 判断行里是否重复
        if (board[row][i] == val) {
            return false;
        }
    }
    for (int j = 0; j < 9; j++) { // 判断列里是否重复
        if (board[j][col] == val) {
            return false;
        }
    }
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == val ) {
                return false;
            }
        }
    }
    return true;
}
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};
```