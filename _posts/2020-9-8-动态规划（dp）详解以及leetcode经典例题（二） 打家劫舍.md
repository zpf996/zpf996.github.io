---
layout:     post
title:      动态规划（dp）详解以及leetcode经典例题（二） 打家劫舍
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
# 动态规划（dp）详解以及leetcode经典例题（二） 打家劫舍
## 题目
```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```
## 分析
```
动态规划的题，都需要先分析题目，从中提炼出状态转移方程
这道题告诉我们相邻的两件房子连接报警器，只能偷一间，并且需要偷到最大金额
解决的问题不偷相邻的两间房子的情况下的最大金额
分解为小问题
1、如果只有一间，则只能偷这间，dp[1]=nums[1]
2、如果只有二间，则偷金额最大的那间 dp[2]=dp[1]>nums[2]?dp[1]:nums[2];
3、如果只有三间，则需要判断偷第一间和第三间的金额是否大于偷第二间 dp[3]=max(dp[1]+nums[3],dp[2]);
4、如果只有k间，则为dp[k]=max(dp[k-2]+nums[k],dp[k-1])

这样便列出了这到题的状态转移方程 dp[k]=max(dp[k-2]+nums[k],dp[k-1])
最后要注意动态规划题的界限问题
```
## 代码
```c
int rob(int* nums, int numsSize)
{
    if(numsSize==0||!nums)
    {
        return 0;
    }
    if(numsSize==1) return nums[0];
    int dp[numsSize];
    dp[0]=nums[0];
    dp[1]=fmax(nums[0],nums[1]);
    for(int i=2;i<numsSize;i++)
    {
        dp[i]=dp[i-1]>dp[i-2]+nums[i]?dp[i-1]:dp[i-2]+nums[i];
    }
    return dp[numsSize-1];
}
```