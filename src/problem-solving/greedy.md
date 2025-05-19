# Greedy Problems

**Find Subarray with Maximum Sum**

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSubArray = nums[0];
        int minPrefixSum = 0, curPrefixSum = 0;
        for (int num: nums) {
            curPrefixSum += num;
            maxSubArray = max(maxSubArray, curPrefixSum - minPrefixSum);
            minPrefixSum = min(minPrefixSum, curPrefixSum);
        }
        return maxSubArray;
    }
};
```

**Jump Game**

Given an integer array nums where you start at index 0 and nums[i] represents your maximum jump length at position i,
return true if you can reach the last index, false otherwise.

```cpp

```

**Jump Game II**

Given an integer array nums where you start at index 0 and nums[i] represents your maximum jump length at position i,
return the minimum number of jumps required to reach the last index.

```cpp

```

**Gas Station**

```cpp

```


