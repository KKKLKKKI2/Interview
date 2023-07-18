# Key

1.From local optimization to find global optimization

# 455. Assign Cookies

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int index = s.size() - 1;
        int result = 0;
		for(int i = s.size()-1; i >= 0;i--)
		{
			if(index>=0&&s[index] >= g[i])
			{
				index--;
				result++;
			}
		}
		return result;
	}
};
```

# 376. Wiggle Subsequence

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        int curDiff = 0; // 当前一对差值
        int preDiff = 0; // 前一对差值
        int result = 1;  // 记录峰值个数，序列默认序列最右边有一个峰值
		for(int i = 0; i< nums.size() - 1;i++)
		{
			curDiff = nums[i+1] - nums[i];
			if((preDiff <= 0 && curDiff>0) || (preDiff>=0&&curDiff<0)
			{	
				result++;
				preDiff = curDiff;
			}
		}
        }
        return result;
    }
};
```
# 53. Maximum Subarray

```cpp
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
		int count = 0;		
		for(int i = 0; i < nums.size();i++)
		{
			count+=nums[i];
			if(count>result)
			{
				result = count;
			}
			if(count<=0)
			{
				count = 0;
			}
		}
        return result;
    }
```

# 122. Best Time to Buy and Sell Stock II

1. count only positive profit

```cpp
    int maxProfit(vector<int>& prices) {
        int result = 0;
        for (int i = 1; i < prices.size(); i++) {
            result += max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
```

## 55. jump-game i

1. calculate cover area not steps

```cpp
    bool canJump(vector<int>& nums) {
        int cover = 0;
        if (nums.size() == 1) return true; // 只有一个元素，就是能达到
        for (int i = 0; i <= cover; i++) { // 注意这里是小于等于cover
            cover = max(i + nums[i], cover);
            if (cover >= nums.size() - 1) return true; // 说明可以覆盖到终点了
        }
        return false;
```
## 45. jump-game II

1. calculate cover area
2. when reach current available place, update next availabe place by i + nums[i]

```cpp
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int curDistance = 0;    // 当前覆盖最远距离下标
        int ans = 0;            // 记录走的最大步数
        int nextDistance = 0;   // 下一步覆盖最远距离下标
        for (int i = 0; i < nums.size(); i++) {
            nextDistance = max(nums[i] + i, nextDistance);  // 更新下一步覆盖最远距离下标
            if (i == curDistance) {                         // 遇到当前覆盖最远距离下标
                ans++;                                  // 需要走下一步
                curDistance = nextDistance;             // 更新当前覆盖最远距离下标（相当于加油了）
                if (nextDistance >= nums.size() - 1) break;  // 当前覆盖最远距到达集合终点，不用做ans++操作了，直接结束
            }
        }
        return ans;
    }
	
```

# 1005. Maximize Sum Of Array After K Negations

1. sort nums by abs number
2. negative number times -1
3. if k mod 2 equals to one, last number times -1
4. sum up all numbers

```cpp
    static bool cmp(int a, int b)
    {
        return abs(a) > abs(b);
    }
    int largestSumAfterKNegations(vector<int> &nums, int k)
    {
        sort(nums.begin(), nums.end(), cmp);
        for (int i = 0; i < nums.size(); i++)
        {
            if (nums[i] < 0 && k > 0)
            {
                nums[i] *= -1;
                k--;
            }
        }
        if (k % 2 == 1)
        {
            nums[nums.size() - 1] *= -1;
        }
        int result = 0;
        for (int i : nums)
        {
            result += i;
        }
        return result;
    }
};
```

# 134

```cpp
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
                start = i + 1;  // 起始位置更新为i+1
                curSum = 0;     // curSum从0开始
            }
        }
        if (totalSum < 0) return -1; // 说明怎么走都不可能跑一圈了
        return start;

```

# 135

1. compare i and i - 1 from left to right, if i > i - 1, nums[i] + 1
2. compare i and i + 1 from right to left, if i > i + 1, nums[i] + 1

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);
        // 从前向后
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) candyVec[i] = candyVec[i - 1] + 1;
        }
        // 从后向前
        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1] ) {
                candyVec[i] = max(candyVec[i], candyVec[i + 1] + 1);
            }
        }
        // 统计结果
        int result = 0;
        for (int i = 0; i < candyVec.size(); i++) result += candyVec[i];
        return result;
    }
};
```

# 860

```cpp
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0, twenty = 0;
        for (int bill : bills) {
            // 情况一
            if (bill == 5) five++;
            // 情况二
            if (bill == 10) {
                if (five <= 0) return false;
                ten++;
                five--;
            }
            // 情况三
            if (bill == 20) {
                // 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
                if (five > 0 && ten > 0) {
                    five--;
                    ten--;
                    twenty++; // 其实这行代码可以删了，因为记录20已经没有意义了，不会用20来找零
                } else if (five >= 3) {
                    five -= 3;
                    twenty++; // 同理，这行代码也可以删了
                } else return false;
            }
        }
        return true;
    }
```

# 406

1. sort height from tall to short, if a[0] == b[0], sort by smaller between a[1] and b[1]
2. insert height by its index a[1]

```cpp
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        vector<vector<int>> que;
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
```

# 452 

1. if point[i] left > point[i-1] right(not overlapping) arrow+1, otherwise(point[i] left <= point[i-1] right) two points are overlapping, update point[i] right

```cpp
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.size() == 0) return 0;
        sort(points.begin(), points.end(), cmp);

        int result = 1; // points 不为空至少需要一支箭
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
                result++; // 需要一支箭
            }
            else {  // 气球i和气球i-1挨着
                points[i][1] = min(points[i - 1][1], points[i][1]); // 更新重叠气球最小右边界
            }
        }
        return result;
    }

```

# 435

## method 1

1. sort by a[0] and b[0]
2. if [i]left < [i-1]right, two intervals are overlapping, count+1 and update interval[i] right

```cpp
    static bool cmp (const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0]; // 改为左边界排序
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int count = 0; // 注意这里从0开始，因为是记录重叠区间
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < intervals[i - 1][1]) { //重叠情况
                intervals[i][1] = min(intervals[i - 1][1], intervals[i][1]);
                count++;
            }
        }
        return count;
    }
```

## method 2 - like 452

1. every arrow ocunted means one isolated interval
2. answer is total size - arrows
```cpp
    static bool cmp (const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1]; // 右边界排序 
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), cmp);

        int result = 1; // points 不为空至少需要一支箭
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] >= intervals[i - 1][1]) {
                result++; // 需要一支箭
            }
            else {  // 气球i和气球i-1挨着
                intervals[i][1] = min(intervals[i - 1][1], intervals[i][1]); // 更新重叠气球最小右边界
            }
        }
        return intervals.size() - result;
    }
```

# 763

1. count farest position of every character
2. set right to farest position of every sub-string and count length
```cpp
        int hash[27] = {0}; // i为字符，hash[i]为字符出现的最后位置
        for (int i = 0; i < S.size(); i++) { // 统计每一个字符最后出现的位置
            hash[S[i] - 'a'] = i;
        }
        vector<int> result;
        int left = 0;
        int right = 0;
        for (int i = 0; i < S.size(); i++) {
            right = max(right, hash[S[i] - 'a']); // 找到字符出现的最远边界
            if (i == right) {
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
```

# 56

1. sort by nums[0]
2. push first element to vector
3. if intervals[i]right >= intervals[i+1]left, update right edge

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if (intervals.size() == 0) return result; // 区间集合为空直接返回
        // 排序的参数使用了lambda表达式
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b){return a[0] < b[0];});

        // 第一个区间就可以放进结果集里，后面如果重叠，在result上直接合并
        result.push_back(intervals[0]); 

        for (int i = 1; i < intervals.size(); i++) {
            if (result.back()[1] >= intervals[i][0]) { // 发现重叠区间
                // 合并区间，只更新右边界就好，因为result.back()的左边界一定是最小值，因为我们按照左边界排序的
                result.back()[1] = max(result.back()[1], intervals[i][1]); 
            } else {
                result.push_back(intervals[i]); // 区间不重叠 
            }
        }
        return result;
    }
};
```

# 738

1. from end to begin, find last nums[i-1]>nums[i], nums[i-1] minus one
2. set all numbers after index to 9

```cpp
    int monotoneIncreasingDigits(int n)
    {
        string strNum = to_string(n);
        int index = strNum.size();
        for (int i = strNum.size() - 1; i >= 1; i--)
        {
            if (strNum[i] < strNum[i - 1])
            {
                index = i;
                strNum[i - 1]--;
            }
        }
        for (int i = index; i < strNum.size(); i++)
        {
            strNum[i] = '9';
        }
        return stoi(strNum);
    }
```

# 968 
1. status zero means not cover, status 1 means this node has camera and status 2 means this node is covered by camera
2. set leaf to 0 so the root of the leaf will be 1
3. if node is 1 the root of that node is 2 means that the node is covered

```cpp
    int result = 0;
    int traversal(TreeNode *cur)
    {
        if (cur == NULL)
        {
            return 2;
        }
        int left = traversal(cur->left);
        int right = traversal(cur->right);

        if (left == 2 && right == 2)
        {
            return 0;
        }
        if (left == 0 || right == 0)
        {
            result++;
            return 1;
        }
        if (left == 1 || right == 1)
        {
            return 2;
        }
        return -1;
    }
    int minCameraCover(TreeNode *root)
    {
        if (traversal(root) == 0)
        {
            result++;
        }
        return result;
    }
```