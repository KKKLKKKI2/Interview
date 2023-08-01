# Do Again

# 121 

find the lowest buying price and highest selling price

1. dp[i][0] represents total cost when holding the stock on this day
- hold stock which buy on i - 1 day
- update lowest price if buying stock on i day

2. dp[i][1] represents total cost when not holding stock on this day
- remain no stock which is the same situation on i - 1 day
- sell stock on i day. And update pocket using yesterday buying cost + current selling price
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if (len == 0) return 0;
        vector<vector<int>> dp(len, vector<int>(2));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[len - 1][1];
    }
};
```

# 122

1. dp[i][0] represents total cost when holding the stock on this day
- hold stock which buy on i - 1 day
- buy stock on i day using yesterday earning money # main difference between 121 & 122 

2. dp[i][1] represents total cost when not holding stock on this day
- remain no stock which is the same situation on i - 1 day
- sell stock on i day. And update pocket using yesterday buying cost + current selling price


```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        vector<vector<int>> dp(len, vector<int>(2, 0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]); // 注意这里是和121. 买卖股票的最佳时机唯一不同的地方。
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[len - 1][1];
    }
};
```

# 123 

Buy and sell stock at most two times

Five status
- dp[i][0] represents initial status
- dp[i][1] represents holding money at first time. Buy stock using dp[i-1][0]
- dp[i][2] represents not holding money at first time. Earn money using dp[i-1][1]
- dp[i][3] represents holding money at second time. Buy stock using dp[i-1][2]
- dp[i][4] represents not holding money at second time. Earn money using dp[i-1][3]

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(5, 0));
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            dp[i][0] = dp[i - 1][0];
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }
        return dp[prices.size() - 1][4];
    }
};
```

# 188

Same logic as 123.
Calculate k times by loop

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {

        if (prices.size() == 0) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(2 * k + 1, 0));
        for (int j = 1; j < 2 * k; j += 2) {
            dp[0][j] = -prices[0];
        }
        for (int i = 1;i < prices.size(); i++) {
            for (int j = 0; j < 2 * k - 1; j += 2) {
                dp[i][j + 1] = max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i]);
                dp[i][j + 2] = max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i]);
            }
        }
        return dp[prices.size() - 1][2 * k];
    }
};
```

# 309

Cool down one day

Four status
- Holding stock (find maximum)
	1. Hold stock that buy yesterday
	2. Buy stock today using the money from __Remain no stock__ 
	3. Buy stcok today using the money from __Cool down__
- Selling stock
	1. Earn money from selling money plus money from __Holding stock__
- Cool down
	1. Update money from Selling stock
- Remain no stcok (find maximum)
	1. Update money from __Cool down__
	2. Update money from itself
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0) return 0;
        vector<vector<int>> dp(n, vector<int>(4, 0));
        dp[0][0] = -prices[0]; // 持股票
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], max(dp[i - 1][3] - prices[i], dp[i - 1][1] - prices[i])); // Holding stock
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3]); // Remain no stcok 
            dp[i][2] = dp[i - 1][0] + prices[i]; // Selling stock
            dp[i][3] = dp[i - 1][2]; // Update money from Selling stock
        }
        return max(dp[n - 1][3], max(dp[n - 1][1], dp[n - 1][2]));
    }
};
```

# 714

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        dp[0][0] -= prices[0]; // 持股票
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }
        return max(dp[n - 1][0], dp[n - 1][1]);
    }
};
```