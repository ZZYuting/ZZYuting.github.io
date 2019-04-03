---
title: 79Longest Increasing Subsequence
layout: post
categories: 动态规划
excerpt: 
Tags: leetcode
---

给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(*n2*) 。

**进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?

```c++
//https://www.youtube.com/watch?v=fV-TF4OvZpk
//time:o(n^2)
//space:o(n)
//Idex 	0 1 2 3
//     [1 2 3 4]
//	   0-1:1 check
//     0-2:2 check
//     0-3:3 check
//     0-4:4 check
//	   n*O(n)
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.empty()){
            return 0;
        }
        vector<int> tmp(nums.size(),1);
        for(int i=1;i<nums.size();i++){
            for(int j=0;j<i;j++){
                if(nums[j]<nums[i]){
                    tmp[i]=max(tmp[i],tmp[j]+1);
                }
            }
        }
        return *max_element(tmp.begin(),tmp.end());
    }
};
```

