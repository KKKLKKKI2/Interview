# Binary Seach

## Find unique

[left, right]

```cpp
int left = 0;
int right = nums.size() - 1;

while(left<=right)
{
	int mid = left + (right - left) / 2;
	if(nums[mid] == target) return mid;
	else if(nums[mid] > target)
	{
		right = mid - 1;
	}
	else if(nums[mid] < target)
	{
		left = mid + 1;
	}
	
	return -1;	
}
```

## Find left most

[left,right]

```cpp
int left = 0;
int right = nums.size() - 1;

while(left<=right)
{
	int mid = left + (right - left) / 2;
	if(nums[mid] > target)
	{
		right = mid - 1;
	}
	else if(nums[mid] < target)
	{
		left = mid + 1;
	}
	else if(nums[mid] == target)
	{
		right = mid - 1;
	}
	
	if( left >= nums.size() || nums[left] != target)
		return - 1;
	return left;
}
```

\[left, right\)

```cpp
int left = 0;
int right = nums.size();

while(left < right)
{
	int mid = left + ( right - left ) / 2;
	if(nums[mid] > target)
	{
		left = mid + 1;
	}
	else if(nums[mid] < target)
	{
		right = mid;
	}
	else if(nums[mid] == target)
	{
		right = mid ;
	}
}

if(left >= nums.size() || nums[left] != target)return -1;


return right;

```


## find right most

[left,right]

```cpp
int left = 0;
int right = nums.size() - 1;

while(left<=right)
{
	int mid = left + ( right - left ) / 2;
	if(nums[mid] > target)
	{
		left = mid + 1;
	}
	else if(nums[mid] < target)
	{
		right = mid - 1;
	}
	else if(nums[mid] == target)
	{
		left = mid - 1;
	}
}

if(right < 0 || nums[right] != target)
	return -1;
	

return right;
```

\[left, right\)

```cpp
int left = 0;
int right = nums.size();

while(left < right)
{
	int mid = left + ( right - left ) / 2;
	if(nums[mid] > target)
	{
		left = mid + 1;
	}
	else if(nums[mid] < target)
	{
		right = mid;
	}
	else if(nums[mid] == target)
	{
		left = mid + 1;
	}
}

if(right < 0 || nums[left] != target)return -1;
return left - 1;

```


