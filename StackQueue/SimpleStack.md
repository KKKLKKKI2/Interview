# Practice again

# 739

Stack store index.
When new index is greater than st.top(), pop st and record index

```cpp
class Solution
{
public:
    vector<int> dailyTemperatures(vector<int> &temperatures)
    {
        stack<int> st;
        st.push(0);
        vector<int> result(temperatures.size(), 0);
        for (int i = 1; i < temperatures.size(); i++)
        {
            while (temperatures[i] > temperatures[st.top()])
            {
                result[st.top()] = i - st.top();
                st.pop();
                if (st.empty())
                {
                    break;
                }
            }
            st.push(i);
        }

        return result;
    }
};
```
# 503

map save next greater number index

```cpp
class Solution
{
public:
    vector<int> nextGreaterElement(vector<int> &nums1, vector<int> &nums2)
    {
        unordered_map<int, int> mp;
        vector<int> result(nums1.size(), -1);
        stack<int> st;
        st.push(0);
        for (int i = 1; i < nums2.size(); i++)
        {
            while (nums2[i] > nums2[st.top()])
            {
                mp[nums2[st.top()]] = nums2[i];
                st.pop();
                if (st.empty())
                {
                    break;
                }
            }
            st.push(i);
        }

        for (int i = 0; i < nums1.size(); i++)
        {
            if (mp.find(nums1[i]) != mp.end())
                result[i] = mp[nums1[i]];
        }
        return result;
    }
};
```

# 503

Traversal nums twice, there are two ways
1. add nums again at nums end, nums + nums
2. simulate traversal nums twice by traversal once from 0 to nums.size() times 2, every iterator must mod by nums size

```cpp
class Solution
{
public:
    vector<int> nextGreaterElements(vector<int> &nums)
    {
        vector<int> result(nums.size(), -1);
        stack<int> st;
        st.push(0);
        for (int i = 1; i < nums.size() * 2; i++)
        {
            while (nums[i % nums.size()] > nums[st.top() % nums.size()])
            {
                result[st.top() % nums.size()] = nums[i % nums.size()];
                st.pop();
                if (st.empty())
                {
                    break;
                }
            }
            st.push(i);
        }
        return result;
    }
};
```

# 42 

## method 1 two pointer
leftHeight save left heightest elevation between itself and left - 1, rightHeight is the same
For each elevation, find minimum (leftHeight, rightHeight) that can become a cave, the water can be trapped is 1 times minimum height

```cpp
class Solution
{
public:
    int trap(vector<int> &height)
    {
        int result = 0;
        vector<int> leftHeight(height.size(), 0);
        vector<int> rightHeight(height.size(), 0);
        leftHeight[0] = height[0];
        rightHeight[height.size() - 1] = height[height.size() - 1];
        for (int i = 1; i < height.size(); i++)
        {
            leftHeight[i] = max(height[i], leftHeight[i - 1]);
        }
        for (int i = height.size() - 2; i >= 0; i--)
        {
            rightHeight[i] = max(height[i], rightHeight[i + 1]);
        }
        int cur = 0;
        for (int i = 1; i < height.size() - 1; i++)
        {
            cur = min(leftHeight[i], rightHeight[i]) - height[i];
            if (cur > 0)
                result += cur;
        }
        return result;
    }
};
```

## method 2 row based

Sample: 3213(cur) 
		
If new hieght is greater than st.top()
- New hieght is right high elevation
- Set st.top() to mid and pop out
- st.top() is now left high elevation

Calculate row based trapped rain(213)
- Width is 3 - 2 - 1(left and right don't count)
- height is min(2,3)
- row based trapped rain is 2 * 1

Then move mid to 2 so left elevation is 3 now



```cpp
class Solution
{
public:
    int trap(vector<int> &height)
    {
        int result = 0;
        stack<int> st;
        st.push(0);

        for (int i = 1; i < height.size(); i++)
        {
            while (!st.empty() && height[i] > height[st.top()])
            {
                int mid = st.top();
                st.pop();
                if (!st.empty())
                {
                    int h = min(height[i], height[st.top()]) - height[mid];
                    int w = i - st.top() - 1;
                    result += h * w;
                }
            }
            st.push(i);
        }

        return result;
    }
};
```