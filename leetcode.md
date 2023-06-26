# Key

1. 可能出界的地方要先判斷
use dummy head
```cpp
	while(nums[left] == nums[left+1] && left < right)  //nums[left+1]可能會出界
	while(left < right && nums[left] == nums[left+1])
```

2. 

