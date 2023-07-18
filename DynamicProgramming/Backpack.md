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

2. 求組合沒有排列

```cpp
dp[j] += dp[j - nums[i]];
```

# Complete Backpack

物品可以多次選擇，內迴圈從小到大
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


