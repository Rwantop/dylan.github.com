---
title: leetcode139实践
date: 2024-03-19 17:51:41
tags: leetcode
---
## 题目描述

给定一个非空字符串 `s` 和一个包含非空单词列表的字典 `wordDict`，判断 `s` 是否可以被拆分为一个或多个字典中的单词。

**示例 1:**

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: "leetcode" 可以被拆分为 "leet" 和 "code"。

**示例 2:**

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: "applepenapple" 可以被拆分为 "apple"、"pen" 和 "apple"。

**示例 3:**

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
解释: "catsandog" 无法被拆分为字典中的单词。

## 解题思路

这道题可以使用动态规划来解决。我们可以定义一个布尔数组 `dp`，其中 `dp[i]` 表示字符串 `s` 的前 `i` 个字符是否可以被拆分为字典中的单词。

### 动态规划步骤

1. **初始化**：`dp[0] = True`，表示空字符串可以被拆分。
2. **状态转移**：对于每个位置 `i`，我们遍历所有可能的前缀 `s[j:i]`，如果 `dp[j]` 为 `True` 且 `s[j:i]` 在字典中，那么 `dp[i]` 也为 `True`。
3. **最终结果**：`dp[len(s)]` 即为所求。

### 代码实现

```python

def wordBreak(s, wordDict):
    word_set = set(wordDict)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[len(s)]
	
```

### 代码解释

1. word_set = set(wordDict)：将字典转换为集合，方便快速查找。
2. dp = [False] * (len(s) + 1)：初始化 dp 数组，长度为 len(s) + 1，初始值为 False。
3. dp[0] = True：空字符串可以被拆分。
4. 双重循环：外层循环遍历字符串的每个位置 i，内层循环遍历所有可能的前缀 s[j:i]。
5. if dp[j] and s[j:i] in word_set：如果 dp[j] 为 True 且 s[j:i] 在字典中，那么 dp[i] 也为 True。
return dp[len(s)]：返回最终结果。