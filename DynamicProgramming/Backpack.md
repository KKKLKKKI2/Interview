# 01 Backpack

1. 物品i放外迴圈正序遍歷，包包容量j放內迴圈倒序遍歷 => 保證物品i只會放入一次

```cpp
for(int i = 0; i< nums.size();i++
{
	for(int j = backSize; j>=nums[i];j--)
	{
		...
	}
}
```
## 轉移函式
1. 01背包求最大能裝多少

```cpp
dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]); 
```

2. 求有幾種組合/排列

```cpp
dp[j] += dp[j - nums[i]];
```

- 求組合範例

j = 背包容量 i = 物品

target = 4, i = nums[1,2,3]

```dp[j - nums[i]]```代表當j容量時，有```(j - nums[i]) + nums[i]```這種組合可以新增
ex: j = 3, nums[i] = 3

```dp[3] += dp[3 - 3]``` => dp[0] = 1('0') 加上使用nums[i]這個物品('3') => '03' => '3' 這個組合可以使用

ex: j = 3, nums[i] = 2

```dp[3] += dp[3 - 2]``` => dp[1] = 1('1') 加上使用nums[i]這個物品('2') => '12' 這個組合可以使用

ex: j = 1, nums[i] = 1

```dp[3] += dp[3 - 1]``` => dp[2] = 2('11','2') 加上使用nums[i]這個物品('1') => '111','21' 這個組合可以使用

3. 求背包容量內物品使用數量最少的狀況 

```cpp
 dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
```

## 遍歷方式

1. 物品在外迴圈，背包容量在內迴圈，求的是組合

```cpp
for(int i = nums.size();i<0;i++)
	for(int j = nums[i]; j <back.size();j++)
```

2. 背包容量在外迴圈，物品在內迴圈，求的是排列

```cpp
for(int j = nums[i]; j <back.size();j++)
	for(int i = nums.size();i<0;i++)

```

# Complete Backpack

物品可以多次選擇，內迴圈正序從小到大
```cpp
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

## template
	
### 2D array

```cpp

	vector<vector<int>> dp(items.size(),vector<int>(backWeight.size()+1,0));
	for(int i = items[0]; i< backWeight.size()+1;i++) // initialize item one situation
		{dp[0][i] = items[0]}
	for(int i = 1; i < items.size();i++)
	{
		for(int j = 0; j < backWeight.size()+1;j++)
		{
			if(j < items[i]) // back size is not afford for item[i]
			{	
				dp[i][j] = dp[i-1][j]; // previous row situation
			}
			else
			{
				// compare previous row situation or current value + (backWeight - weight[i]) situation value
				dp[i][j] = max(dp[i-1][j], dp[i - 1][j - weight[i]] + value[i]); 
			}
		}
	}
	cout<<dp[items.size()-1][backWeight - 1];
```

### 1D array

space optimiztion

```cpp
	vector<int> dp(backWeight.size() + 1; 0);
	
	for(int i = 0; i< items.size();i++)
	{
		for(int j = backWeight; j >= weight[i]; j--) // different
		{
			dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
		}
	}
	cout<<dp[backWeight.size()]<<endl;
```


# 416 

1. separate sum to two part and set target to sum divided by 2
2. if dp[target] equals to target means all numbers can separate to two part

2D 

```cpp
    bool canPartition(vector<int> &nums)
    {
        int sum = 0;
        for (int i : nums)
        {
            sum += i;
        }
        if (sum % 2 == 1)
        {
            return false;
        }
        int target = sum / 2;

        vector<vector<int>> dp(nums.size(), vector<int>(target + 1, 0));
        for (int i = nums[0]; i < target + 1; i++)
        {
            dp[0][i] = nums[0];
        }
        for (int i = 1; i < nums.size(); i++)
        {
            for (int j = 0; j < target + 1; j++)
            {
                if (nums[i] > j)
                {
                    dp[i][j] = dp[i - 1][j];
                }
                else
                {
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - nums[i]] + nums[i]);
                }

                if (dp[i][j] == target)
                    return true;
            }
        }

        return false;
    }
```
1D

```cpp
        int sum = 0;
        for (int i : nums)
        {
            sum += i;
        }
        if (sum % 2 == 1)
        {
            return false;
        }
        int target = sum / 2;
        vector<int> dp(target + 1, 0);

        for (int i = 0; i < nums.size(); i++)
        {
            for (int j = target; j >= nums[i]; j--)
            {

                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
                if (dp[j] == target)
                    return true;
            }
        }
        return false;
```

# 1049

1. separate all numbers into two parts, substraction of two parts is the answer
2. set target to sum divided by 2, dp[target] means the biggest sum that one of two parts can store.
3. sum - dp[target](part 1) - dp[target](part 2)

```cpp
    int lastStoneWeightII(vector<int> &stones)
    {
        int sum = 0;
        for (int i : stones)
        {
            sum += i;
        }
        int target = sum / 2;
        vector<int> dp(target + 1, 0);
        for (int i = 0; i < stones.size(); i++)
        {
            for (int j = target; j >= stones[i]; j--)
            {
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }

        return sum - dp[target] - dp[target];
    }
```


# 494

1. combination question


```cpp
dp[j] += dp[j - nums[i]]
```
dp[j-nums[i]] 代表"包包容量 - 加入的容量"之前的狀態
dp += dp[j - nums[i]] 代表"現在已有的狀態" + "包包容量 - 加入的容量"之前的狀態

dp[j - nums[i]] means the number of combination when back size minus nums[i] 
dp[0] = 1 means 0 is one situation

```cpp
    int findTargetSumWays(vector<int> &nums, int target)
    {
        int sum = 0;
        for (int i : nums)
        {
            sum += i;
        }
        if (abs(target) > sum)
            return 0;
        if ((target + sum) % 2 == 1)
            return 0;
        int basSize = (target + sum) / 2;
        vector<int> dp(basSize + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < nums.size(); i++)
        {
            for (int j = basSize; j >= nums[i]; j--)
            {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[basSize];
    }
```

# 474

1. one backpack with two dimenstion object
```cpp
	vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
	for (string s : strs)
	{
		int zeroNum = 0;
		int oneNum = 0;
		for (char c : s)
		{
			if (c == '0')
				zeroNum++;
			else
				oneNum++;
		}
		for (int i = m; i >= zeroNum; i--)
		{
			for (int j = n; j >= oneNum; j--)
			{
				dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
			}
		}
	}
	return dp[m][n];
```

# 518

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < coins.size(); i++) { // 遍历物品
            for (int j = coins[i]; j <= amount; j++) { // 遍历背包
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```

# 377

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 0; i <= target; i++) { // 遍历背包
            for (int j = 0; j < nums.size(); j++) { // 遍历物品
                if (i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```

# 322

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i < coins.size(); i++) { // 遍历物品
            for (int j = coins[i]; j <= amount; j++) { // 遍历背包
                if (dp[j - coins[i]] != INT_MAX) { // 如果dp[j - coins[i]]是初始值则跳过
                    dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
                }
            }
        }
        if (dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```

# 279

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) { // 遍历物品
            for (int j = i * i; j <= n; j++) { // 遍历背包
                dp[j] = min(dp[j - i * i] + 1, dp[j]);
            }
        }
        return dp[n];
    }
};
```

# 319
1. 求排列外背包內物品

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for (int i = 1; i <= s.size(); i++) {   // 遍历背包
            for (int j = 0; j < i; j++) {       // 遍历物品
                string word = s.substr(j, i - j); //substr(起始位置，截取的个数)
                if (wordSet.find(word) != wordSet.end() && dp[j]) {
                    dp[i] = true;
                }
            }
        }
        return dp[s.size()];
    }
};
```
