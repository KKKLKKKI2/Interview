# Redo
- 33
- 81

# Key

1. Binary search
	1. **[Left, Right]**，左閉右閉，while(left<=right)，left = 0，right = size - 1
		- 當(target<mid) right = mid - 1 
		- 當(target>mid) left = mid + 1
	
	2. **[Left, Right)**，左閉右開，while(left< right)，left = 0，right = size
		- 當(target<mid) right = mid
		- 當(target>mid) left = mid + 1

# 33

```cpp
class Solution
{
public:
    int search(vector<int> &nums, int target)
    {
        int left = 0;
        int right = nums.size() - 1;
        int mid;

        while (left < right)
        {
            mid = left + (right - left) / 2;
            if (nums[mid] == target)
            {
                return mid;
            }
            if (nums[left] > target && nums[left] > nums[mid])
            {
                if (target > nums[mid])
                {
                    left = mid + 1;
                }
                else
                {
                    right = mid;
                }
            }
            else if (nums[left] > target && nums[left] <= nums[mid])
            {
                left = mid + 1;
            }
            else if (nums[left] <= target && nums[left] > nums[mid])
            {
                right = mid;
            }
            else if (nums[left] <= target && nums[left] <= nums[mid])
            {
                if (target > nums[mid])
                {
                    left = mid + 1;
                }
                else
                {
                    right = mid;
                }
            }
        }
        if (nums[left] == target)
            return left;
        return -1;
    }
};
```

# 81

```cpp
class Solution
{
public:
    bool search(vector<int> &nums, int target)
    {
        int left = 0;
        int right = nums.size() - 1;
        int mid;
        while (left < right)
        {
            while (left + 1 < right && nums[left] == nums[left + 1])
                left++;
            mid = left + (right - left) / 2;
            if (nums[left] == target || nums[mid] == target || nums[right] == target)
            {
                return true;
            }

            if (nums[left] >= target && nums[left] >= nums[mid])
            {
                if (target >= nums[mid])
                {
                    left = mid + 1;
                }
                else
                {
                    right = mid - 1;
                }
            }
            else if (nums[left] < target && nums[left] < nums[mid])
            {
                if (target >= nums[mid])
                {
                    left = mid + 1;
                }
                else
                {
                    right = mid - 1;
                }
            }
            else if (nums[left] < nums[mid] && nums[left] >= target)
            {
                left = mid + 1;
            }
            else if (nums[left] >= nums[mid] && nums[left] < target)
            {
                right = mid - 1;
            }
        }
        if (nums[left] == target)
            return true;

        return false;
    }
};
```