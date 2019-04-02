---
title: 73 Find First and Last Position of Element in Sorted Array
layout: post
categories: 排序和搜索
excerpt: 
Tags: leetcode
---

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

**示例 2:**

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

```c++
//二分搜索，o(logn)
//https://www.youtube.com/watch?v=gOkNq8Co6B8
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int i=0,j=nums.size()-1;
        vector<int> ret(2,-1);
        if(nums.empty())
            return ret;
        while(i<j){
            int mid=i+(j-i)/2;
            if(nums[mid]<target){
                i=mid+1;
            }
            else{
                j=mid;
            }
        }
        if(nums[i]!=target){
            return ret;
        }
        ret[0]=i;
        j=nums.size()-1;
        while(i<j){
            int mid=i+(j-i)/2+1;
            if(nums[mid]>target){
                j=mid-1;
            }
            else{
                i=mid;
            }
        }
        ret[1]=j;
        return ret;
    }
};
```

