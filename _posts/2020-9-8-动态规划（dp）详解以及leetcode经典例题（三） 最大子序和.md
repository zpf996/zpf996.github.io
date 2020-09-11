---
layout:     post
title:      动态规划（dp）详解以及leetcode经典例题（三） 最大子序和
subtitle:   算法
date:       2020-7-31
author:     leon7777
header-img: img/user.jpg
catalog: 	 true
tags:
    - c
    - 动态规划
    - leetcode
---
# 动态规划（dp）详解以及leetcode经典例题（三） 最大子序和
## 题目
```
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
```
示例
```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
## 分析
```
观察这道题，发现是动态规划，就按照老规矩来
1、寻找状态转移方程，设数组dp[]
    对于一个数来说dp[0]=nums[0];
    对于二个数来说dp[1]=max(nums[1],dp[0]+nums[1]);
    对于三个数来说dp[2]=max(nums[2],dp[1]+nums[2]);
    对于i个数来说dp[i]=max(nums[i],dp[i-1]+nums[i]);
    通过类比的方法得出状态转移方程
2、找出动态规划界限问题
    只有一个数，没有数
接下来开始写代码
```
## 代码
```c
int maxSubArray(int* nums, int numsSize){
    if(!nums||numsSize==0)
    {
        return 0;
    }
    if(numsSize==1)
    {
        return nums[0];
    }
    int dp[numsSize];
    int max=nums[0];
    dp[0]=nums[0];
    for(int i=1;i<numsSize;i++)
    {
        dp[i]=nums[i]>dp[i-1]+nums[i]?nums[i]:dp[i-1]+nums[i];
        if(max<dp[i])
        {
            max=dp[i];
        }
    }
    return max;
}
```