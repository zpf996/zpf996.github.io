---
layout:     post
title:      动态规划（dp）详解以及leetcode经典例题（四） 零钱兑换
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
# 动态规划（dp）详解以及leetcode经典例题（四） 零钱兑换 
## 题目
```
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

```
示例
```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```
示例2
```
输入: coins = [2], amount = 3
输出: -1
```
## 分析
```
动态规划题，老规矩推倒状态转移方程
设dp[n]为amount=n时需要的硬币个数
1、找出子问题（自顶向下）
    如果amount=11，分解为小问题为知道amount=10的个数+1
    以此得出dp[n]=dp[n-1]+1;
    此时还有一个最少的硬币数的条件，暴力法将硬币数组循环查一遍
    即dp[n-coins[i]]+1
    此时状态转移方程为 dp[n]=min(dp[n],dp[n-coins[i]+1])
2、边界问题
    dp[0]=0
    dp[1]=1
```
## 代码
```c
int coinChange(int* coins, int coinsSize, int amount){
    if(!coins||coinsSize==0) return -1;
    int dp[amount+1]; ///amount+1钱数
    int i;
    for(i=0;i<=amount;i++)
    {
        dp[i]=amount+1;    //初始化
    }
    dp[0]=0;
    for(i=1;i<=amount;i++)
    {
        for(int j=0;j<coinsSize;j++)
        {
            if(i-coins[j]<0) continue;  //钱数小于0跳过
            dp[i]=dp[i]<dp[i-coins[j]]+1?dp[i]:dp[i-coins[j]]+1;
        }
    }
    return (dp[amount]==amount+1)?-1:dp[amount]; //如果dp[amount]==amount+1说明amount-1找不到一个硬币额度加起来等于amount，不符合条件
    
}
```