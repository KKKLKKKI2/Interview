# Merge Sort

## Pros
- O(nlogn) worst case asymptotic complexity.

## Cons


## Code 
```cpp
void combineMergeResult(vector<int>&nums,int start,int end ,int mid)
{
    vector<int> leftPart(nums.begin() + start, nums.begin() + mid + 1);
    vector<int> rightPart(nums.begin() + mid + 1, nums.begin() + end + 1);
    
    int mergeIndex = start; 
    int leftIndex = 0;
    int rightIndex = 0;
     
    while(leftIndex < leftPart.size() && rightIndex < rightPart.size()) 
    {
       if(leftPart[leftIndex] <= rightPart[rightIndex]) 
       {
           nums[mergeIndex] = leftPart[leftIndex];
           leftIndex++;
       }
       else
       {
           nums[mergeIndex] = rightPart[rightIndex];
           rightIndex++;
       }
       mergeIndex++;
    }
    
    while(leftIndex<leftPart.size())
    {
        nums[start+leftIndex+rightIndex] = leftPart[leftIndex];
        leftIndex++;
    }
    
    while(rightIndex<rightPart.size())
    {
        nums[start+leftIndex+rightIndex] = rightPart[rightIndex];
        rightIndex++;
    }
}

void mergeSort(vector<int>&nums, int start, int end)
{
    if(start < end)
    {
        int mid = start + (end - start) / 2;
        mergeSort(nums, start, mid);        
        mergeSort(nums, mid + 1, end);
        combineMergeResult(nums,start,end,mid); 
    }
}

int main()
{
    vector<int> nums = {9, 100,25,5,4,6,11,27,88,49,50,44,35,11,21};
    mergeSort(nums,0,nums.size()-1);
}
```

# QuickSort

## Pros

## Cons

## Code
```cpp
int findPartition(vector<int>&nums, int start, int end)
{
    int i = start -1;
    int pivot = nums[end];
    for(int j = start;j<end;j++)
    {
       if(nums[j] < pivot) 
       {
           i++;
           swap(nums[i], nums[j]);
       }
       
    }
    i++;
    swap(nums[i], nums[end]);
    return i;
}

void quickSort(vector<int>&nums, int start, int end)
{
    if(start<end)
    {
        int partition = findPartition(nums,start,end);
        quickSort(nums, start, partition - 1);
        quickSort(nums, partition + 1, end);
    }
}

int main()
{  
    vector<int> nums = {9, 100,25,5,4,6,11,27,88,49,50,44,35,11,21};
    quickSort(nums,0,nums.size()-1);
    return 0;
}
```