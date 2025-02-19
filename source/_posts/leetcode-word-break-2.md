---
title: leetcode140实践
date: 2024-03-20 17:05:40
tags:  leetcode
---
# LeetCode 140. 单词拆分 II

LeetCode 140. 单词拆分 II 是一道经典的动态规划与回溯结合的题目。题目要求给定一个非空字符串 `s` 和一个包含非空单词列表的字典 `wordDict`，要求将字符串 `s` 拆分成由字典中的单词组成的句子，并返回所有可能的句子。

## 示例

**输入:**
s = "catsanddog"

wordDict = ["cat", "cats", "and", "sand", "dog"]


**输出:**
[

"cats and dog",

"cat sand dog"

]


## 解题思路

这道题可以分解为两个部分：

1. 判断字符串是否可以拆分成字典中的单词：这一步可以通过动态规划来解决。
2. 生成所有可能的拆分方式：这一步可以通过回溯算法来解决。

### 1. 动态规划判断是否可以拆分

我们可以使用动态规划来判断字符串 `s` 是否可以拆分成字典中的单词。定义一个布尔数组 `dp`，其中 `dp[i]` 表示字符串 `s` 的前 `i` 个字符是否可以拆分成字典中的单词。

- 初始化 `dp[0] = True`，表示空字符串可以被拆分。
- 对于每个位置 `i`，遍历所有可能的 `j`（`0 <= j < i`），如果 `dp[j]` 为 `True` 且 `s[j:i]` 在字典中，则 `dp[i] = True`。

### 2. 回溯生成所有可能的拆分方式

在确定了字符串可以被拆分之后，我们可以使用回溯算法来生成所有可能的拆分方式。具体步骤如下：

- 从字符串的起始位置开始，尝试所有可能的拆分点。
- 如果当前子串在字典中，则递归处理剩余部分。
- 当处理到字符串末尾时，将当前的拆分方式加入结果集。

## 代码实现

```python
from typing import List

class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        # 将字典转换为集合，方便快速查找
        wordSet = set(wordDict)
        n = len(s)
        
        # 动态规划判断是否可以拆分
        dp = [False] * (n + 1)
        dp[0] = True
        
        for i in range(1, n + 1):
            for j in range(i):
                if dp[j] and s[j:i] in wordSet:
                    dp[i] = True
                    break
        
        # 如果无法拆分，直接返回空列表
        if not dp[n]:
            return []
        
        # 回溯生成所有可能的拆分方式
        result = []
        self.backtrack(s, wordSet, 0, [], result)
        return result
    
    def backtrack(self, s, wordSet, start, path, result):
        if start == len(s):
            result.append(" ".join(path))
            return
        
        for end in range(start + 1, len(s) + 1):
            word = s[start:end]
            if word in wordSet:
                self.backtrack(s, wordSet, end, path + [word], result)
```

## 代码解释
### 动态规划部分：

1. 我们首先将 wordDict 转换为集合 wordSet，以便快速查找。
2. 然后我们初始化 dp 数组，dp[0] 为 True，表示空字符串可以被拆分。
3. 对于每个位置 i，我们遍历所有可能的 j，如果 dp[j] 为 True 且 s[j:i] 在 wordSet 中，则 dp[i] 为 True。

### 回溯部分：

1. 我们从字符串的起始位置开始，尝试所有可能的拆分点。
2. 如果当前子串在 wordSet 中，则递归处理剩余部分。
3. 当处理到字符串末尾时，将当前的拆分方式加入结果集。